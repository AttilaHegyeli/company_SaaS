# Control Plane API

Base path: `/api/v1`  
Style: REST + OpenAPI (generated from Nest decorators)  
Auth: Bearer session/JWT (local) → OIDC later

## Auth

| Method | Path | Notes |
|---|---|---|
| `POST` | `/auth/login` | Local bootstrap |
| `POST` | `/auth/logout` | |
| `GET` | `/auth/me` | Current user + roles |

## Jobs

| Method | Path | Notes |
|---|---|---|
| `GET` | `/jobs` | Filter: `status`, `agent_key` |
| `POST` | `/jobs` | Body: `agent_key`, `input`, `idempotency_key` |
| `GET` | `/jobs/:id` | |
| `POST` | `/jobs/:id/cancel` | |

## Approvals

| Method | Path | Body |
|---|---|---|
| `GET` | `/approvals?status=pending` | |
| `GET` | `/approvals/:id` | |
| `POST` | `/approvals/:id/approve` | `{ note? }` |
| `POST` | `/approvals/:id/reject` | `{ note }` |
| `POST` | `/approvals/:id/edit-approve` | `{ edited_payload, note? }` |

## Agents

| Method | Path | Notes |
|---|---|---|
| `GET` | `/agents` | Catalog |
| `PATCH` | `/agents/:key` | Enable/disable/config |
| `POST` | `/agents/:key/run` | Convenience → creates job |

## Connectors

| Method | Path | Notes |
|---|---|---|
| `GET` | `/connectors` | |
| `POST` | `/connectors/:provider/test` | Health check |
| `PUT` | `/connectors/:provider` | Set `mode` (`demo`\|`live`), `secret_ref` |

## Audit

| Method | Path | Roles |
|---|---|---|
| `GET` | `/audit?entity_type=&entity_id=` | Authenticated with audit read |
| `GET` | `/audit/export` | Approver, Auditor, Owner |

## Internal (agent runtime only)

Protected by `agent_service` token. Not exposed to browsers.

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/internal/jobs/:id/proposal` | Submit proposal + open approval |
| `POST` | `/internal/jobs/:id/fail` | Mark job failed |

## Contracts package

Canonical OpenAPI will live at:

```text
packages/contracts/openapi/control-plane.yaml
```

TypeScript Zod and Python JSON Schema mirrors stay in sync via codegen scripts.
