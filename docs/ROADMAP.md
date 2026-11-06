# Roadmap

## Feasibility snapshot

| Scope | Realistic? | Timing |
|---|---|---|
| Next.js + NestJS + FastAPI/LangGraph + Postgres + Redis + Mongo | Yes | Weeks 1–3 |
| Approval Inbox + audit + Finance demo (mock data) | Yes — first sellable story | Weeks 3–6 |
| Comms / HR / Marketing / Sales demos (mock connectors) | Yes | Months 2–4 |
| Briefing Assistant daily packet | Yes | After ≥2 specialist agents |
| Live connectors (Gmail, Stripe, LinkedIn, HRIS) | Yes, one at a time | After demos prove value |
| SSO OIDC | Yes | Hardening phase |
| SAML, air-gapped LLM, full customer factory | Yes, later | Enterprise packaging |
| Unsupervised irreversible actions | No — product forbids | Never as default |

**MVP cut line:** Foundation + Approval Gateway + Finance Demo 1.

## Phase A — Documentation

- [x] `docs/*` published
- [x] Root `README.md` describes what we build

## Phase B — Foundation

- [ ] SOLID folder tree as in [STRUCTURE.md](./STRUCTURE.md)
- [ ] Import-lint / import-linter gates in CI
- [ ] Monorepo boots via Compose
- [ ] Nest / Next / FastAPI health OK
- [ ] Migrations create Postgres tables
- [ ] Redis / Mongo reachable

## Phase C — Approval Gateway + Inbox

- [ ] Create pending approval via API
- [ ] Approve / reject / edit-approve works
- [ ] Audit events written
- [ ] Inbox UI lists pending items

## Phase D — Finance Demo 1

- [ ] Seed 20 payments / 2 anomalies
- [ ] Agent run produces approval packet
- [ ] Approve executes DEMO side effect idempotently
- [ ] 15-minute demo script passes cold

## Phase E — Runtime hardening

- [ ] Queue workers restart-safe
- [ ] HITL resume tokens reliable
- [ ] DEMO / LIVE tool switch per connector

## Phase F — Expand demos 2–6

- [ ] Comms, HR, Marketing, Sales mock demos
- [ ] Briefing Assistant ranking packet

## Phase G — Enterprise packaging

- [ ] OIDC SSO
- [ ] Helm + Terraform baseline
- [ ] Customer install runbook
- [ ] Internal security checklist signed off

## Immediate next engineering step

1. Scaffold Foundation (Compose + three apps + schemas) using SOLID tree  
2. Implement Approval Gateway + Finance Demo 1 only  
3. Do **not** start live Stripe / Gmail / LinkedIn until Demo 1 is stable  

## Team assumption

1–2 strong engineers (or founder + AI-assisted implementation) can ship Demo 0→1. A polished five-agent mock fleet is about one quarter.
