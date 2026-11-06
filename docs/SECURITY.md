# Security

## Baseline controls

| Control | v1 implementation |
|---|---|
| Isolation | Single-tenant stack per customer |
| AuthN | Local users first; OIDC next; SAML enterprise |
| AuthZ | RBAC roles below |
| Secrets | Env / sealed secrets; DB stores `secret_ref`, not raw tokens |
| Audit | Append-only `audit_events` |
| HITL | Mandatory before irreversible tools |
| PII | Redaction flags on artifacts and traces |
| Network | FastAPI internal-only |
| Policies | Amount thresholds and role gates |
| Observability | OpenTelemetry across Nest ↔ FastAPI ↔ tools |
| Backup | Postgres continuous backup + Mongo snapshots (install runbook) |

## Roles

| Role | Intent |
|---|---|
| `owner` | Full admin for the private install |
| `approver` | Decide on decision packets |
| `operator` | Trigger/monitor jobs |
| `auditor` | Read-only audit and exports |
| `agent_service` | Machine identity for FastAPI → Nest internal API |

## Human-in-the-loop

Irreversible tools (`pay`, `send_email`, `publish_post`, `grant_access`, `create_crm_lead`) require:

```text
approval_requests.status IN (approved, edited_approved)
```

plus RBAC + policy checks in Nest `SideEffectsModule`.

## Secrets

- Connector credentials referenced by `secret_ref`
- Never store live OAuth/API tokens as plaintext columns in Postgres
- Prefer sealed Kubernetes secrets or cloud secret manager per customer install

## Audit ledger

- Append-only table
- Application role: `INSERT` + `SELECT` only
- Every approve/reject/edit and every executed side effect is recorded with actor identity

## PII

- Emails, salaries, and similar fields marked via `redaction_flags`
- Run traces store hashed/redacted tool args where required

## Network

```text
Internet → Next.js / NestJS only
FastAPI ← private network from Nest / workers only
```

## Policy examples

- Finance proposals above a threshold require `approver` role
- Marketing publish always requires approval (no auto-publish path in v1)
