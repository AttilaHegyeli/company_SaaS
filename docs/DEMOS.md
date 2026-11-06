# Demos

Deterministic fixtures so sales demos never flake. Seed org: **Acme Demo**.

## Demo 0 — Skeleton

- Services up: `web`, `api`, `agents`, `postgres`, `redis`, `mongo`
- Login as `owner@acme.demo` (documented demo password)
- Empty Approval Inbox
- Health endpoints green

## Demo 1 — Finance (MVP sell story)

- Fixture: **20 payments** (`PAY-001` … `PAY-020`)
- Anomalies (exactly two):
  - Invoice `#104` / `PAY-104`: amount mismatch (invoice `10000`, payment `9500`)
  - `PAY-007`: overdue more than 30 days
- Run Finance agent → 1–2 approval cards
- Approver approves flag on `#104` → audit shows actor + action
- Later expansion: Stripe / QuickBooks live connector

## Demo 2 — Comms

- Fixture mailbox: 12 messages; 3 are project requests
- Agent drafts replies; project briefs as artifacts
- Later expansion: Gmail / Outlook OAuth; send on approve

## Demo 3 — HR

- Fixture: 15 employees; 2 contracts expiring in 30 days; 1 excessive access right
- Proposals only
- Later expansion: HRIS + e-sign draft

## Demo 4 — Marketing

- Brand kit + 5 posts
- Approve → DEMO publisher sets `published_mock=true`
- Later expansion: LinkedIn / X APIs

## Demo 5 — Sales research

- ICP: mid-market B2B industrial services, EU, 50–500 employees
- 8 seeded company pages in fixtures
- Agent returns ranked dossiers (top 3)
- Later expansion: live web search + CRM

## Demo 6 — Fleet + Briefing Assistant

- Cron triggers Finance + Comms + Sales
- Assistant ranks a single morning briefing
- Handoff example: Sales dossier → Comms outreach draft → both await in one inbox

## Demo script goal

Prospects understand in ~15 minutes (Demo 1–2):

1. Agents work unsupervised until the gate
2. Nothing irreversible without a named human
3. Every action is auditable
4. Install is their private stack

## Fixture location (target)

```text
packages/demo-fixtures/
  finance/payments.json
  comms/
  hr/
  marketing/
  sales/
  org/acme-demo.json
```
