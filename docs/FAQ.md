# Frequently asked questions

[Back to the project overview](../README.md)

## What is Prometheus for?

Prometheus is a private personal assistant that brings conversation, saved information and connected everyday tools into WhatsApp. Its purpose is to let the owner ask for information and controlled actions in one place, while server-side workflows handle schedules, integrations and persistence.

It can help manage personal tasks, reminders, mail, calendar events and documents. It is not an autonomous marketing department or a public customer-support service.

## Can I download or use it?

No public application, bot access, workflow exports or source-code package is offered. This repository is a project showcase. The implementation is private and proprietary; no release date, free distribution or public service is announced.

## Does my computer need to stay on?

No. The private deployment runs on a server. Enabled workflows can continue while the owner's computer is closed, subject to server availability, credentials, AI limits and provider rules.

Turning the assistant OFF stops new assistant activity; closing a browser or computer does not. Backups and infrastructure checks can continue independently. Already-running work may still finish after OFF, and requests sent while OFF are not a guaranteed backlog for later replay.

## What does the WhatsApp 24-hour window mean?

The window runs from the owner's **latest message to the bot**. A new owner message opens or resets it. A bot message does not reset it, and publishing or restarting the workflow does not reset it. Outside that window, ordinary free-form messages require a different delivery route using approved templates. This is a delivery-permission rule, separate from billing. [Official WhatsApp messaging policy](https://whatsappbusiness.com/policy/).

In the accepted configuration, optional paid-template sending is OFF. Reminder and email notices can remain deferred until the owner messages the bot again; task check-ins are reconsidered on later eligible ticks. Delivery schedules, quiet hours and other guards still apply. It is not an urgent-alert configuration for an owner who has been silent for more than a day.

## Has delivery after 24 hours been tested?

Standalone approved-template delivery after a genuinely closed window has passed. That proves the tested template/provider route, not all automatic notification workflows.

Automatic positive-budget acceptance across **reminder, email and task** routes remains unperformed and disabled by owner choice. Completing the paid-OFF personal assistant did not require another paid test or another 24-hour wait.

## What does it cost?

This showcase offers no public subscription or purchase price. Operating the private system involves separate infrastructure and provider costs:

| Cost area | Boundary |
| --- | --- |
| Server, mailbox and domain | Existing recurring subscriptions can continue while the assistant is OFF. |
| Default AI route | Uses the configured Codex subscription and its shared usage allowance; it is not unlimited inference. |
| Optional paid AI route | OpenRouter is explicitly selected, never a silent fallback. Its charges are separate from WhatsApp controls. |
| Research | Uses the configured search account and its allowance; account-specific billing remains separate. |
| Local speech and embeddings | No separate external transcription/embedding API for these local steps; server resources still have a cost. |
| WhatsApp delivery | Depends on message category, recipient market, allowances and current provider rates. |

**Pricing checked 10 October 2026:** Meta's FAQ states that, effective 1 October 2026, each business phone number gets its first **1,000 delivered service messages per month free**, with charges beginning at the 1,001st. This is not 1,000 free template messages. Template categories have separate pricing. [Official WhatsApp pricing FAQ](https://whatsappbusiness.com/resources/faq/).

The local paid-template control is OFF with zero allowances, but it **does not stop ordinary service-message billing beyond that allowance** or enforce an account-wide spending cap. This repository does not report a live account balance or outstanding invoice. Check current provider rates and account usage before assuming a request is free.

## Can it do anything an AI model can describe?

No. Tools, account permissions, confirmation rules, queue capacity and input budgets define the available operations. A convincing answer is not evidence that an external action succeeded.

Important scope limits include no arbitrary shell/browser control, no bulk or attachment email, no permanent deletion, no unlimited memory and no independent execution of the task list. The [capability guide](CAPABILITIES.md) gives the supported formats and operations.

## Is it connected to Meta Business Suite?

WhatsApp transport uses the official Meta Cloud API. That does not provide access to Facebook/Instagram inboxes, comments, Business Suite labels, orders or delivery tracking. Those capabilities are outside this personal assistant's accepted scope and would need separate integrations and permission checks.

The marketing team and the automation control centre remain separate projects. Prometheus's local health checks do not replace a dashboard for all automations.

## What happens if something fails?

Inspect the saved task, event, document or mail result before repeating an action. A tool operation can succeed before the final chat reply fails. Uncertain sends are held for review rather than automatically replayed.

Authentication, quota or provider failures can require operator repair. Encrypted backups and an isolated restore have been checked, but production replacement still needs recovery keys, account reconnection and reconciliation of later external actions. See [control and reliability](CONTROL-AND-RELIABILITY.md).

## Is it finished?

The agreed **personal-assistant configuration is complete as of October 2026**, with WhatsApp as the main chat and optional paid notifications OFF. That is a scoped acceptance result, not a promise of uninterrupted operation, commercial multi-user readiness or every possible future integration.

## Where can I follow the project?

Follow this repository for showcase documentation and join the author's community at [discord.gg/ksahub](https://discord.gg/ksahub). Repository updates are not an announcement of public software availability unless explicitly stated.
