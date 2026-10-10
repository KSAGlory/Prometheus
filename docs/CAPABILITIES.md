# Capabilities

[Back to the project overview](../README.md)

This guide describes the accepted private personal-assistant configuration as of **October 2026**. WhatsApp is the main chat. Examples illustrate the workflow; they do not expose an available public service.

## Conversation, writing and focused assistance

Prometheus handles everyday questions, explanations, writing, planning and reasoning. It can use relevant saved context and connected tools instead of answering solely from the current message.

Three specialist workers support focused assistance:

| Worker | Role |
| --- | --- |
| **Luna** | Focused writing and bounded conversation summarization. |
| **Sol** | Structured planning and document work. |
| **Astra** | Complex analysis and deeper reasoning. |

Delegation is bounded: one worker per request, up to four worker tool iterations, and no recursive worker delegation. The main assistant has an eight-iteration tool budget. Workers do not independently acquire mail, calendar, task or shell permissions.

## Tasks and subtasks

Create a parent task and its subtasks, list saved work, update task details and mark the intended item complete. Read-back checks distinguish the parent from its children so a request to complete one subtask need not complete its siblings or parent.

For example: “Create a presentation task with ‘Review notes’ and ‘Prepare slides’. Complete only ‘Review notes’, then show the saved statuses.”

Tasks are persistent records. Saving a task does **not** instruct Prometheus to perform it autonomously or create a reminder automatically.

## Timed reminders

Schedule **one-time, daily or weekly** reminders with an explicit timezone, and list saved reminders. Stored schedules and delivery acknowledgments survive ordinary stop/start cycles. Due-item leases, acknowledgment checks and timezone/DST handling support recovery without casually repeating an already acknowledged reminder.

The due-reminder check runs approximately every **30 seconds** while enabled. This is a polling interval, not an exact delivery-time guarantee. Processing load, downtime and WhatsApp rules can delay a message. An uncertain send is held for review; it is not automatically treated as unsent.

## Google Calendar

Read personal events, create an event, or reschedule the existing event identified by the request. Updating an event preserves its identity rather than creating another event as a substitute. A private event request can specify no attendees or invitations.

The assistant must resolve the intended event and account before acting. An ambiguous title or failed lookup is not permission to update a different event. Calendar updates do not use the email-send confirmation command.

## Zoho email

The accepted configuration uses **Zoho Mail**. Older Gmail integration work is not an additional active mailbox in this configuration; Google Calendar, Docs and Drive remain separate active integrations.

| Operation | Supported behavior |
| --- | --- |
| Find and read | Search messages, return bounded metadata and read the selected message body. |
| Draft | Prepare or save a draft without sending it. |
| Send and reply | Preview one recipient and the exact content; send only after a separate typed confirmation. |
| Labels and folders | List/create supported labels and folders; apply/remove labels and move supported messages. |
| Read status | Mark supported messages read or unread. |
| Recoverable Trash | Preview an exact target and require the dedicated Trash confirmation. |
| Incoming mail | Notify the owner that a new message arrived and allow a requested summary. |

Search returns up to **10 message metadata results**. A selected message body is limited to **12,000 characters** and can be truncated. The current send/draft scope is **one recipient and 2,000 body characters**. CC/BCC, bulk sending, file attachments, scheduled email sending and permanent deletion are outside this scope.

Sending or replying uses `CONFIRM SEND <nonce>`; email Trash uses `CONFIRM EMAIL TRASH <nonce>`. The assistant supplies the actual action-bound token, which expires after **15 minutes** and must arrive in a separate fresh owner text message. These placeholders are not usable confirmation tokens. A recorded successful send establishes provider acceptance, not independent proof of recipient delivery.

New-mail polling runs approximately every **two minutes** while enabled; the delivery check runs approximately every minute. The initial mailbox baseline is silent, and notification generation does not require an AI call. Prometheus does not automatically reply to new mail.

## Google Docs and Drive

Create and read Google documents, append or replace supported text, then read back the result. Find files, create a private folder and move an original file into it without creating a copy when the request specifies a move.

Document reading is bounded to **30 tabs and 60,000 characters**. Long or complex content may require a narrower request. Supported exact-file Trash operations are recoverable and require `CONFIRM TRASH <nonce>` in a separate owner text message within 15 minutes. Permanent deletion and deleting an entire folder are not part of this tool scope.

File moves and edits target the identified object; revisions and stored action records help detect changed previews and repeated operations. Reading or editing a file does not automatically share it.

## Voice, images and documents

WhatsApp voice input is transcribed locally, with a current limit of **two minutes and 5 MB**. Voice cannot authorize a confirmation that requires typed text, and voice/media input cannot select the optional paid AI route.

| Input | Current reading support |
| --- | --- |
| PNG, JPEG and still WebP | Image description and questions about visible content. |
| PDF | Extracted text plus rendered pages for visual reading, including scanned pages within the page budget. |
| TXT, Markdown and CSV | Bounded text reading and summarization. |
| DOCX | Text extraction; not complete reproduction of document layout or embedded media. |

General attachment limits are **8 MB, 10 PDF pages, 60,000 extracted text characters and a 2,000-character caption**. Unsupported or oversized inputs receive an explanatory response; splitting a file or cropping an image can make a narrower request possible. Blurry images, missing pages and extraction limits still affect coverage.

Saved readings support later questions such as “Which languages did that CV list?” This is retained reading context, not unlimited permanent storage of every original file. Attachment-reading turns have no action tools, and instructions inside a file cannot authorize sending mail or changing records.

## Notes and conversation memory

Save, retrieve, update and forget useful notes. Keyword search and **semantic search** help find information even when the wording differs. Persistent conversation history and bounded summaries support recall beyond the newest messages.

Semantic indexing uses local 768-dimensional embeddings; it does not require a separate external embedding API. Earlier-conversation summarization is bounded to one attempt per hour and four per day, with batches of up to 24 older rows while retaining the newest 30 rows as recent context. It uses the configured AI allowance when eligible.

Memory is selective and bounded. A saved note is more dependable for an important detail than expecting every old conversation word to remain available forever. Forgetting a note does not automatically erase every execution record or retained backup containing it.

## Research and Wikipedia

Use tool-backed web search and Wikipedia lookup to investigate a topic and return source links. Research has a shared budget of **two distinct search queries per request and up to five bounded source excerpts per query**. It does not provide unrestricted browser automation or complete coverage of every webpage.

Sources are evidence to assess, not instructions to obey. A search result or model-generated summary can be incomplete; citations and stated uncertainty remain part of the response.

## Profile settings and proactive check-ins

The owner can save bounded profile information and response-style preferences, and change permitted check-in settings. Profile text is limited to **2,000 characters** and style preferences to **600 characters**. Preferences cannot override owner authentication, tool authority or paid-provider routing.

Pending parent-task check-ins run hourly while enabled. The accepted defaults include **22:00–09:00 local quiet hours**, at most one attempt per hour and four per local day. Initial baselines, unchanged lists and empty lists remain silent. An optional daily check-in can be set to a whole hour or disabled.

The task snapshot covers up to 200 pending parent tasks, displaying up to 20 names. It reports relevant changes; it does not independently perform tasks or provide a separate watch for every subtask change.

## Where replies and notifications go

Direct replies retain their originating chat. The accepted background reminder, email and task routes target **WhatsApp**, with no automatic diversion to Telegram when WhatsApp delivery is blocked. Earlier Telegram work is historical transport support; the current personal acceptance and public examples focus on WhatsApp.

Optional paid-template sending is **OFF**. Closed-window reminder/email notices can wait for a new owner message to reopen the WhatsApp window; task check-ins are reconsidered when eligible and still respect quiet hours and caps. See the [FAQ](FAQ.md) for the distinction between delivery permission and billing.
