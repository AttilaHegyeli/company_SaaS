# Repository Structure

Every concern lives in a deeply separated folder. A developer can change Finance DEMO tools without touching Approvals domain, Nest HTTP, or Next.js UI.

## Top level

```text
company_SaaS/
├── apps/
│   ├── web/                      # Next.js presentation only
│   ├── api/                      # NestJS control plane (hexagonal)
│   └── agents/                   # FastAPI + LangGraph runtime (hexagonal)
├── packages/
│   ├── contracts/                # Shared API contracts (OpenAPI, Zod, JSON Schema)
│   ├── domain-ids/               # Branded IDs / enums (codegen)
│   ├── demo-fixtures/            # Deterministic demo datasets ONLY
│   └── eslint-config/            # Lint rules blocking illegal imports
├── infra/
│   ├── docker/
│   ├── helm/
│   └── terraform/
├── docs/                         # This documentation set
├── scripts/                      # Codegen, seed, demo reset
├── pnpm-workspace.yaml
├── pyproject.toml
└── README.md
```

## `apps/api` — NestJS hexagonal

```text
apps/api/
├── src/
│   ├── main.ts
│   ├── app.module.ts                       # composition root only
│   ├── config/
│   ├── modules/
│   │   ├── identity/
│   │   │   ├── domain/
│   │   │   ├── application/
│   │   │   ├── ports/
│   │   │   ├── adapters/
│   │   │   │   ├── inbound/http/
│   │   │   │   └── outbound/persistence/
│   │   │   └── identity.module.ts
│   │   ├── jobs/
│   │   │   ├── domain/
│   │   │   ├── application/
│   │   │   ├── ports/
│   │   │   ├── adapters/
│   │   │   │   ├── inbound/http/
│   │   │   │   ├── inbound/internal/
│   │   │   │   └── outbound/
│   │   │   │       ├── persistence/
│   │   │   │       └── queue/
│   │   │   └── jobs.module.ts
│   │   ├── approvals/
│   │   │   ├── domain/
│   │   │   ├── application/
│   │   │   ├── ports/
│   │   │   ├── adapters/
│   │   │   │   ├── inbound/http/
│   │   │   │   └── outbound/
│   │   │   │       ├── persistence/
│   │   │   │       └── realtime/
│   │   │   └── approvals.module.ts
│   │   ├── agents-catalog/
│   │   ├── connectors/
│   │   ├── side-effects/
│   │   │   ├── domain/
│   │   │   ├── application/
│   │   │   ├── ports/
│   │   │   └── adapters/outbound/tools/
│   │   │       ├── demo/
│   │   │       └── live/
│   │   ├── audit/
│   │   ├── artifacts/
│   │   └── health/
│   └── shared/
│       ├── result/
│       ├── errors/
│       └── observability/
├── test/
│   ├── unit/
│   ├── integration/
│   └── e2e/
└── prisma/   # or migrations/
```

**Rule:** UI never imports `modules/*/adapters/outbound`. Agents never import Nest controllers — only contracts + internal HTTP.

## `apps/agents` — FastAPI / LangGraph hexagonal

```text
apps/agents/
├── src/
│   ├── main.py
│   ├── composition/
│   │   ├── container.py
│   │   └── settings.py
│   ├── domain/
│   ├── application/
│   ├── ports/
│   │   ├── control_plane.py
│   │   ├── artifact_store.py
│   │   ├── ledger.py
│   │   ├── mailbox.py
│   │   ├── hr_directory.py
│   │   ├── social_publisher.py
│   │   └── web_search.py
│   ├── adapters/
│   │   ├── inbound/
│   │   │   ├── http/
│   │   │   └── queue/
│   │   └── outbound/
│   │       ├── control_plane/
│   │       ├── persistence_mongo/
│   │       └── tools/
│   │           ├── demo/
│   │           └── live/
│   └── graphs/
│       ├── finance_graph.py
│       ├── hr_graph.py
│       ├── comms_graph.py
│       ├── marketing_graph.py
│       ├── sales_graph.py
│       └── assistant_graph.py
└── tests/
    ├── unit/
    ├── contract/
    └── integration/
```

**Rule:** `graphs/*` never contains SQL, Redis clients, or raw Gmail/Stripe HTTP.

## `apps/web` — Next.js presentation

```text
apps/web/
├── src/
│   ├── app/                    # thin routes
│   ├── views/
│   │   ├── approvals-inbox/
│   │   ├── jobs/
│   │   ├── agents/
│   │   └── admin/
│   ├── widgets/
│   ├── features/
│   │   ├── approvals/
│   │   ├── jobs/
│   │   └── auth/
│   ├── entities/
│   ├── shared/
│   │   ├── ui/
│   │   ├── lib/
│   │   └── api-client/
│   └── processes/
└── tests/
```

**Rule:** no business state machine in React. Nest enforces invariants.

## `packages/`

```text
packages/
├── contracts/
│   ├── openapi/control-plane.yaml
│   ├── zod/
│   └── jsonschema/
├── demo-fixtures/
│   ├── finance/
│   ├── comms/
│   ├── hr/
│   ├── marketing/
│   ├── sales/
│   └── org/
└── eslint-config/
```

## Why this structure sells

| Concern | Folder | Buyer value |
|---|---|---|
| Domain rules | `*/domain`, `*/application` | Testable without cloud |
| DEMO → LIVE | `adapters/**/demo` vs `live` | Safe demos; production later |
| Trust layer | `modules/approvals` | HITL cannot be bypassed by agents |
| Agent intelligence | `apps/agents/graphs` | LLM changes do not rewrite RBAC |
| UI | `apps/web` | Design without touching payroll logic |
| Install | `infra/*` | Repeatable private cloud |

See also: [SOLID.md](./SOLID.md).
