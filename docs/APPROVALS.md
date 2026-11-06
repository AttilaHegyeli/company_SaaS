# Approval Gateway

The product differentiator. Agents never execute irreversible side effects alone.

## Decision packet

Every `approval_request` shown to the Approver includes:

1. **Agent** and **job title**
2. **Summary** (1–3 sentences)
3. **Evidence** (links to artifacts: invoice rows, email thread, dossier)
4. **Risk level** and why (`low` | `medium` | `high` | `critical`)
5. **Proposed actions** — structured list; each action has `tool`, `args`, `reversible`
6. **Recommendation** — `approve` | `reject` | `edit`
7. Human actions — **Approve** / **Reject** / **Edit then approve**

## Briefing Assistant

- Aggregates pending and recently decided approvals
- Ranks by risk, money impact, deadline (`sla_due_at`)
- Produces a daily/shift briefing packet in the Inbox
- Never auto-approves

## Side-effect execution (approve path)

1. NestJS validates RBAC + policy
2. Writes audit `approval.approved`
3. Sets job status `executing`
4. `SideEffectExecutor` runs each proposed action with idempotency key `job_id:action_index`
5. Writes audit per action
6. Sets job `done` (or `failed` with compensating notes)

## Reject path

- Job becomes `rejected`
- No side effects run
- Audit records `approval.rejected` with human note

## Edit-then-approve path

- Approver supplies `edited_payload`
- Status becomes `edited_approved`
- Side effects use the edited payload
- Audit stores both original proposal and edited payload

## SLA

- Each approval may set `sla_due_at`
- Aging / escalation hooks belong in Approvals application layer (notify assignee / owner)

## Ownership

All approval invariants live in `apps/api/src/modules/approvals/`.

Agent graphs may **open** approvals via internal API. They may not mark them approved.
