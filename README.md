# Company OS

Private multi-agent operating system for a company. Specialist AI agents do domain work; humans approve every irreversible action.

**Core rule:** Agents propose. Humans dispose.

## What we build

| Piece | Role |
|---|---|
| **Finance agent** | Checks payments, flags anomalies |
| **HR agent** | Contracts, employees, access/rights proposals |
| **Comms agent** | Emails, messages, project-request triage |
| **Marketing agent** | Social drafts and publish proposals |
| **Sales agent** | Web research for companies/projects |
| **Briefing Assistant** | Surfaces finished jobs as decision packets |
| **Approval Gateway** | Approve / reject / edit before side effects |

Delivery model: **single-tenant private cloud** (one isolated stack per customer).

## Stack (locked)

| Layer | Choice |
|---|---|
| Web | Next.js (App Router) + TypeScript |
| Control plane | NestJS + TypeScript (REST + OpenAPI) |
| Agent runtime | FastAPI + Python + LangGraph |
| System of record | PostgreSQL |
| Queue / realtime | Redis |
| Artifacts / traces | MongoDB |

Architecture style: **SOLID + hexagonal (ports & adapters)** with deeply separated folders per bounded context.

## Documentation

Start here: **[docs/README.md](docs/README.md)**

| Doc | Topic |
|---|---|
| [VISION](docs/VISION.md) | Product purpose, buyers, non-goals |
| [ARCHITECTURE](docs/ARCHITECTURE.md) | System design and trust boundaries |
| [STRUCTURE](docs/STRUCTURE.md) | Folder map and ownership |
| [SOLID](docs/SOLID.md) | Principles, dependency rules, lint gates |
| [DATA_MODEL](docs/DATA_MODEL.md) | Postgres / Mongo / Redis schemas |
| [API](docs/API.md) | Control-plane REST API |
| [AGENTS](docs/AGENTS.md) | Agent missions and forbidden actions |
| [APPROVALS](docs/APPROVALS.md) | Decision packets and side effects |
| [SECURITY](docs/SECURITY.md) | RBAC, secrets, audit, PII |
| [DEMOS](docs/DEMOS.md) | Deterministic demo ladder 0–6 |
| [DEPLOYMENT](docs/DEPLOYMENT.md) | Compose and private-cloud install |
| [ROADMAP](docs/ROADMAP.md) | Phases, MVP cut line, commercial tiers |

## Current status

- Documentation blueprint published under `docs/`
- Application code not scaffolded yet
- **MVP cut line:** Foundation → Approval Gateway → Finance Demo 1 (mock fixtures)

## Repo layout (target)

```text
apps/web          Next.js UI
apps/api          NestJS control plane
apps/agents       FastAPI + LangGraph
packages/         Contracts, fixtures, lint rules
infra/            Docker, Helm, Terraform
docs/             Product and engineering docs
```

See [docs/STRUCTURE.md](docs/STRUCTURE.md) for the full SOLID tree.

## License

Proprietary — all rights reserved until otherwise stated.
