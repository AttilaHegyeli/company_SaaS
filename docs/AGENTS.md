# Agents

## Shared pattern

Every specialist LangGraph:

```text
observe → reason → tool_call → draft_proposal → emit_approval → END
```

- At `emit_approval`, write proposal artifact, open Nest approval, **stop**
- Nest owns human decision and side-effect execution
- Tools use dual mode: `DEMO` | `LIVE` with identical port contracts
- DEMO adapters read `packages/demo-fixtures`

```text
ToolMode = DEMO | LIVE
tool.execute(mode, args) -> ToolResult
```

## Finance Agent

| | |
|---|---|
| **Mission** | Detect payment anomalies; propose flags, reminders, or payment actions |
| **DEMO inputs** | 20 payments; exactly 2 anomalies (see [DEMOS.md](./DEMOS.md)) |
| **Outputs** | `flag_invoice`, `request_payment_review` |
| **Forbidden without approval** | Capture payment, issue refund, change bank details |

## HR Agent

| | |
|---|---|
| **Mission** | Contracts, employee records, access/rights proposals |
| **DEMO inputs** | Employees, contract end dates, access matrix |
| **Outputs** | `remind_contract_renewal`, `propose_role_change`, `propose_access_grant` |
| **Forbidden without approval** | Grant production access, send binding contracts |

## Comms Agent

| | |
|---|---|
| **Mission** | Triage emails/messages; extract project requests; draft replies |
| **DEMO inputs** | Mock mailbox with project-request threads |
| **Outputs** | `send_email` draft, `create_project_brief` artifact |
| **Forbidden without approval** | Send any outbound message |

## Marketing Agent

| | |
|---|---|
| **Mission** | Draft social posts from brand kit; schedule proposals |
| **DEMO inputs** | Brand kit + 5 draft posts |
| **Outputs** | `publish_social`, `schedule_social` |
| **Forbidden without approval** | Publish to live networks |

## Sales / BD Agent

| | |
|---|---|
| **Mission** | Research companies/projects matching ICP; build dossiers; draft outreach |
| **DEMO inputs** | ICP + seeded mock web pages |
| **LIVE expansion** | Tavily / Serper search API |
| **Outputs** | `create_lead_dossier`, `draft_outreach` |
| **Forbidden without approval** | Contact lead; commit CRM outreach |

## Briefing Assistant

| | |
|---|---|
| **Mission** | Rank and explain ended/pending jobs for the human |
| **Inputs** | Jobs + approval requests (+ artifact metadata) |
| **Outputs** | Briefing artifact; attention notifications |
| **Forbidden** | Approving on behalf of a human |

The Briefing Assistant does **not** bypass the Approval Gateway. It aggregates and ranks; humans still decide.
