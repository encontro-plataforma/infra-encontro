# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**infra-encontro** is the local development infrastructure for the Encontro platform. It contains **no application code** — only Docker Compose orchestration, environment templates, and documentation for running the stack locally.

It exists alongside two sibling app repositories under the `encontro-plataforma` GitHub org:

- [`financeiro-encontro-api`](https://github.com/encontro-plataforma/financeiro-encontro-api) — FastAPI backend
- [`financeiro-encontro-web`](https://github.com/encontro-plataforma/financeiro-encontro-web) — Angular frontend

These three repos were split out of a single `financeiro-encontro` monorepo on 2026-07-19. As the platform grows (mobile apps, more APIs, more frontends), new services should be added here as additional `docker-compose` entries following the same sibling-folder convention, not as code in this repo.

## Sibling-folder convention

`docker-compose.yml`'s `build.context` values point to `../financeiro-encontro-api` and `../financeiro-encontro-web`. All repos must be cloned side by side:

```
SISTEMA_ENCONTRO/
├── financeiro-encontro-api/
├── financeiro-encontro-web/
└── infra-encontro/          <- this repo
```

## Files

- `docker-compose.yml` — full stack: Postgres + backend + frontend, built from the sibling app repos
- `docker-compose-db.yml` — DB-only (Postgres + pgAdmin), for developers running the apps via their own dev servers (`./start-backend.sh`, `ng serve`)
- `.env.example` — template for all variables needed by `docker-compose.yml` (DB credentials, JWT secret, CORS/API URLs)

## Commands
```bash
cp .env.example .env      # first time only
docker compose up --build              # full stack
docker compose -f docker-compose-db.yml up -d   # db only
docker compose down / down -v          # stop (optionally wiping volumes)
```

No render.yaml or cloud deploy config lives here — each app repo has its own Render Blueprint (`render.yaml`) for production deploys; this repo is local-only.
