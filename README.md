# SOP Operations Dashboard — Production  Scaffold 

Full-stack production app: Next.js 14 frontend + FastAPI backend with Redis cache, retry/backoff, and 25+ API integrations.

## Stack

| Layer | Tech |
|---|---|
| Frontend | Next.js 14 (App Router) + Tailwind CSS |
| Backend | FastAPI + Uvicorn |
| Cache | Redis (TTL per source) |
| Queue | Celery + Redis broker |
| DB | PostgreSQL (trigger history, SLA log) |
| Infra | Docker Compose (local) / Railway or Render (prod) |

## Quick Start

```bash
# 1. Clone and install
cp .env.example .env          # fill in your API keys
docker compose up --build     # starts all services

# 2. Access
Frontend  → http://localhost:3000
API docs  → http://localhost:8000/docs
Redis UI  → http://localhost:8001   (RedisInsight)
```

## Project Structure

```
sop-dashboard/
├── backend/
│   ├── app/
│   │   ├── main.py              # FastAPI app entrypoint
│   │   ├── api/
│   │   │   ├── market.py        # /api/market  — crypto, FX, stocks
│   │   │   ├── macro.py         # /api/macro   — FRED, CPI, unemployment
│   │   │   ├── aqi.py           # /api/aqi     — AQI + weather
│   │   │   ├── news.py          # /api/news    — HackerNews, SEC EDGAR
│   │   │   ├── hr.py            # /api/hr      — Clockify, team data
│   │   │   └── sop.py           # /api/sop     — Notion, Airtable, triggers
│   │   ├── core/
│   │   │   ├── config.py        # Settings (pydantic-settings)
│   │   │   ├── cache.py         # Redis wrapper + TTL constants
│   │   │   ├── http.py          # Shared httpx client + retry logic
│   │   │   └── trigger.py       # Trigger rule engine
│   │   ├── services/            # One file per external API
│   │   └── models/              # Pydantic response models
│   ├── tests/
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── page.tsx             # Dashboard root
│   │   └── api/                 # Next.js API routes (proxy to FastAPI)
│   ├── components/
│   │   ├── dashboard/           # KPI cards, action queue, kanban, charts
│   │   └── ui/                  # shadcn/ui primitives
│   ├── lib/
│   │   ├── fetcher.ts           # SWR fetcher with error handling
│   │   └── types.ts             # Shared TypeScript types
│   ├── package.json
│   └── Dockerfile
├── scripts/
│   ├── seed_db.py               # Seed trigger rules + SOP registry
│   └── health_check.sh          # Check all 25 API sources
├── docker-compose.yml
├── docker-compose.prod.yml
└── .env.example
```

## API Keys Needed

See `.env.example` for the full list. Free tiers are sufficient for Week 1-2.

| Service | Key Name | Free Tier |
|---|---|---|
| CoinGecko | `COINGECKO_API_KEY` | 30 calls/min |
| Alpha Vantage | `ALPHA_VANTAGE_KEY` | 25 calls/day |
| FRED | `FRED_API_KEY` | Unlimited |
| Frankfurter FX | — | No key needed |
| Open-Meteo | — | No key needed |
| AQICN | `AQICN_TOKEN` | Free |
| SEC EDGAR | — | No key needed |
| HackerNews | — | No key needed |
| Notion | `NOTION_TOKEN` | Free |
| Airtable | `AIRTABLE_TOKEN` | Free |
| Clockify | `CLOCKIFY_API_KEY` | Free |
| Trello | `TRELLO_API_KEY` + `TRELLO_TOKEN` | Free |
