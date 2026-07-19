# Infra Encontro

Infraestrutura local (Docker Compose) para desenvolvimento da **Plataforma Encontro** — hoje cobre o sistema financeiro (`financeiro-encontro-api` + `financeiro-encontro-web`), e deve acomodar futuros serviços da plataforma.

Este repositório **não contém código de aplicação** — apenas orquestração Docker, variáveis de ambiente de exemplo e documentação de como subir o ambiente local completo.

---

## Convenção: pastas irmãs

Os `build.context` do `docker-compose.yml` apontam para pastas irmãs deste repositório. Clone os repositórios da plataforma lado a lado:

```
SISTEMA_ENCONTRO/
├── financeiro-encontro-api/     # github.com/encontro-plataforma/financeiro-encontro-api
├── financeiro-encontro-web/     # github.com/encontro-plataforma/financeiro-encontro-web
└── infra-encontro/              # este repositório
```

```bash
mkdir SISTEMA_ENCONTRO && cd SISTEMA_ENCONTRO
git clone https://github.com/encontro-plataforma/financeiro-encontro-api.git
git clone https://github.com/encontro-plataforma/financeiro-encontro-web.git
git clone https://github.com/encontro-plataforma/infra-encontro.git
```

---

## Opção 1 — Stack completa (banco + backend + frontend) via Docker

**Pré-requisitos:** Docker e Docker Compose.

```bash
cd infra-encontro
cp .env.example .env
# edite .env: defina ao menos POSTGRES_PASSWORD e JWT_SECRET
docker compose up --build
```

| Serviço | URL |
|---|---|
| Frontend | `http://localhost` |
| API | `http://localhost:8000` |
| Swagger | `http://localhost:8000/docs` |
| Health Check | `http://localhost:8000/health` |

```bash
docker compose down           # para e remove os containers (dados persistem nos volumes)
docker compose down -v        # para e remove containers E volumes (apaga o banco)
docker compose logs -f backend
docker compose up --build --no-cache
```

---

## Opção 2 — Apenas o banco (para rodar backend/frontend em modo dev)

Use quando quiser rodar o backend com `./start-backend.sh` e o frontend com `ng serve` (veja o README de cada repositório).

```bash
cd infra-encontro
docker compose -f docker-compose-db.yml up -d
```

| Serviço | URL |
|---|---|
| PostgreSQL | `localhost:5432` |
| pgAdmin | `http://localhost:9090` |

---

## Documentação detalhada

- [financeiro-encontro-api](https://github.com/encontro-plataforma/financeiro-encontro-api) — endpoints, autenticação, variáveis de ambiente, migrations
- [financeiro-encontro-web](https://github.com/encontro-plataforma/financeiro-encontro-web) — estrutura de componentes, variáveis de ambiente e build
