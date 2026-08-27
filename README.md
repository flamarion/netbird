# netbird-presentation

Technical case submission for the NetBird Solutions Engineer role (Part I):
a phased design for replacing Summit Infrastructure's FortiGate SSL VPN and
site-to-site IPsec with a self-hosted NetBird overlay.

Deployed as a static-assets-only Cloudflare Worker: `netbird-presentation`.

## Contents

```
.
├── wrangler.jsonc     Worker config (assets-only, no main)
└── site/
    ├── index.html     Overview: what the deliverables are and how to read them
    ├── write-up.html  Technical write-up, answers the design questions per phase
    └── deck.html      12-slide presentation (press N for speaker notes)
```

Everything is plain static HTML. No build step, no dependencies, no framework.
Styles and scripts are inlined per page; the diagrams are embedded as data URIs.

## Deploying

Automatic on push to `main` once the Worker is connected to this repository
through Workers Builds.

Manual deploy from a clone:

```bash
npx wrangler@latest deploy
```

Or without reading the config file:

```bash
npx wrangler@latest deploy \
  --assets ./site \
  --name netbird-presentation \
  --compatibility-date 2026-08-27
```

## Local preview

```bash
npx wrangler@latest dev
```

Or, since it is only static files:

```bash
python3 -m http.server 8080 --directory site
```

## Notes

- Requests that match no file return a plain 404. To brand it, add
  `site/404.html` and set `"not_found_handling": "404-page"` under `assets`.
- Do not set `single-page-application` handling. These are three real pages,
  and that mode would serve `index.html` instead of a genuine 404.
- Keep `wrangler.jsonc` at the repository root, outside `site/`. Anything
  inside `site/` is uploaded and publicly served.
