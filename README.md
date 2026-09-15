# Deploy and Host Simple Analytics with Railway

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new)

Self-hosted page analytics you own the code for — FastAPI, PostgreSQL, one embed script, password-protected dashboard.

**Live demo:** [simple-analytics-production.up.railway.app/health](https://simple-analytics-production.up.railway.app/health)

## What you get

- **`/script.js`** — drop-in tracker for any website
- **`/dashboard?key=...`** — 7-day views, top pages, referrers
- **FastAPI source** you can fork (not a black-box Docker image)
- **PostgreSQL** storage on a Railway volume
- **`/health`** for deploy health checks

## About Simple Analytics

Simple Analytics is a minimal FastAPI service for developers who want analytics without maintaining an Umami fork or shipping data to Google. Collect page views and referrers in Postgres, open a password-protected dashboard, extend the API when you need custom logic.

## About Hosting Simple Analytics

Railway builds from GitHub, wires `${{Postgres.DATABASE_URL}}` over private networking, and exposes HTTPS with health checks. Two services — app and Postgres — no Redis, ClickHouse, or nginx required.

## Architecture

```
Browser ──► script.js ──► FastAPI ──► PostgreSQL (volume)
                │
Dashboard ◄─────┘ (password in query string)
```

## Environment Variables

| Variable | Description | Secret | Example/Notes |
| --- | --- | --- | --- |
| `DATABASE_URL` | PostgreSQL connection string | Yes | `${{Postgres.DATABASE_URL}}` |
| `SITE_ID` | Label for this site's traffic | No | `my-site` |
| `DASHBOARD_PASSWORD` | Password for `/dashboard?key=...` | Yes | `${{secret(32)}}` |

## Deploy and Host

1. Deploy this repo from GitHub on Railway.
2. Add **PostgreSQL** and attach a **volume** at `/var/lib/postgresql/data`.
3. Set `DATABASE_URL`, `SITE_ID`, and `DASHBOARD_PASSWORD` on the app service.
4. Enable **public HTTP** and deploy.
5. Confirm `/health` returns `{"status":"ok"}`.
6. Add the tracking script:

```html
<script src="https://YOUR-RAILWAY-URL/script.js" defer></script>
```

7. Open `https://YOUR-RAILWAY-URL/dashboard?key=YOUR_DASHBOARD_PASSWORD`.

## Common Use Cases

- Portfolio and landing-page traffic without GA4
- Early MVPs measuring which docs or pricing pages get views
- Internal tools with a public marketing page
- Developers who want to own the analytics codebase

## Why not Umami?

Umami has thousands of Railway listings. Choose **Simple Analytics** when you want a tiny FastAPI app you control — not funnels or session replay, just page views and referrers with code you can read in an afternoon.

## Dependencies for Simple Analytics Hosting

| Service | Source | Volume |
| --- | --- | --- |
| Simple Analytics | GitHub repo (this template) | — |
| Postgres | Railway PostgreSQL plugin | `/var/lib/postgresql/data` |

## Deployment Dependencies

- [FastAPI documentation](https://fastapi.tiangolo.com/)
- [Railway PostgreSQL docs](https://docs.railway.com/databases/postgresql)

## Why Deploy Simple Analytics on Railway?

Two services, one GitHub deploy, reference variables for Postgres, built-in health checks — the fastest path from zero to self-hosted analytics you actually understand.

## Run locally

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
copy .env.example .env
uvicorn app.main:app --reload --port 8000
```

## Marketing site

Preview `website/index.html`. Marketplace icon: export `website/icon.svg` to 512×512 PNG.

## Author

romeoxt — herbylegall9@gmail.com

## License

MIT
