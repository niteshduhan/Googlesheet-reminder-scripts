# 🎂 HR Birthday & Anniversary Reminder Bot

> Automated Google Sheets → Gmail alert system for employee birthdays and work anniversaries — zero infrastructure, zero cost.

![Google Apps Script](https://img.shields.io/badge/Google_Apps_Script-4285F4?style=flat-square&logo=google&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google_Sheets-34A853?style=flat-square&logo=google-sheets&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-EA4335?style=flat-square&logo=gmail&logoColor=white)
![Automation](https://img.shields.io/badge/Trigger-Daily_Cron-orange?style=flat-square)
![Status](https://img.shields.io/badge/Status-Production_Ready-brightgreen?style=flat-square)

---

## Overview

Reads employee records from a Google Sheet, detects birthdays and work anniversaries matching today's date, and fires a consolidated email digest to HR — all triggered automatically via Google's built-in cron scheduler.

**No server. No API keys. No third-party dependencies.**

| What | Detail |
|------|--------|
| **Input** | Google Sheet (`Sheet1`) with employee data |
| **Output** | Single consolidated email to HR |
| **Trigger** | Daily time-based trigger (Google Apps Script) |
| **Filter** | Active employees only (Column J = `"active"`) |
| **Anniversary logic** | Only fires for ≥ 1 year completed |

---

## Sheet Schema

Column mapping expected in `Sheet1`:

| Column | Field | Notes |
|--------|-------|-------|
| A | Name | Employee full name |
| E | DOB | Date of Birth |
| F | DOJ | Date of Joining |
| J | Status | Must be `"active"` to be included |

---

## Methodology

```
Sheet Read → Filter Active Employees → Date Match (DOB / DOJ)
    → Build Email Payload → Send via MailApp → Done
```

1. **Read** — Pull all rows from `Sheet1` via `getDataRange()`
2. **Filter** — Skip inactive employees (Column J ≠ `"active"`)
3. **Birthday check** — Match `day` + `month` against today; year-agnostic
4. **Anniversary check** — Match `day` + `month`, compute `currentYear - joinYear`, skip if < 1
5. **Consolidate** — Build a single email body for both event types
6. **Send** — One `MailApp.sendEmail()` call; no email sent if no events today

---

## Project Structure

```
hr-reminder-bot/
│
├── Code.gs                  # Main script — all logic lives here
├── README.md                # This file
└── sample_sheet_schema.md   # Column reference for Sheet1 setup
```

---

## How to Run

### 1. Deploy the Script

```
Google Sheet → Extensions → Apps Script → Paste Code.gs → Save
```

### 2. Set the Recipient Email

```javascript
// Line 6 in Code.gs
var myEmail = "hr@yourcompany.com";
```

### 3. Set a Daily Trigger

```
Apps Script → Triggers → Add Trigger
  Function   : sendBirthdayAndAnniversaryReminders
  Event type : Time-driven → Day timer → 9:00 AM
```

### 4. Authorize Permissions

On first run, Google will prompt for:
- `spreadsheets.readonly` — read sheet data
- `gmail.send` — send email via MailApp

---

## Sample Email Output

```
Subject: 🎂 Birthday & 🏆 Work Anniversary Reminder

Hello Team,

🎂 Birthday:
• Riya Sharma

🏆 Work Anniversary:
• Amit Verma – Completed 3 year(s)
• Priya Singh – Completed 1 year(s)

Please join us in wishing them!

Best regards,
HR System
```

---

## Key Takeaways

- **Serverless automation** — Entire pipeline runs inside Google's infrastructure; no compute cost, no maintenance overhead
- **Defensive filtering** — Active-only check and ≥1 year anniversary guard prevent false positives without any extra configuration
- **Consolidated digest pattern** — Single email per day regardless of N events; avoids inbox spam and keeps HR workflow clean

---

## Author

**Nitesh Duhan** — Data Scientist & ML Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-niteshduhan--carp112-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/niteshduhan-carp112)
[![Email](https://img.shields.io/badge/Email-niteshduhan686@gmail.com-EA4335?style=flat-square&logo=gmail)](mailto:niteshduhan686@gmail.com)
[![Instagram](https://img.shields.io/badge/Instagram-@nitesh._duhan-E4405F?style=flat-square&logo=instagram)](https://www.instagram.com/nitesh._duhan)
