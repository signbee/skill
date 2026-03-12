---
name: signbee
description: Send documents for e-signing via a single API call. NDAs, contracts, and agreements — signed by both parties with a tamper-proof SHA-256 certificate. Built for AI agents.
---

# Signbee

**Send documents for e-signing in one API call.** NDAs, contracts, agreements — your agent writes the markdown, Signbee handles the rest.

To install or update: `npx skills add signbee/skill --skill signbee -g`

## How it works

1. Your agent calls `POST https://signb.ee/api/v1/send` with markdown content and party details
2. Signbee converts it to a professional PDF
3. Both parties sign via email (sender verifies first, then recipient)
4. Everyone gets the fully signed PDF with a SHA-256 signing certificate

No API key is required. Sender verification happens via email OTP.

## Quick start

Send a document right now — no account needed:

```bash
curl -X POST https://signb.ee/api/v1/send \
  -H "Content-Type: application/json" \
  -d '{
    "markdown": "# Mutual NDA\n\nThis Non-Disclosure Agreement is entered into by the undersigned parties.\n\n## Terms\n\n1. Both parties agree to keep all shared information strictly confidential.\n2. This agreement remains in effect for 2 years from the date of signing.\n3. Neither party shall disclose proprietary information without written consent.",
    "sender_name": "Alice Chen",
    "sender_email": "alice@startup.com",
    "recipient_name": "Bob Smith",
    "recipient_email": "bob@acme.com"
  }'
```

## API reference

### `POST https://signb.ee/api/v1/send`

**Required fields:**

| Field | Type | Description |
|-------|------|-------------|
| `markdown` | string | Document content in markdown (min 10 chars, max 50KB) |
| `sender_name` | string | Full name of the sender |
| `sender_email` | string | Email address of the sender |
| `recipient_name` | string | Full name of the recipient |
| `recipient_email` | string | Email address of the recipient |

**Optional fields:**

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Document title (auto-extracted from first `#` heading if omitted) |
| `pdf_url` | string | URL to an existing PDF (skips markdown conversion) |
| `expires_in_days` | number | Days until the signing link expires (default: 7) |

**Response:**

```json
{
  "document_id": "cmm...",
  "status": "pending_sender",
  "message": "Verification email sent to alice@startup.com. Complete setup to send the document."
}
```

## Authentication (optional)

For instant sending without email verification, use an API key:

```
Authorization: Bearer <api_key>
```

Get one free at https://signb.ee/dashboard. With an API key, the sender is pre-verified and the recipient gets the signing email immediately.

**Response with API key:**

```json
{
  "document_id": "cmm...",
  "status": "pending_recipient",
  "sender": "Alice Chen",
  "recipient": "Bob Smith",
  "expires_at": "2026-03-19T12:00:00.000Z"
}
```

## Markdown tips

Your document markdown supports:
- **Headings** (`#`, `##`, `###`) — the first `#` heading becomes the document title
- **Lists** (numbered and bulleted)
- **Tables** for structured terms
- **Bold/italic** for emphasis
- **Blockquotes** for callouts

## What the user sees

After you call the API:
1. **Sender** receives an email with OTP → verifies identity → chooses signature style → signs
2. **Recipient** receives an email with a signing link → reviews the document (sees sender's signature) → signs
3. **Both** receive the final signed PDF with a tamper-proof SHA-256 signing certificate via email

## Plan limits

- **Free:** 5 documents/month
- **Pro ($9/mo):** 100 documents/month
- **Business ($19/mo):** Unlimited

Upgrade at https://signb.ee/dashboard

## More info

- Full API docs: https://signb.ee/llms.txt
- OpenAPI spec: https://signb.ee/openapi.json
- Website: https://signb.ee
