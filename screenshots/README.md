# Screenshots

All screenshots use sanitized example data only. No real credentials, tokens, personal information, private invoices, or real financial data are included.

| File | What it shows |
|------|---------------|
| `admin-dashboard-voorbeeld.png` | The traffic-light payments dashboard rendered with example data (Testboer Energie, Helder Water, Demo Telecom — all fictional providers). |
| `workflow.png` | The n8n workflow: Telegram Trigger → If (text or binary?) → Create file from text / Upload file → Send a text message back. |
| `telegram-intake-photo.jpg` | Telegram chat showing a test invoice (Demo Telecom B.V., labeled TESTFACTUUR) being sent to the bot, and the bot's automatic confirmation. |
| `telegram-intake-water.jpg` | Same pipeline — Helder Water N.V. test invoice received and confirmed. |
| `telegram-intake-energy.jpg` | Same pipeline — Testboer Energie B.V. test invoice received and confirmed. |

The three Telegram screenshots together demonstrate the traffic-light concept using test invoices:
- 🔴 Energy — deadline expired (red)
- 🟠 Water — deadline within a week (orange)
- 🟢 Internet — deadline more than a week away (green)
