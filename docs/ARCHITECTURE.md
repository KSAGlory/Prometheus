# Architecture

[Back to the project overview](../README.md)

Prometheus combines an AI worker with an always-on integration layer. The AI interprets requests and proposes tool use; the workflow system authenticates the owner, enforces scope, talks to providers and records outcomes. An AI chat alone does not supply this messaging, scheduling and persistence infrastructure.

This is an architectural description of the private implementation, not a deployment guide or a collection of reusable workflows.

## System layers

| Layer | Responsibility |
| --- | --- |
| **WhatsApp transport** | Receive official Meta Cloud API events, authenticate signed input, restrict owner access and preserve the destination for replies. |
| **n8n orchestration** | Route text, voice and attachments; invoke connected tools; enforce action guards; coordinate schedules, results and error handling. |
| **Private AI queue** | Persist admitted work, run a bounded serial worker and record terminal results. Default AI work uses the configured Codex subscription allowance. |
| **Specialist workers** | Provide bounded Luna, Sol and Astra assistance without recursive delegation or independent authority over external accounts. |
| **Personal integrations** | Access the connected Zoho mailbox, Google Calendar, Google Docs and Google Drive through private provider credentials. |
| **Persistent data** | Store tasks, notes, conversation records, profile settings, approvals, outboxes and queue/budget receipts. |
| **Local processing** | Transcribe voice, prepare PDF/text inputs and generate embeddings for semantic retrieval. |
| **Operations** | Apply stop controls, bounded retention, local health checks, encrypted backups and operator-led recovery. |

## Request lifecycle

1. **Receive and authenticate.** A message is associated with the paired owner and original chat. Media identifiers, supported type, size and content integrity are checked before processing.
2. **Prepare the input.** Text enters the assistant route; voice is transcribed locally; supported attachments are converted into bounded reading context.
3. **Admit bounded AI work.** Durable queue admission and iteration limits constrain the request. Incoming attachment-reading routes do not expose action tools.
4. **Retrieve context or use tools.** The assistant can look up saved information and invoke permitted integration operations. Tool inputs remain subject to owner and action controls.
5. **Confirm where required.** Email sends and supported Trash actions stop at a frozen preview until a separate valid typed confirmation arrives.
6. **Record and report.** Tool results, approvals and delivery acknowledgments support a reply in the originating conversation and later inspection of saved state.

Provider acceptance and the final chat reply are separate events. If the reply fails after a tool action succeeded, the action must not simply be repeated.

## Storage and memory

**PostgreSQL** holds the assistant's native records, with **pgvector** supporting semantic retrieval. Local **SQLite** ledgers keep durable queue state, budget reservations, attempt records and consumed confirmations where used by the private services.

Recent conversation context is supplemented by saved notes, semantic retrieval and bounded summaries of older conversations. This is persistent assistant state, not unlimited context for the model or a permanent copy of every original attachment.

Private execution history is bounded to roughly **seven days and 10,000 records** under the accepted retention settings. Terminal AI jobs also have bounded retention. Tasks, notes and other saved native records have their own lifecycles; they are not all deleted merely because execution history is pruned. Retention limits also mean duplicate-detection history is not an eternal audit trail.

## Local processing and external services

Voice processing uses **Speaches/faster-whisper** locally. Semantic indexing uses **Ollama and EmbeddingGemma** locally, with 768-dimensional vectors. PDFs use bounded text extraction and rendered pages for model-assisted reading.

Local preprocessing avoids a separate external transcription or embedding API for these steps. It does not make the whole assistant offline: configured AI inference, WhatsApp, search, mail and Google tools still depend on external services. Those services receive the information required for the requested operation.

The default AI route uses an existing Codex subscription and its shared usage limits. **OpenRouter is a separate, explicitly selected paid route**, not an automatic fallback when the default route fails or reaches a limit. The paid WhatsApp notification control does not cap OpenRouter usage.

## Schedules and notification delivery

Reminders, mail polling, task check-ins and conversation summarization use separate schedules with their own eligibility checks. They are not all long-running AI requests. New-mail and task notices can be generated without AI inference.

Persistent outbox and attempt records separate “notice is due” from “provider accepted delivery.” Reminder leases and acknowledgment checks support recovery of due work. WhatsApp destination data remains attached to the notice rather than being inferred from whichever channel happens to be available.

With optional paid-template sending OFF, an expired WhatsApp messaging window can defer eligible background notices. A new owner message can reopen the window; the normal delivery schedule and other guards still apply.

## Capacity and stopping work

The accepted AI worker processes one job at a time, with a bounded queue of up to **100 queued/running jobs**. Production n8n execution concurrency is configured at **two**; that setting is not a universal cap on every manual execution or helper call. Resource and storage checks can refuse new admission rather than accepting unbounded work.

The count of saved workflows is not the main capacity measure. Reliability depends on concurrent work, model latency, memory, storage, provider limits and queue age. Larger workloads would need measured sizing and queue design before increasing these limits.

The owner control stops new assistant work and scheduled processing. Closing the desktop computer does not stop the server. An execution already in progress may finish after the control is turned OFF; stopping new admission is not cancellation or rollback of an accepted external action.

## Recovery boundary

Daily encrypted backups cover private workflow definitions, native records, credential material and relevant queue/confirmation ledgers. Consistent backup collection pauses new work and coordinates running services before encryption and remote storage verification.

A current isolated restore has checked saved records and local processing without live provider access. Replacing a production server still requires operator work: recovery-key access, model availability, account reconnection and reconciliation of external actions that occurred after the backup. This is recoverability, not automatic failover or high availability.

See [control and reliability](CONTROL-AND-RELIABILITY.md) for the tested recovery scope and remaining limits.
