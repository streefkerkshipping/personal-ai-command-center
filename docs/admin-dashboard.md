# Admin Dashboard

The admin dashboard is an AI-maintained administration layer that transforms ingested documents into a live, colour-coded action overview.

---

## The problem it solves

When documents arrive — invoices, bills, letters, reminders — the usual result is a folder full of unread PDFs. Tracking what is due when requires opening each file, reading it, remembering the amount, and recalculating urgency every time.

The admin dashboard replaces that with a single view: what is urgent today, what needs action this week, and what is already handled.

---

## Traffic-light system

Every payment or obligation gets a single status colour, recalculated from its due date:

| Colour | Meaning | Threshold |
|--------|---------|-----------|
| 🔴 Red | Expired, due today, or critical | Deadline passed or ≤ 3 days |
| 🟠 Orange | Action needed this week | 4–7 days |
| 🟢 Green | On track or handled | More than 7 days, or paid |

The colour is not set manually — it is computed from the due date. As deadlines approach, items move from green to orange to red without any manual update needed.

![Dashboard example](../screenshots/admin-dashboard-voorbeeld.png)

*Example data only. The real dashboard contains private financial information and is never published.*

---

## What gets extracted from each document

When an invoice or bill is ingested, the AI extracts:

- **Sender** — which provider or institution sent it
- **Subject** — what the charge is for
- **Amount** — the total due
- **Due date** — when payment is required
- **Urgency** — calculated from due date
- **Source file** — which ingested document this row came from
- **Notes** — any relevant caveats (e.g. "automatic direct debit — no action unless disputing")

---

## Action workboard

Beyond the overview, a daily action workboard turns each urgent item into a **self-contained card**. The goal: complete a task without opening a single other file.

Each card contains:

| Field | Content |
|-------|---------|
| Action | What to do |
| Status | Traffic-light + short description |
| Deadline | When |
| Amount | If financial |
| Payment link | Direct URL where possible |
| Source file | Which ingested document this came from |
| Notes | Context, caveats, linked obligations |
| Checkboxes | Step-by-step sub-actions |

---

## Shared costs

When a household expense is split between multiple people, the split is tracked in a dedicated file — not copied across several notes.

**Design rule:** each shared amount appears in **exactly one place**. This prevents the contradictions that emerge when the same number is updated in five different files.

The AI adds new shared bills to this file, calculates each person's share, and recalculates the running total automatically. Corrections are made by instruction ("mark X as paid" → status updates to paid, date recorded).

---

## Weekly admin routine

Once a week, a single command:

1. Reads all recent admin documents
2. Recomputes every traffic light from current due dates
3. Lists all open actions sorted by urgency (red first)
4. Prepares the week's to-do items
5. Generates a ready-to-send message for shared costs

Administration becomes a 15-minute review rather than a scramble.

---

## Scope of the dashboard

The dashboard covers more than payments:

| Section | Contents |
|---------|---------|
| Payments | Bills due to external providers, grouped by urgency |
| Shared costs | Household expenses split with others |
| Documents | Index of all processed administrative documents |
| Emails | Incoming correspondence requiring a response |
| Admin tasks | One-off tasks that do not fit other categories |

---

## What the dashboard does not do

- It does not make payments
- It does not send emails or messages automatically
- It does not delete documents
- It does not overwrite its own history — completed items are moved to a "done" section with date, not deleted

---

## Transferable pattern

This pattern applies directly to small business operations:

- Supplier invoice tracking
- Client payment follow-up
- Regulatory deadlines (tax, permits, compliance)
- Staff expense tracking
- Shared team costs

The traffic-light design and action workboard are tool-agnostic and can be rebuilt in any Markdown-based system, Notion, Airtable, or a custom dashboard.
