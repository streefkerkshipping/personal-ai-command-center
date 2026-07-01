# Architecture

## Overview

The system has three distinct layers. Each layer has a single job and a clear boundary.

```
[Telegram bot]          — user input: text, photo, or file
      │
      ▼
[n8n — routing layer]   — receive, route, store, confirm
      │
      ▼
[Google Drive]          — raw/Inbox — the intake landing zone
      │
      ▼
[Supervised processing] — classify, file, update knowledge base
      │
      ▼
[Knowledge base]        — Markdown files: wiki, admin, projects
```

---

## Layer 1 — Input (Telegram)

Telegram serves as the mobile input interface. The user sends messages to a private bot. The bot accepts:

- **Text** — ideas, notes, tasks, reminders
- **Photos** — screenshots, invoices, handwritten notes, whiteboard captures
- **Files** — PDFs, documents, images

Telegram sends a webhook event to n8n for every message.

---

## Layer 2 — Routing (n8n)

n8n receives the Telegram webhook and decides what to do with it.

**Workflow v2 (current):**

```
Telegram Trigger (Updates: message)
    │
    ▼
If node — does the message have a text field?
    │
    ├── true  (text) ──► Google Drive: Create file from text
    │                          │
    └── false (binary) ──► Google Drive: Upload file
                                │
                           Send text message (Telegram confirmation)
```

**Why this design:**

- The If-node is the correct routing primitive. Telegram photo messages carry no `text` field — without branching, binary input produces an empty file.
- n8n has write access **only** to the intake folder. It cannot modify other parts of the knowledge base.
- No business logic lives in n8n. It routes and stores — nothing more.

**Credentials** are stored in n8n's credential store, not in the workflow definition. The exported workflow uses placeholders (see `workflows/sanitized-n8n-workflow.json`).

---

## Layer 3 — Processing (supervised)

After intake, a human-triggered processing step reads the inbox and routes each item to the right place.

**For each item:**
1. Read the content (text, or image using AI vision)
2. Classify: type (idea, invoice, document, note), destination folder, urgency
3. Move source file to the correct `raw/` subfolder
4. Create or update relevant knowledge pages (notes, admin records, project pages)
5. Log what was done
6. Flag any assumptions made

This step is **supervised** — it runs on demand, not automatically. The output is reviewed before any changes are committed to the knowledge base.

**Why supervised?**

Fully automated processing is fast but brittle for personal knowledge. Edge cases — ambiguous invoices, duplicate items, entries that span multiple categories — are better handled with a human in the loop. The processing step is fast enough to run manually.

---

## Admin module

When the processed item is a bill, invoice, or financial document, the processing step additionally:

- Extracts sender, subject, amount, and due date
- Adds the item to a payments dashboard with a traffic-light status (🔴🟠🟢)
- Calculates and recalculates urgency based on due date
- If applicable, calculates shared costs (e.g. for items split between housemates)

See `docs/admin-dashboard.md` for details.

---

## Tool roles

| Tool | Role | Boundary |
|------|------|----------|
| Telegram | Input interface — text, photo, file; confirmation output | User-facing only |
| n8n | Routing — webhook → route → store → confirm | Write access to intake folder only |
| Claude (Anthropic) | Classification — type, destination, rationale | Reads and proposes; does not execute |
| Google Drive | Storage — intake landing zone and knowledge base storage | Structured folder hierarchy |
| Markdown / Obsidian | Knowledge base — all knowledge as plain text files | Local, version-controlled |

---

## Design decisions

**No separate OCR node.** n8n stores the image file; the AI model reads it natively during supervised processing using built-in vision. Building a dedicated OCR pipeline adds complexity for something the model already handles.

**Single intake folder.** n8n writes all intake to one folder (`raw/Inbox`). This simplifies write permissions and makes the processing step predictable — one place to look.

**Text-first MVP, binary added second.** The system launched with text-only intake and added binary support once the text path was stable. This kept the first working version small and testable.

**Human-in-the-loop is a feature, not a limitation.** Automated processing would be faster, but the supervised step allows the system to handle edge cases gracefully. The trust this builds matters more than the speed saved.
