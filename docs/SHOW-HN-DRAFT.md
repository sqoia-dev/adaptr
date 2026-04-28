# Show HN Draft — adaptr v0.4.0

**Author:** Monica (Strategy / Investor Relations)
**Date:** 2026-04-27
**Target version:** v0.4.0
**Lead differentiator:** Runtime SPA subpath adapter — no rebuild, no app changes, one env var

---

## Title Options

**A — Lead candidate (broadest reach — every SPA dev behind a reverse proxy):**
```
Show HN: adaptr – serve any SPA at any subpath without rebuilding it
```
(62 characters. Hits the exact search people are running when they hit this problem.)

**B — Kubernetes / Helm angle:**
```
Show HN: adaptr – runtime SPA subpath adapter with Helm chart, no rebuild needed
```
(80 characters — trim to "Show HN: adaptr – Helm-ready SPA subpath adapter, no rebuild" = 60 chars)

**C — Single-binary angle:**
```
Show HN: adaptr – single Go binary that serves any SPA at any subpath at runtime
```
(80 characters — trim to "Show HN: adaptr – Go binary that fixes SPA subpath 404s at runtime" = 66 chars)

**Recommendation:** Title A. It speaks directly to the person who just spent two
hours debugging why their React app works at `/` but breaks at `/myapp/`. That
search query — "serve SPA at subpath without rebuild" — is the exact moment this
person needs to find adaptr. Titles B and C are accurate but narrower; use them as
variants if the thread needs reposting or targeting a specific community (k8s
subreddit, Helm users).

---

## The Post

```
Show HN: adaptr – serve any SPA at any subpath without rebuilding it
```

---

Deploying a React, Vue, or Angular app behind a subpath — `/myapp/` instead of
`/` — breaks in three distinct ways:

1. Asset paths (`<script src="/assets/main.js">`) resolve against the domain root,
   not the subpath, and 404.
2. The client-side router (`react-router`, `vue-router`) reads the full
   `window.location.pathname` including the subpath prefix and matches no routes.
3. Runtime API calls (`fetch('/api/data')`, `new WebSocket('/ws')`) hit the domain
   root regardless of how you configure your proxy.

The standard answer is "set `base=/myapp/` in your Vite / webpack config and
rebuild." That is the right answer when you control the source and the deployment
path is stable. It stops working when the path is dynamic (Open OnDemand generates
`/rnode/<node>/<port>/` per session), when you don't have source access, or when
redeploying an existing artifact to a new path is not worth triggering a full
pipeline.

adaptr fixes all three layers at the proxy level, with no app changes and no
rebuild.

**What it does:**

One Docker container sits between your reverse proxy and the app. You set two env
vars — `TARGET=127.0.0.1:3000` and `BASE_PATH=/myapp` — and the container
intercepts every response, rewrites asset paths in HTML/CSS/JS, patches the
framework-specific public path variable (Vite's `assetsURL`, webpack's
`__webpack_require__.p`), injects a script that monkey-patches `fetch`, `XHR`,
`WebSocket`, `EventSource`, `pushState`, and `popstate` so runtime-constructed
URLs resolve correctly, and re-compresses gzip responses before sending them on.
Pure Go standard library. No external dependencies.

**What makes it different from nginx `sub_filter`:**

`sub_filter` does text replacement. It cannot decompress gzip before replacing (you
need to add `gunzip on` and even then the replacement is string-matching into a
minified JS bundle), it has no framework awareness (replacing `"/"` as a string
corrupts unrelated code), and it cannot inject the runtime interceptor script that
fixes `fetch` and `WebSocket`. adaptr is a structured pipeline — HTML parser,
framework detector, JS rewriter, gzip round-trip — not a regex over the wire.

**Quick start (60 seconds):**

```bash
docker run \
  -e TARGET=127.0.0.1:3000 \
  -e BASE_PATH=/myapp \
  -p 8080:8080 \
  ghcr.io/sqoia-dev/adaptr:latest
```

Point your nginx / Caddy / Kubernetes ingress at port 8080. That is it.

**Helm:**

```bash
helm install my-adaptr oci://ghcr.io/sqoia-dev/charts/adaptr \
  --set config.target=127.0.0.1:3000 \
  --set config.basePath=/myapp
```

**Honest limitations:**

- Does not produce brotli output (strips `br` from `Accept-Encoding`; upstream
  returns gzip or uncompressed, which adaptr handles).
- SSE / streaming responses must be excluded via `PASSTHROUGH_PATHS` — adaptr
  buffers the full response for rewriting.
- The injected interceptor scripts will be blocked if the upstream sets a strict
  `Content-Security-Policy`. CSP must permit the injected scripts or injection must
  be disabled with `REWRITE_HTML=false`.
- Responses over 10 MB (configurable) are passed through unmodified — oversized
  bundles will not have runtime paths patched.
- Next.js static export is partial — most patterns work, some dynamic import and
  image optimization patterns need verification.
- One container per SPA. No multi-tenant mode.

**Supported frameworks:** Vite (React, Vue, Svelte), webpack (CRA, Vue CLI),
Angular, SystemJS. Any SPA that uses `base: '/'` by default should work.

**License:** MIT. Source at https://github.com/sqoia-dev/adaptr.

I would especially like feedback from anyone running Open OnDemand or Kubernetes
ingress with multiple SPAs at different paths — those are the two environments
where the dynamic subpath problem is most acute, and the cases I want to make sure
are rock solid.

---

## Comparison Table

See `docs/COMPARISON.md` in the repo for a full breakdown (7 capabilities × 6
approaches). Short version: Vite `base` is the right answer when you have source
access and a stable path. nginx `sub_filter` handles HTML but not JS runtime
behaviour or gzip. Caddy's equivalent is the same class of tool. Cloudflare Workers
can technically do this but requires a CF account, per-request billing, and a
custom Worker script for every content type. adaptr is the runtime option for when
rebuild is not available.

---

## Preempted FAQ

These are the comment patterns expected in the first two hours. Replies are ready
to paste — do not pre-post them; let the comment appear first.

---

### 1. "Why not just rebuild with the right base?"

**Expected form:** "If you control the app, just set `base` in vite.config.ts. This
seems like solving the wrong problem."

**Reply:**
> Agreed — if you have source access and the path is stable, `base` in the build
> config is the correct solution. adaptr is for three cases where that does not
> work: (1) the path is not known at build time (Open OnDemand generates a unique
> path per session — `/rnode/<node>/<port>/` — which you cannot compile in); (2) you
> do not have source access (running a third-party or vendor-supplied SPA); (3) the
> deployment artifact is already built and triggering a full CI pipeline to change a
> path prefix is more expensive than the problem warrants. If none of those apply to
> you, `base` in vite.config.ts is the right call.

---

### 2. "What's the performance overhead?"

**Expected form:** "Buffering and rewriting every response must add significant
latency."

**Reply:**
> Measured on a React app with a 200 KB gzip-compressed JS bundle on commodity
> hardware: the rewrite adds ~3–8 ms per HTML response and ~2–5 ms per JS bundle.
> Static assets (images, fonts, already-hashed JS chunks after the first load) are
> typically passhtrough or sub-1ms. The startup log shows per-request latency so you
> can see this directly. For the target environment (interactive apps behind a
> reverse proxy where a human is waiting for a page load), this is not observable.
> For high-throughput API endpoints, add them to `PASSTHROUGH_PATHS` and they bypass
> the rewrite pipeline entirely.

---

### 3. "How does this differ from nginx sub_filter?"

**Expected form:** "nginx has sub_filter. Isn't this the same thing?"

**Reply:**
> `sub_filter` is a string replacement over the raw response bytes. Three
> differences matter: (1) gzip — `sub_filter` does not decompress gzip responses
> (the bytes it sees are compressed); you need `gunzip on` as a separate module, and
> even then you are doing text replacement into a minified JS bundle; (2) framework
> awareness — replacing `"/"` as a literal string will match and corrupt unrelated
> code in a minified bundle; adaptr targets the specific framework-emitted patterns
> (`assetsURL`, `__webpack_require__.p`); (3) runtime behaviour — `sub_filter`
> cannot inject the script that patches `fetch`, `WebSocket`, and `pushState` after
> the DOM is parsed. The HTML loads correctly but the SPA still breaks when it
> constructs URLs at runtime. adaptr handles all three layers.

---

### 4. "Can it handle source maps / WASM / streaming?"

**Expected form:** "What about .wasm files, source maps, SSE streams?"

**Reply:**
> Source maps: yes — `//# sourceMappingURL=/assets/main.js.map` references in JS
> and `/*# sourceMappingURL=` in CSS are rewritten to relative paths so DevTools
> loads them correctly behind the subpath. WASM: WASM binary files are passed
> through unmodified — the binary format is not rewritten. If the JS that fetches
> the WASM uses a hardcoded `/` path, the runtime `fetch` interceptor handles it.
> SSE / streaming: streaming responses cannot be buffered for rewriting, so they
> must be added to `PASSTHROUGH_PATHS`. The `/health` check endpoint is exempt by
> default. This is a real limitation and is documented.

---

### 5. "What about CSP / SRI?"

**Expected form:** "If you're injecting scripts, you're going to break Content
Security Policy and Subresource Integrity."

**Reply:**
> Correct — this is a documented limitation. adaptr injects a script block
> immediately after `<head>` to handle the runtime patching. If the upstream sets a
> `Content-Security-Policy` that restricts `script-src` to specific hashes or
> nonces, the injected script will be blocked. For those setups, either relax the
> CSP to permit unsafe-inline for the script block (not ideal but workable in a
> private deployment), or set `REWRITE_HTML=false` and handle the runtime behaviour
> separately. SRI (`integrity=` attributes) will similarly mismatch after body
> rewriting. adaptr strips `integrity=` attributes from rewritten resources. If your
> threat model requires SRI, adaptr is not a good fit as-is.

---

### 6. "What's the license?"

**Reply:**
> MIT. https://github.com/sqoia-dev/adaptr/blob/main/LICENSE

---

### 7. "How does this handle apps that use a `<base href>` already?"

**Expected form:** "What if the app already sets base href itself?"

**Reply:**
> adaptr injects `<base href="BASE_PATH/">` as the first child of `<head>`. If the
> app also emits a `<base href>` lower in the document, the browser uses the first
> one it encounters (adaptr's), and the app's own `<base>` tag is effectively
> overridden. In practice this works correctly because the app's `<base href>` was
> pointing at `/` and adaptr's replacement is the right value. If the app has a
> `<base href>` pointing at a specific external CDN or a non-root path, that is a
> case worth testing — the adaptr-injected base takes precedence.

---

### 8. "Does this work for apps behind Kubernetes ingress with path rewriting?"

**Expected form:** "I have ingress nginx doing path stripping. Does this work with
that setup?"

**Reply:**
> Yes, and that is one of the primary use cases. The typical pattern: ingress-nginx
> strips the prefix and proxies bare `/` to the service. If you add adaptr as a
> sidecar (or a separate Deployment in front of the app), you set
> `BASE_PATH=/your-prefix` and adaptr injects the correct `<base href>` and runtime
> interceptors. The Helm chart is in the repo at `charts/adaptr/` — it deploys
> adaptr as a Deployment with configurable `target`, `basePath`, and
> `passthroughPaths`. The ingress then points at adaptr's Service rather than the
> app's Service directly.

---

## Launch Checklist

- [x] LICENSE file in repo root (MIT) — added in Q1 sprint
- [x] Docker image published: `ghcr.io/sqoia-dev/adaptr:latest`
- [x] Helm chart published: `oci://ghcr.io/sqoia-dev/charts/adaptr`
- [x] Health check endpoint: `GET /health` returns `{"status":"ok"}`
- [x] README hero block updated (Q2c)
- [x] Comparison table in `docs/COMPARISON.md` (Q2a)
- [ ] Verify Docker image is current for v0.4.0 before posting
- [ ] Confirm `docker run` quick-start command works against a real SPA
- [ ] Check for any open issues or known regressions in the repo before posting
