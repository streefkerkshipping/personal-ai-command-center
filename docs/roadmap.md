# Roadmap

## Current status — v2

- ✅ Text intake via Telegram (live)
- ✅ Photo and file intake via Telegram (live — added v2)
- ✅ AI classification using Claude with structured Markdown output
- ✅ Telegram confirmation after each intake
- ✅ Supervised processing: classification, routing, knowledge base updates
- ✅ Admin dashboard: payments, deadlines, traffic-light grouping
- ✅ Action workboard: one self-contained card per urgent item
- ✅ Shared cost tracking with single source of truth design

---

## Known issues being fixed

- [ ] **Empty message filter** — when a Telegram message arrives with no text and no file (e.g. an empty update), the workflow fires and creates an empty file. Fix: add a filter node at the start of the workflow that discards empty messages.
- [ ] **Markdown formatting standardisation** — the AI-generated Markdown files have inconsistencies in section names and fields. Fix: tighten the classifier prompt in n8n.

---

## Planned modules

### Stability (next)
1. Fix empty message filter
2. Standardise Markdown output formatting
3. Export sanitised n8n workflow template for portfolio

### Voice notes
4. Research Whisper integration in n8n
5. Build: voice note → transcript → classify → store

### Knowledge-base Q&A
6. Answer questions via Telegram from knowledge-base context ("What should I do today?", "What deadlines are coming up?")
7. Answer from structured files: task lists, admin dashboard, project logs

### Calendar integration
8. Read-only: show upcoming calendar events in responses
9. Write: propose reminders based on invoice deadlines (with confirmation)

### Automation
10. Schedule automated inbox processing (3× per day or on arrival)
11. Schedule nightly consolidation: cross-links, deduplication, index updates

---

## Later / backlog

- Multi-photo grouping: Telegram sometimes sends a multi-photo message as separate events — detect and group them
- Duplicate detection: flag when the same document has already been ingested
- n8n self-hosted option: evaluate when Cloud tier limits are approached
- Export workflow as a reusable template with setup guide

---

## Build principle

**Project-first, skill-first.** Each new module is added when a real need appears in daily use — not to collect features. Research first, build second, automate last.
