# Safety + Redaction (Non-negotiable)

This guide is written from real usage, but it must never leak anything sensitive.

## Why this exists
Two audiences read this book:
- humans (friends/colleagues), and
- AI agents (which may copy patterns aggressively).

So the book must be safe **by construction**.

## Never include
- Personal info about the operator or the assistant
- Phone numbers, email addresses, physical addresses
- API keys, tokens, secrets, passwords
- WhatsApp group IDs/JIDs, message IDs
- Tracking numbers, invoice IDs, PNR/AWB, transaction IDs
- Links/URLs copied from internal systems

If a detail could identify a person, a company, or a system — remove it.

## Always do instead
### 1) Paraphrase (no quotes)
- Don’t copy chat messages verbatim.
- Summarize the **use case → behavior → outcome**.

### 2) Use placeholders
Use a consistent placeholder vocabulary:
- Identity: `<USER>`, `<TEAM>`, `<GROUP_NAME>`
- Contact: `<YOUR_EMAIL>`, `<PHONE_NUMBER>`
- Secrets: `<API_KEY>`, `<TOKEN>`, `<PASSWORD>`
- Ops IDs: `<ORDER_ID>`, `<TRACKING_ID>`, `<TICKET_ID>`

### 3) Prefer “minimal sufficient detail”
If a detail isn’t necessary to teach the pattern, omit it.

## Sentinel rules (checker)
Before anything is considered publishable, Sentinel enforces:
- placeholders-only
- no links
- no identifiers
- no quotes

If uncertain, it deletes content rather than risking leakage.

## Redaction examples
Bad:
- “Message `<PHONE_NUMBER>` and ask them to…” (even if it’s your own number)
- “Here’s the tracking link…”
- “Use this token: …”

Good:
- “Notify the owner (<USER>) using your normal internal channel.”
- “Check the courier status via the official tracking portal.”
- “Store secrets in a secure secret manager and reference them via `<TOKEN>`.”
