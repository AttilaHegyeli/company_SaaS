# Data Model

## Job status machine (PostgreSQL)

Happy path:

```text
queued → running → awaiting_approval → executing → done
```

Alternate terminals: `rejected`, `failed`, `cancelled`

### Allowed transitions

| From | To |
|---|---|
| `queued` | `running` |
| `running` | `awaiting_approval`, `failed`, `cancelled` |
| `awaiting_approval` | `executing`, `rejected`, `cancelled` |
| `executing` | `done`, `failed` |
| terminals | _(none)_ |

## PostgreSQL tables (v1)

### organizations

`id`, `name`, `slug`, `created_at`

### users

`id`, `org_id`, `email`, `name`, `password_hash` (local bootstrap only), `sso_subject`, `is_active`, `created_at`

### roles

Enum values: `owner`, `approver`, `operator`, `auditor`, `agent_service`

### user_roles

`user_id`, `role`

### agent_definitions

`id`, `key` (`finance` | `hr` | `comms` | `marketing` | `sales` | `assistant`), `display_name`, `is_enabled`, `config_json`

### jobs

`id` (UUID), `org_id`, `agent_key`, `title`, `status`, `priority`, `trigger_type` (`manual` | `cron` | `webhook` | `agent_handoff`), `input_json`, `proposal_artifact_id`, `error_message`, `created_by`, `created_at`, `updated_at`, `idempotency_key` (unique)

### approval_requests

`id`, `job_id` (unique), `org_id`, `status` (`pending` | `approved` | `rejected` | `edited_approved`), `risk_level` (`low` | `medium` | `high` | `critical`), `summary`, `recommendation`, `proposed_actions_json`, `decision_packet_json`, `assignee_user_id`, `decided_by`, `decision_note`, `edited_payload_json`, `sla_due_at`, `created_at`, `decided_at`

### connector_accounts

`id`, `org_id`, `provider` (`stripe` | `gmail` | `outlook` | `linkedin` | `x` | `hris` | `web_search`), `mode` (`demo` | `live`), `status`, `secret_ref`, `metadata_json`

### audit_events

`id`, `org_id`, `actor_type` (`user` | `agent` | `system`), `actor_id`, `action`, `entity_type`, `entity_id`, `before_json`, `after_json`, `ip`, `created_at`

Append-only. Application DB role: `INSERT` + `SELECT` only (no `UPDATE` / `DELETE`).

### policies

`id`, `org_id`, `key`, `rules_json`

Examples:

- `finance.max_auto_propose_amount`
- `approvals.finance_requires_role=approver`

## MongoDB collections

### artifacts

`_id`, `org_id`, `job_id`, `type` (`email` | `ledger_snapshot` | `research_page` | `social_draft` | `decision_packet` | `other`), `content`, `content_type`, `redaction_flags`, `created_at`

### run_traces

`_id`, `org_id`, `job_id`, `agent_key`, `steps[]` (tool name, args hash, latency_ms, token_usage), `model`, `created_at`

PII fields redacted where policy requires.

## Redis keys

| Key / channel | Purpose |
|---|---|
| `agent_runs` | Queue payload: `job_id` |
| `lock:job:{job_id}` | Concurrency lock |
| `approvals:{org_id}` | Pub/sub for inbox updates |
| `hitl:resume:{approval_id}` | Human resume token |

## Data ownership summary

| Store | Owns |
|---|---|
| PostgreSQL | Users, roles, jobs, approvals, connectors, audit, policies |
| Redis | Queues, locks, live notifications |
| MongoDB | Artifacts, run traces |
