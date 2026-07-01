# Safety Rules

The automation is deliberately constrained. These boundaries are explicit and documented — not implied by implementation.

---

## What the system may do automatically

- Save input to the intake landing folder
- Route input based on type (text vs binary)
- Classify input and create or update ordinary knowledge notes
- Add invoices or bills to the admin dashboard as "assumed open"
- Propose reminders based on due dates
- Answer questions using knowledge-base context
- Send intake confirmations via Telegram

---

## What the system must never do automatically

| Action | Rule |
|--------|------|
| Make payments | Never — all financial actions require human confirmation |
| Send emails or external messages | Never — only Telegram intake confirmations are sent automatically |
| Delete files | Never — deletion requires explicit human instruction |
| Modify core configuration files | Never — instruction files, roadmaps, and architecture documents require explicit approval |
| Make legal or financial decisions | Never |
| Publish private content | Never — knowledge base content stays private unless deliberately exported |
| Write to a calendar | Never without explicit confirmation per event |
| Take any action outside the intake boundary | Never — n8n writes only to the intake folder |

---

## Write boundary

n8n may write **only** to the intake landing zone (`raw/Inbox`).

It does not have write access to:
- Other parts of the knowledge base
- The admin dashboard
- The payments tracker
- Project files
- Configuration or instruction files

All further routing happens during the supervised processing step.

---

## Protected files

These files may not be modified without explicit human approval:

- Root `CLAUDE.md` and project `CLAUDE.md` (instruction files)
- `ROADMAP.md` (the plan)
- `ARCHITECTURE.md` (the technical design)
- `PROCESS.md` (the working method)
- Any playbook file

The AI may flag that a protected file needs updating and explain why — but it must stop and wait for approval before making any change.

---

## Credential handling

- All credentials (Telegram bot token, Anthropic API key, Google Drive OAuth) are stored in n8n's credential store
- Credentials are never committed to version control
- Any workflow export is sanitised before publishing: all tokens, IDs, and keys replaced with named placeholders
- Private-not-for-github content (screenshots with local paths, etc.) is tracked in `.gitignore`

---

## Privacy boundary for this repository

This repository contains only:
- Code and configuration (sanitised)
- Documentation
- Example files using fictional data
- Screenshots using fictional / example data

It does not contain:
- API keys, tokens, or credentials
- Real invoices or financial documents
- Personal screenshots
- Private knowledge base content
- Real names, addresses, or payment details

---

## Human-in-the-loop principle

Every significant action passes through a human review step:

1. **Intake** — automatic (n8n routes and stores)
2. **Classification and filing** — supervised (human triggers, reviews output)
3. **Admin dashboard updates** — supervised (human reviews before acting)
4. **Corrections** — human instructs ("mark X as paid")
5. **Core file changes** — explicit approval required

This is not a workaround for technical limitations. It is a deliberate design choice: the system is trusted to classify and propose, not to decide and act.
