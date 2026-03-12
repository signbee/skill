# signbee/skill

> AI agent skill for [Signbee](https://signb.ee) — send documents for e-signing in one API call.

## Install

```bash
npx skills add signbee/skill --skill signbee -g
```

## What this does

Teaches your AI agent how to use the Signbee API to send documents (NDAs, contracts, agreements) for e-signing. Both parties sign, and everyone gets a tamper-proof signed PDF.

## Quick example

Ask your AI agent:

> "Send an NDA to bob@acme.com from me (alice@startup.com)"

The agent will call the Signbee API, draft the document in markdown, and trigger the full signing flow — no extra setup needed.

## Links

- [Signbee](https://signb.ee)
- [API Docs](https://signb.ee/llms.txt)
- [skills.sh](https://skills.sh)
