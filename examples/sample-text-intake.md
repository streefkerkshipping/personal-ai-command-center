# Sample — Text Intake

This is an example of a Markdown file created by the intake pipeline when a text message is sent to the Telegram bot.

All data is fictional. No real names, chat IDs, API keys, or personal information are included.

---

## Generated file (example)

```markdown
---
date: 2026-07-01
from: example_user
type: idea
destination: raw/ideas/
status: unprocessed
---

## AI Classification

- **Type:** idea
- **Destination:** raw/ideas/
- **Rationale:** User describes a new feature or concept to explore later.
  No urgency detected. Routing to ideas folder for review.

## Original Message

Build a small demo that shows the invoice intake flow to a potential client.
Use the test invoices as props. 5-minute walkthrough.

## Notes

- Assumed no immediate action required
- Will be picked up during the next Feed the Brain run
```

---

## How this file is created

1. User sends: `Build a small demo that shows the invoice intake flow to a potential client.`
2. n8n receives the Telegram webhook (text message)
3. The If-node routes to **Create file from text** (Google Drive)
4. Claude classifies the message: type → `idea`, destination → `raw/ideas/`
5. The structured Markdown file is written to `raw/Inbox/`
6. Telegram sends back: `Ontvangen ✅ Opgeslagen in raw/Inbox. Status: unprocessed.`
7. During the next supervised processing run, the file is moved to `raw/ideas/` and a knowledge page is created or updated

---

## Classification categories (examples)

| Type | Destination | Examples |
|------|-------------|---------|
| `idea` | `raw/ideas/` | New project ideas, feature requests, hypotheses |
| `task` | `raw/projects/` | Action items, reminders, to-dos |
| `note` | `raw/reference/` | General reference, links, snippets |
| `admin` | `raw/legal/` | Bills, invoices, letters, government correspondence |
| `bible` | `raw/bible/` | Bible study notes, passages, reflections |
| `journal` | `raw/journals/` | Daily reflections, personal notes |
| `training` | `raw/training/` | Course notes, book summaries, learning notes |
