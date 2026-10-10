# Control and reliability

[Back to the project overview](../README.md)

Prometheus is designed around an identifiable owner, explicit action scope and recorded outcomes. These controls reduce predictable mistakes; they are not a guarantee that an AI model or external provider never fails.

## Owner authority

The private personal deployment restricts requests to its paired owner. Receiving an attachment, email or search result does not make its author an authorized operator. Instructions inside retrieved content cannot replace the owner's request or alter provider routing and permissions.

Attachment-reading turns have no action tools. Profile and style preferences can change permitted presentation settings, but cannot expand authority. Specialist workers remain bounded by the assistant's tool and delegation limits.

## Preview and confirmation

| Operation | Confirmation after preview |
| --- | --- |
| Send or reply to email | `CONFIRM SEND <nonce>` |
| Move an exact email to recoverable Trash | `CONFIRM EMAIL TRASH <nonce>` |
| Move an exact Drive file to recoverable Trash | `CONFIRM TRASH <nonce>` |

The assistant supplies a real action-bound token. A confirmation must arrive in a **separate fresh owner text message within 15 minutes**. The placeholders above are explanatory, not valid tokens. A voice message, quoted command or instruction embedded in a file does not satisfy this requirement.

Previews bind the target account/object and approved content to the operation. Expired, changed or already consumed confirmations are rejected rather than reused for a different action. Other permitted owner-requested edits, such as creating a task or updating a calendar event, do not all require these confirmation commands.

Recoverable Trash is not permanent deletion. Recovery depends on the provider's own retention behavior.

## Failures, duplicates and uncertainty

The workflow records attempts and acknowledgments around protected actions and notifications. Important distinctions remain explicit:

- **Queued** means work is waiting; it does not establish that a provider received it.
- **Provider accepted** means the provider acknowledged the operation; it does not independently establish recipient delivery or reading.
- **Failed** can occur after an earlier tool action succeeded. Saved state must be inspected before repeating the request.
- **Uncertain** means the result cannot safely be treated as either sent or unsent. The operation is held for review instead of being automatically replayed.

Reminder leases and acknowledgment checks handle due work and expired processing claims. Recovery does not reset consumed approvals or delivery markers just to make a test pass. A provider or AI failure does not authorize silent migration to another channel or paid AI provider.

AI authentication or quota problems can block or pause queue processing. Recovery requires reviewing the cause and pending work; failed jobs are not blindly replayed. Cancelling a job does not refund consumed AI usage or undo an external action already accepted.

## Stop and cost controls

The owner's stop control prevents new assistant work and scheduled processing. Already-running work may still finish and needs separate attention if cancellation is required. Turning the assistant OFF is not a rollback, and turning the computer OFF is not the same as stopping the server.

Optional paid WhatsApp notification controls include durable reservations, message/spend allowances and attempt records. In the accepted configuration the paid notification policy is **OFF**, with **zero allowances** and no connected automatic paid sender.

These controls apply to that optional template-delivery route. They are **not an account-wide Meta billing cap**, a hard stop for ordinary service messages after an allowance is exhausted, or a spending limit for an explicitly selected paid AI route. See [costs in the FAQ](FAQ.md#what-does-it-cost).

## Privacy and retained records

Credentials remain server-side and outside this showcase. Native credential storage is encrypted; private execution history, queued input and retained backups may still contain sensitive conversation or document content. Self-hosting provides infrastructure control, not a promise that external providers receive no data.

Retention is bounded and depends on record type. Deleting a saved note does not instantly scrub every earlier execution or backup. The public repository deliberately excludes implementation code, workflow exports, credentials, chat screenshots, account identifiers, private logs and recovery material.

## Backups and recovery

Daily backups coordinate services for a consistent snapshot, encrypt the archive before remote upload and verify the encrypted bytes after storage. The accepted retention keeps seven local copies and roughly 30 days of remote backup history.

The current recovery check restored native records, saved workflow definitions and five SQLite ledgers in an isolated environment. It checked owner login/credential decryption and local voice, PDF and semantic processing while keeping clone workflows and inference disabled and blocking production network access. Disposable recovery resources and plaintext staging were removed afterwards.

Recovery-key availability remains a prerequisite. The current local protected identity depends on the operator's Windows account and its usable encryption keys; portable offline key escrow has not been established. A new production server also needs model files, provider reconnection and reconciliation of activity since the snapshot. Restoring an old outbox without that reconciliation could repeat an external action.

Local health checks cover infrastructure and backup freshness. They are not a full automation-fleet dashboard or a guarantee of instant WhatsApp alerts for every missed schedule or outage.

## Verification scope

The October 2026 closeout completed the agreed personal configuration: existing integrations, WhatsApp as the main chat and optional paid notifications OFF.

| Evidence | What was established |
| --- | --- |
| **Owner-visible WhatsApp tests** | Text/voice replies and reminders, saved notes, task/subtask handling, calendar creation/rescheduling, document editing/read-back, original-file organization and separately confirmed recoverable Trash. |
| **Owner-visible information updates** | Incoming-email notice, pending-task check-in, image description, PDF summary and a later question using the saved reading. |
| **Saved mail action metadata** | Recorded accepted Zoho send/reply operations and consumed approvals; not independent recipient-delivery proof. |
| **64 notification/control checks** | Notification route validation, paid-OFF controls, reservations and attempt handling. Earlier route/payload checks overlap this suite. |
| **29 queue/reminder recovery checks** | 16 queue checks with controlled provider substitutes and 13 checks against native reminder scheduling/recovery logic. These are not 29 new live provider sends. |
| **Current isolated restore** | Matching fingerprints for 26 native tables, 22 stored workflow definitions/versions and five SQLite ledgers; credential recovery and local processing checks. |
| **Read-only final inspection** | Current assistant state, worker readiness, disabled paid policy and backup/health scheduling, without new messages or inference. |

Historical Telegram or Gmail tests do not substitute for current WhatsApp or Zoho evidence. Supported formats and guards have implementation checks; not every supported format has a separate owner screenshot. The private evidence remains outside this public repository.

Standalone approved-template delivery after a genuinely closed WhatsApp window has passed. **Automatic positive-budget delivery across reminder, email and task routes remains unperformed and disabled by owner choice.** It is an optional extension, not required for the accepted paid-OFF configuration.

These checks do not establish commercial multi-tenancy, automatic failover, uninterrupted availability, arbitrary-input correctness or perpetual provider permission. Normal maintenance, backup review and provider/account checks remain necessary.
