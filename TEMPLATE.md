# Railway Template Composer Setup

## Marketplace listing

- **Title:** Deploy and Host Simple Analytics with Railway
- **Short description:** Self-hosted page analytics you own the code for — FastAPI, Postgres, embed script, password dashboard.
- **Icon:** `website/icon-512.png`
- **Differentiator:** Not Umami — forkable FastAPI source, 2 services only
- **Category:** Analytics
- **Overview:** paste `README.md`

## Services

| Service | Source | Volume | Public HTTP |
| --- | --- | --- | --- |
| Simple Analytics | GitHub repo (this folder) | — | Yes |
| Postgres | Railway PostgreSQL plugin | `/var/lib/postgresql/data` | No |

## Variables — Simple Analytics

| Variable | Value | Secret | Description |
| --- | --- | --- | --- |
| `DATABASE_URL` | `${{Postgres.DATABASE_URL}}` | Yes | Postgres connection |
| `SITE_ID` | `my-site` | No | Label for this site's traffic |
| `DASHBOARD_PASSWORD` | `${{secret(32)}}` | Yes | Opens `/dashboard?key=...` |

## Settings — Simple Analytics

- Healthcheck: `/health`
- Attach volume to PostgreSQL
