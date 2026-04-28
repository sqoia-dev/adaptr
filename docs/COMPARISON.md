# adaptr vs. Alternatives — SPA Subpath Deployment Comparison

**Last updated:** 2026-04-27
**adaptr version:** v0.4.0

This table compares adaptr against the five approaches teams actually use when they
need to deploy a SPA at a subpath prefix (e.g., `/myapp/` instead of `/`).

---

## The Problem Being Solved

A SPA built with the default `base: '/'` embeds absolute paths everywhere —
`<script src="/assets/main.js">`, `fetch('/api/data')`,
`navigator.serviceWorker.register('/sw.js')`. When a reverse proxy serves that app
at `/myapp/`, the browser resolves all of these against the domain root and gets
404s. There are three distinct failure layers:

1. **Static asset paths** — `src="/..."` and `href="/..."` in HTML/CSS resolve against the domain root
2. **Client-side routing** — React Router / Vue Router initialise from `window.location.pathname`, which includes the subpath prefix
3. **Runtime-constructed URLs** — `fetch`, `XMLHttpRequest`, `WebSocket`, `EventSource` are assembled dynamically in JS and bypass any static rewrite

Each approach below addresses a different subset of these three layers.

---

## Comparison Table

| Capability | adaptr | nginx `sub_filter` | Vite `base` config | Caddy `replace_response` | Cloudflare Workers | No solution (hand-roll) |
|---|---|---|---|---|---|---|
| **Rewrites absolute paths in HTML** (`src`, `href`, `action`) | Yes | Partial — string match only, no structural awareness | Yes (build-time) | Partial — string match only | Partial — depends on implementation | Manual |
| **Rewrites absolute paths in CSS** (`url()`, `@import`) | Yes | With `sub_filter_types text/css` — fragile | Yes (build-time) | With plugin — fragile | Partial | Manual |
| **Rewrites paths in JS bundles** (webpack `__webpack_require__.p`, Vite `assetsURL`) | Yes — framework-detected, targeted replacement | No — would corrupt unrelated strings | Yes (build-time) | No | Partial | Manual |
| **Rewrites sourcemap URLs** (`//# sourceMappingURL=`) | Yes | No | Yes (build-time) | No | No | Manual |
| **Rewrites JSON / asset manifests** (`manifest.json`, import maps) | Yes | With `sub_filter_types *` — unsafe | Yes (build-time) | No | Partial | Manual |
| **Service worker patching** (`navigator.serviceWorker.register`) | Yes — patches scope + script URL | No | Yes (build-time) | No | Partial | Manual |
| **Runtime URL interception** (`fetch`, `XHR`, `WebSocket`, `EventSource`) | Yes — injected interceptor script | No — static text replacement only | Yes (build-time, compile-time injection) | No | Partial | Manual |
| **React Router / Vue Router integration** (`pushState`/`popstate` patching) | Yes | No | Yes (build-time) | No | No | Manual |
| **Handles gzip-compressed responses** (decompress, rewrite, recompress) | Yes | No — compressed responses bypass sub_filter without `gunzip` module | N/A — build-time | No — depends on module | Yes | N/A |
| **Handles brotli-compressed responses** | Partial — strips `br` from `Accept-Encoding` so upstream serves gzip or uncompressed; does not produce brotli output | No | N/A — build-time | No | Yes | N/A |
| **Multiple SPAs at different subpaths** | Yes — one container per SPA; compose as needed | Yes — multiple `location` blocks | No — each build is locked to one base | Yes — multiple virtual hosts | Yes | Manual |
| **Runtime configurable** (no app rebuild, no server restart) | Yes — `TARGET` and `BASE_PATH` are env vars; restart the container | No — requires nginx reload | No — requires rebuild | No — requires Caddy reload | Yes — deploy script change | N/A |
| **Helm chart included** | Yes | No | No | No | No | No |
| **Single binary / single container** | Yes — one Docker image, zero external deps | No — nginx + modules | No — requires build toolchain | No — Caddy + module | No — requires CF account + Workers subscription | No |
| **Pure stdlib Go** (no runtime dependencies) | Yes | N/A | N/A | N/A | N/A | N/A |
| **Free / open source** | Yes (MIT) | Yes (BSD-like) | Yes (MIT) | Yes (Apache 2.0) | No — paid above free tier | N/A |
| **Works without app source access** | Yes | Yes | No — requires build config | Yes | Yes | Depends |
| **Dynamic subpath at boot time** (path unknown until runtime, e.g. OOD `/rnode/<node>/<port>/`) | Yes — `BASE_PATH` set as env var per container | No — nginx config must be reloaded | No — requires rebuild per path | No — Caddy must be reloaded | Partial — requires redeploy | No |

---

## Honest Notes Per Approach

### nginx `sub_filter`

The closest prior art. Works for simple cases where all the paths are known strings
and compression is disabled. Falls apart in three ways: (1) gzip responses bypass
`sub_filter` unless you also add `gunzip on` (an extra module, not in the default
build); (2) it cannot patch JS runtime behaviour — fetch, WebSocket, pushState — so
the SPA router still breaks even if the HTML loads; (3) there is no framework
awareness, so replacing `"/"` as a string will corrupt unrelated JS that uses `"/"`.
The right analogy: a regex find-and-replace applied to a minified JS bundle.

### Vite `base` config

The correct solution when you have source access and control the deployment path.
Vite compiles the base path into the output so deeply that there is no runtime
fragility. The hard constraint: the path must be known at build time. It does not
work for dynamic paths (OOD's `/rnode/<hostname>/<port>/` is different for every
session), for apps where you do not have source access, or for organisations that
need to redeploy an existing artifact to a new subpath without triggering a full CI
pipeline. adaptr is not a better Vite `base` — it is for the cases where Vite `base`
cannot be used.

### Caddy `replace_response`

A community Caddy module (not in the default build) that does response-body string
replacement, analogous to nginx `sub_filter`. Same class of limitations: string
replacement without structural awareness, no framework-specific JS patching, no
gzip/brotli decompression, no runtime interceptor injection. Caddy's strength here
is its clean API-driven config reload — runtime reconfiguration without a process
restart — but the actual rewriting capability is no deeper than nginx's.

### Cloudflare Workers / edge functions

Technically capable — Workers can decompress, rewrite, and recompress arbitrary
response bodies. In practice: requires a CF account, is priced per-request above the
free tier, and places your app traffic through Cloudflare's infrastructure (a
non-starter for private subnets, HPC clusters, and air-gapped environments). The
implementation also requires writing and maintaining a custom Worker script that
handles every content type adaptr handles — HTML, CSS, JS, sourcemaps, manifests,
service workers. That is not something you pick up from a one-liner. adaptr is a
single env var.

### No solution (hand-roll in the app router)

The actual status of most teams. The app has a hardcoded `basename` prop passed to
React Router, a `PUBLIC_URL` env var checked at startup, and a script that runs
`sed -i 's|/old-prefix|/new-prefix|g'` on the build output before deploy. This
works until the path changes, until a framework upgrade moves where the public path
is set, or until someone deploys the app somewhere new and gets a half-broken SPA
with some assets loading and the router broken. It is also entirely invisible to
code review — the subpath handling is scattered across the app rather than isolated
in infrastructure.

---

## What adaptr Does Not Do

Being specific here matters. These are real limitations, not hedges:

- **No brotli output.** adaptr decompresses brotli by refusing it at the
  `Accept-Encoding` level (the upstream sends gzip or uncompressed instead). It
  does not re-compress as brotli. Production CDN setups that rely on brotli for
  performance will see slightly larger responses on the wire.
- **No streaming responses.** Responses must be fully buffered for rewriting. SSE
  and streaming endpoints should be added to `PASSTHROUGH_PATHS`.
- **No CSP / SRI fixup.** If the upstream sets a `Content-Security-Policy` or
  `<meta http-equiv="Content-Security-Policy">` that restricts script sources, or
  if assets use `integrity=` attributes (Subresource Integrity), the injected
  interceptor scripts will be blocked. CSP must be relaxed to permit the injected
  scripts or adaptr's injection must be disabled with `REWRITE_HTML=false`.
- **10 MB rewrite cap by default.** Responses larger than `MAX_REWRITE_BODY_BYTES`
  (configurable) are passed through unmodified. Oversized JS bundles will not have
  their runtime paths patched.
- **Requires a container per SPA.** Multiple SPAs at different subpaths require
  multiple adaptr instances. There is no multi-tenant mode in a single process.
- **Does not fix WebSocket over a non-standard port.** WebSocket patching handles
  `ws://` with the current hostname; it does not handle `ws://hostname:custom-port`
  patterns automatically.
- **Next.js static export is partial support.** The `/_next/` prefix is preserved
  (not rewritten), which covers most asset loading, but some Next.js patterns around
  image optimization and dynamic imports may need additional configuration.
