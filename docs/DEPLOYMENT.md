# Deployment

Single-tenant private cloud: **one customer = one isolated stack**.

## Local / early customer (Docker Compose)

Services:

- `web` — Next.js
- `api` — NestJS
- `agents` — FastAPI / LangGraph workers
- `postgres`
- `redis`
- `mongo`
- `migrate` — schema migrations

Target path: `infra/docker/`

## Enterprise private cloud

### Terraform

- VPC, subnets
- KMS / secrets
- Managed Postgres (or VM)
- Redis
- Mongo (or Atlas private link)

Target path: `infra/terraform/`

### Helm

- App deployments (`web`, `api`, `agents`)
- Ingress + certificates
- One customer = one values file, e.g. `customers/acme.yaml`

Target path: `infra/helm/`

## Isolation rules

- No shared database across customers
- Own secrets per install (LLM keys, Gmail, Stripe, LinkedIn, …)
- Optional air-gapped / private LLM endpoint as enterprise upsell

## Commercial packaging

| Tier | Includes |
|---|---|
| **Base** | Finance + Comms + Approval Inbox + Audit |
| **Growth** | Base + HR + Marketing |
| **Enterprise** | Growth + Sales research, SSO, private LLM option, Helm install |

## Health

Each app exposes health/readiness used by Compose/Kubernetes probes. Nest `HealthModule` checks Postgres, Redis, Mongo, and agents ping.
