# Vision

## Purpose

Build a sellable, industry-grade **private multi-agent operating system** for a company. Specialist AI agents perform domain work. A **Briefing Assistant** turns finished jobs into decision packets. A human **Approver** must approve, reject, or edit before any irreversible side effect runs.

## Buyers

- Mid-market and enterprise operations leaders who want AI leverage without unsupervised risk
- Companies that require private deployment (data stays in their stack)
- Buyers who need audit trails for finance, HR, and outbound communications

## In scope

- Hybrid control plane (NestJS) + agent runtime (FastAPI / LangGraph)
- Mandatory human-in-the-loop Approval Gateway
- Six agent roles: Finance, HR, Comms, Marketing, Sales, Briefing Assistant
- Deterministic demo datasets and a demo ladder that expands over time
- Single-tenant deploy model (Docker Compose → Helm / Terraform)
- Security, RBAC, audit, and observability baselines
- Phased roadmap from MVP to enterprise packaging

## Out of scope (v1 non-goals)

- Shared multi-tenant cloud database (customers do not share data planes)
- Autonomous money movement, contract execution, email send, or social publish without approval
- Full SOC 2 / ISO certification in the first engineering cycle (we design for auditability; certification is a later program)
- NestJS and Fastify as parallel public APIs (NestJS only)
- GraphQL in v1 (REST + OpenAPI only)

## Core product rule

**Agents propose. Humans dispose.**

Side effects such as `pay`, `send_email`, `publish_post`, `grant_access`, and `create_crm_lead` execute **only** after:

```text
approval_requests.status = APPROVED
```

Every decision writes an audit event naming the human actor.

## Success narrative

A prospect watches Demo 1–2 in about 15 minutes and understands:

1. Agents work continuously on company tasks
2. Nothing irreversible happens without a named human
3. Every decision is auditable
4. The system runs as **their** private stack
