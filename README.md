# Actx0 Docs

Mintlify documentation site. Everything for the docs lives in this directory.

## Preview locally

```bash
npm i -g mint
cd docs
mint dev
```

## Deploy

Connect this `docs/` folder (or the repo with docs path set to `docs`) in the [Mintlify dashboard](https://dashboard.mintlify.com).

## Structure

| Path | Purpose |
| --- | --- |
| `docs.json` | Mintlify site config and navigation |
| `index.mdx` | Docs home |
| `actx0-platform/` | Platform overview and quickstart |
| `api-reference/` | REST API pages |
| `integrations/` | Integrations (in progress) |
| `agent-plugins/` | Agent plugins (in progress) |
| `release-notes/` | Platform and SDK release notes |
| `logo.png` / `favicon.png` | Brand assets (add locally — see below) |

## Brand assets

Image writes are blocked in some agent environments. Add them manually once:

```bash
cp static/logo.png docs/logo.png
cp static/logo.png docs/favicon.png
```

Then set in `docs.json`:

```json
"logo": { "light": "/logo.png", "dark": "/logo.png" },
"favicon": "/favicon.png"
```
