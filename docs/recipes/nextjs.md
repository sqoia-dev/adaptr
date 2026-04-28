# Recipe: Next.js Static Export behind a Subpath

Tested against **Next.js 16 (Turbopack)** with `output: 'export'`, both Pages
Router and App Router.

---

## Prerequisites

Your Next.js app must be configured for static export:

```ts
// next.config.ts
const nextConfig: NextConfig = {
  output: "export",
  images: {
    unoptimized: true, // required: next/image optimization requires a server
  },
};
export default nextConfig;
```

Do **not** set `basePath` in `next.config`. adaptr handles the subpath at the
proxy layer. Combining Next.js `basePath` with adaptr's `BASE_PATH` produces
double-prefixed paths and breaks asset loading.

---

## Quick Start

```bash
# 1. Build your Next.js app
npm run build
# Output lands in ./out/

# 2. Serve the static files (any static server works)
docker run -d --name nextjs-app \
  -v ./out:/usr/share/nginx/html:ro \
  -v ./nginx.conf:/etc/nginx/conf.d/default.conf:ro \
  nginx:alpine

# 3. Run adaptr in front of it
docker run -d \
  -e TARGET=nextjs-app:80 \
  -e BASE_PATH=/myapp \
  -p 8080:8080 \
  --network your-network \
  ghcr.io/sqoia-dev/adaptr:latest
```

**Nginx config for Next.js static export** (`nginx.conf`):

```nginx
server {
    listen 80;
    root /usr/share/nginx/html;
    index index.html;

    # Try .html extension for extensionless paths (Next.js Pages Router pattern)
    location / {
        try_files $uri $uri.html $uri/index.html =404;
    }

    # Cache immutable hashed assets
    location /_next/static/ {
        alias /usr/share/nginx/html/_next/static/;
        expires max;
        add_header Cache-Control "public, max-age=31536000, immutable";
    }
}
```

---

## What Works

Tested and verified with `BASE_PATH=/myapp`:

| Feature | Pages Router | App Router | Notes |
|---|---|---|---|
| Initial page load | ✅ | ✅ | `<base href="/myapp/">` injected, `/_next/` paths rewritten to relative |
| Static pages | ✅ | ✅ | Direct navigation via adaptr |
| Dynamic routes (`/blog/[slug]`) | ✅ | ✅ | Static HTML pre-generated via `getStaticPaths` / `generateStaticParams` |
| Client-side navigation | ✅ | ✅ | `pushState` patched; RSC payloads (`.txt` files) fetched via adaptr `fetch` interceptor |
| Lazy chunk loading | ✅ | ✅ | Turbopack inserts `<script src="/_next/...">` — adaptr's MutationObserver rewrites before network request |
| SSG data fetches | ✅ | N/A | `/_next/data/` JSON fetches rewritten by `fetch` interceptor |
| RSC page transitions | N/A | ✅ | `.txt` payloads fetched at relative paths, served by adaptr |
| CSS, fonts, images | ✅ | ✅ | HTML attribute rewrite handles all static asset paths |

---

## Known Limitations

- **ISR and SSR are not supported.** These require a running Next.js server
  (`next start`). adaptr works with static export (`output: 'export'`) only.
  If your app uses `getServerSideProps` or incremental static regeneration,
  static export will fail at build time.

- **Do not combine adaptr `BASE_PATH` with Next.js `basePath`.** If Next.js is
  built with `basePath: '/myapp'`, the static HTML will have paths like
  `/myapp/_next/static/...`. Adaptr would then rewrite these to
  `./myapp/_next/...`, which with `<base href="/myapp/">` resolves to
  `/myapp/myapp/_next/...` — double prefix, 404s. Use one or the other:
  - Use adaptr with `BASE_PATH=/myapp` and build Next.js without `basePath`.
  - Use Next.js `basePath: '/myapp'` and no adaptr.

- **`next/image` optimization API requires a server.** Set `images: { unoptimized: true }`
  in `next.config` when exporting statically. The static export build will fail
  otherwise.

- **Turbopack runtime base path is not rewritten in JS bundles.** The turbopack
  runtime stores `t="/_next/"` as a hardcoded constant. adaptr does not rewrite
  this (it would require matching a minified local variable). Instead, adaptr's
  MutationObserver intercepts dynamic `<script src="/_next/...">` DOM insertions
  (the mechanism turbopack uses for lazy chunk loading) and prepends the base path
  before the browser issues the network request. This is correct behavior in all
  modern browsers (MutationObserver fires as a synchronous microtask before I/O).

---

## How It Works

When adaptr serves the Next.js HTML response:

1. `<base href="/myapp/">` is injected as the first child of `<head>`.
2. All `src="/_next/..."` and `href="/_next/..."` attributes are rewritten to
   `src="./_next/..."` — relative paths that resolve correctly via `<base href>`.
3. The adaptr runtime interceptor script is injected. It patches `fetch`, `XHR`,
   `pushState`, and `popstate`. When the Next.js router fetches RSC payloads
   (`fetch("/about.txt")`), the interceptor prepends the base path
   (`/myapp/about.txt`). adaptr receives the request, strips `/myapp`, and
   forwards `/about.txt` to the static file server.
4. The MutationObserver watches for dynamically inserted `<script>` and `<link>`
   elements. When turbopack's runtime loads a lazy code-split chunk by appending
   `<script src="/_next/static/chunks/foo.js">`, the observer fires synchronously,
   rewrites `src` to `/myapp/_next/static/chunks/foo.js`, and the browser requests
   the correct URL.

---

## Next.js Pages Router vs App Router

Both work. The difference is how client-side navigation data is fetched:

- **Pages Router**: fetches SSG data from `/_next/data/{buildId}/page.json`.
  adaptr's `fetch` interceptor rewrites `/_next/data/...` to `/myapp/_next/data/...`.
- **App Router**: fetches RSC payloads from `/{path}.txt` (root-relative path,
  no `/_next/` prefix). adaptr's `fetch` interceptor rewrites `/about.txt` to
  `/myapp/about.txt`. The static file server must be configured to serve
  extensionless paths with the `.html` suffix (see nginx config above).
