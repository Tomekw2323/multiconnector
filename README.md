# 🔗 Multi-App Connector

> Connect the tools your business already uses. When something happens in App A, something automatically happens in App B — no Zapier subscription, no usage limits, full control.

**[🔴 Live Demo](https://twojname.github.io/connector-demo)** &nbsp;|&nbsp; Built by [Tomasz Witkowski](mailto:tomekw2323@gmail.com)

---

## What it does

Most businesses run on 5–10 different tools that don't talk to each other. Every handoff between systems is a manual step someone has to remember. This connector automates those handoffs.

**Example flows included:**

- **Form → Sheet → Email** — Typeform submission → row in Google Sheets → personalised welcome email via Gmail
- **CRM Deal Won → Slack + Asana** — deal closed in Pipedrive → notify #sales channel → create onboarding tasks
- **Shopify Order → Drive + Slack** — new order → save invoice PDF to Google Drive → ping #orders
- **Calendly Booking → Notion + Email** — meeting booked → create Notion notes page → send confirmation

---

## Demo

The `index.html` demo shows an animated flow diagram — pick a flow, click Trigger, and watch each node activate in sequence with a live event log.

### Deploy to GitHub Pages
1. Fork this repo
2. **Settings → Pages → Deploy from branch → main / root**
3. Live at `https://yourusername.github.io/connector-demo`

---

## Production Setup

```bash
git clone https://github.com/yourusername/connector-demo
cd connector-demo
pip install -r requirements.txt
cp .env.example .env
# Fill in API keys for the apps you want to connect
python connector.py --flow form_to_sheet_email
```

### Defining a custom flow

Flows are defined as simple Python config — no special syntax needed:

```python
FLOWS = {
    "new_client": [
        {
            "trigger": "typeform.new_submission",
            "filter": lambda r: r["service"] == "automation"
        },
        {
            "action": "sheets.append_row",
            "sheet": "Leads 2026",
            "mapping": {"Name": "{{name}}", "Email": "{{email}}", "Date": "{{today}}"}
        },
        {
            "action": "gmail.send",
            "to": "{{email}}",
            "template": "templates/welcome.html"
        },
        {
            "action": "slack.post",
            "channel": "#new-leads",
            "message": "New lead: {{name}} — {{email}}"
        }
    ]
}
```

### Running as a webhook server

```bash
# Start the webhook listener (receives events from connected apps)
python server.py --port 8000

# Or run as a scheduled poller (for apps without webhooks)
python poller.py --interval 300
```

---

## Supported integrations

| App | Trigger support | Action support |
|---|---|---|
| Typeform | ✓ new submission | — |
| Google Sheets | ✓ new row | ✓ append row, update cell |
| Gmail | — | ✓ send email, add label |
| Slack | ✓ new message | ✓ post message, DM |
| Notion | — | ✓ create page, update property |
| Asana | ✓ task completed | ✓ create task, assign |
| HubSpot | ✓ deal stage change | ✓ create contact, update deal |
| Pipedrive | ✓ deal won/lost | ✓ create person, add note |
| Google Drive | — | ✓ create folder, upload file |
| Shopify | ✓ new order, payment | ✓ update inventory |
| Airtable | ✓ new record | ✓ create record, update field |
| Calendly | ✓ event scheduled | — |

---

## Stack

| Component | Library |
|---|---|
| Core runtime | Python 3.11 |
| Webhook server | FastAPI |
| HTTP client | httpx |
| Scheduling | APScheduler |
| Templates | Jinja2 |
| Config | python-dotenv |
| Logging | loguru |

---

## Why not Zapier / Make?

| | Zapier/Make | This connector |
|---|---|---|
| Monthly cost | $20–$100+ | Free (self-hosted) |
| Usage limits | Yes (task limits) | None |
| Custom logic | Limited | Full Python |
| Data privacy | Cloud | Your server |
| Vendor lock-in | Yes | No |
| Debugging | Black box | Full logs |

---

## Contact

**Tomasz Witkowski** — Automation & AI specialist  
📧 tomekw2323@gmail.com  
📱 +48 784 237 870  
🔗 [LinkedIn](https://linkedin.com/in/twojprofil)
