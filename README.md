# 📦 Shipment Reminder Automation (n8n)

An **n8n workflow** that reads shipment data from a **Google Sheet**, validates it against shipment rules, and automatically sends **email reminders** — at most once per day per recipient.

![n8n](https://img.shields.io/badge/Built%20with-n8n-ea4b71?logo=n8n&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Data-Google%20Sheets-34a853?logo=googlesheets&logoColor=white)
![Status](https://img.shields.io/badge/Status-Working-brightgreen)

---

## 📖 Overview

Manually following up on shipments is repetitive and easy to forget. This workflow automates it:

1. Pulls shipment rows from a Google Sheet.
2. Checks which shipments need a reminder based on defined rules.
3. Builds the reminder email.
4. Makes sure the same reminder isn't sent twice in one day.
5. Sends the email and records that it was sent.

It can be run **manually** (click **Execute workflow**) or **automatically once a day** via the schedule trigger.

---

## 🔄 Workflow

```
Schedule Shipment Check
        │
        ▼
Get row(s) in sheet  (Google Sheets)
        │
        ▼
Validate Shipment Rules  (Code)
        │
        ▼
Generate reminder mail  (Code)
        │
        ▼
Check Daily mail status  (Code)
        │
        ▼
Send Shipment Reminder  (Send Email)
        │
        ▼
Update daily email status  (Code)
```

### Node breakdown

| # | Node | Type | Purpose |
|---|------|------|---------|
| 1 | **Schedule Shipment Check** | Schedule Trigger | Starts the workflow once a day (or run manually with *Execute workflow*). |
| 2 | **Get row(s) in sheet** | Google Sheets | Reads shipment data and recipient email addresses from the sheet. |
| 3 | **Validate Shipment Rules** | Code | Filters rows and applies the shipment rules to decide which need a reminder. |
| 4 | **Generate reminder mail** | Code | Builds the email subject and body for each shipment. |
| 5 | **Check Daily mail status** | Code | Skips anyone who has already been emailed today (prevents duplicates). |
| 6 | **Send Shipment Reminder** | Send Email | Sends the reminder to the email address(es) in the sheet. |
| 7 | **Update daily email status** | Code | Marks the email as sent so it isn't repeated the same day. |

---

## 🛠️ Tech Stack

- [n8n](https://n8n.io/) — workflow automation
- Google Sheets — data source
- JavaScript (n8n Code nodes) — validation and email logic
- Email (SMTP / Gmail) — delivery

---

## ⚙️ Setup

### 1. Prerequisites
- An n8n instance (n8n Cloud or self-hosted)
- A Google account with access to the spreadsheet
- An email account for sending (SMTP or Gmail)

### 2. Import the workflow
1. Open n8n → **Workflows** → **Import from file**.
2. Select `workflow.json` from this repository.

### 3. Prepare the Google Sheet
Create a sheet with the columns your workflow expects, for example:

| Shipment ID | Customer | Email | Status | Due Date | Last Email Sent |
|-------------|----------|-------|--------|----------|-----------------|
| SHP-001 | ... | name@example.com | Pending | 2026-10-12 | |

> Adjust the column names to match the ones used in your Code nodes.

### 4. Configure credentials
- **Google Sheets node** → connect your Google account (OAuth2) and choose the spreadsheet and sheet.
- **Send Email node** → add your SMTP/Gmail credentials.

### 5. Test and activate
1. Click **Execute workflow** to run it once manually.
2. Confirm the emails arrive.
3. **Publish** the workflow so the daily schedule runs automatically.

---

## ▶️ Usage

- **Manual run:** open the workflow and click **Execute workflow**.
- **Automatic run:** once published, the Schedule Trigger runs it once every day at the configured time.

---

## 📁 Repository Structure

```
.
├── README.md
├── workflow.json        # Exported n8n workflow
└── screenshots/
    └── workflow.png     # Screenshot of the workflow canvas
```

---

## 🔐 Security Notes

- Never commit credentials, API keys, or real customer email addresses.
- Remove or anonymize credential IDs and sheet IDs from `workflow.json` before pushing.

---

## 🚀 Future Improvements

- Add Slack / WhatsApp notifications alongside email
- Support multiple reminder levels (e.g., 3 days before, on due date, overdue)
- Error-handling workflow that alerts on failures
- Dashboard of sent reminders

---

## 👤 Author

**Avinash**
Third-year B.E. Computer Science Engineering student, Rajalakshmi Engineering College

---

⭐ If you found this useful, consider giving the repo a star!
