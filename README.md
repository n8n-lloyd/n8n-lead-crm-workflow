# 🤖 AI Lead Classifier + CRM Sync Workflow

An end-to-end AI automation pipeline built with **n8n** that captures leads via webhook, classifies them using **Groq LLM**, and syncs structured data to **Google Sheets CRM** automatically.

---

## ⚡ What It Does

1. **Receives lead data** via HTTP webhook (name, email, company, message)
2. **AI-powered classification** using Groq LLM to score leads (hot/warm/cold), detect intent, and generate follow-up recommendations
3. **Structured data parsing** via custom JavaScript node with fallback handling
4. **Auto-sync to Google Sheets CRM** — appends a new row with all lead details + timestamp

---

## 🏗️ Architecture
┌─────────────┐     ┌─────────────────┐     ┌──────────────────┐
│   Webhook   │────▶│  Basic LLM Chain │────▶│ Code: JavaScript │
│  (Trigger)  │     │   (Groq LLM)     │     │  (Data Parser)   │
└─────────────┘     └─────────────────┘     └──────────────────┘
│
┌────────────────────┘
▼
┌──────────────────┐
│  HTTP Request    │
│ (Google Sheets   │
│  via Apps Script)│
└──────────────────┘
│
▼
┌──────────────────┐
│ Respond to       │
│ Webhook          │
└──────────────────┘


---

## 🚀 Quick Start

### Prerequisites
- n8n instance (self-hosted or cloud)
- Groq API key (get free credits at console.groq.com)
- Google account for Sheets + Apps Script

### Setup

1. **Import** `AI Lead Classifier + CRM Sync Workflow.json` into n8n
2. **Configure Groq credentials** in the Groq Chat Model node
   - Model: `llama-3.1-8b-instant` or `qwen3-32b`
3. **Create Google Sheet** with headers:

Timestamp | Name | Email | Company | Score | Industry | Intent | Follow Up

4. **Add Google Apps Script** (see code in repo) and deploy as Web App
5. **Paste Web App URL** into the HTTP Request node
6. **Publish** the workflow

### Test the Webhook
```bash
curl -X POST "https://your-n8n-instance.com/webhook/your-path" \
-H "Content-Type: application/json" \
-d '{
 "name": "David Chen",
 "email": "david.chen@techcorp.com.au",
 "company": "TechCorp Australia",
 "message": "I need to book 15 jet skis for our corporate team building event on Saturday May 10th at 11am. Budget is $8,500. Can you send me a formal quote today?"
}'

| Field         | Type      | Source                       |
| ------------- | --------- | ---------------------------- |
| **Timestamp** | Date/Time | Auto-generated               |
| **Name**      | Text      | Webhook                      |
| **Email**     | Text      | Webhook                      |
| **Company**   | Text      | Webhook                      |
| **Score**     | Text      | LLM Analysis (hot/warm/cold) |
| **Industry**  | Text      | LLM Analysis                 |
| **Intent**    | Text      | LLM Analysis                 |
| **Follow Up** | Text      | LLM Analysis                 |


🛠️ Tech Stack
n8n — Workflow automation engine
Groq — Ultra-fast LLM inference (Llama 3.1 / Qwen)
Google Sheets — CRM database (via Apps Script Web App)
JavaScript — Custom data parsing & transformation

function doPost(e) {
  const data = JSON.parse(e.postData.contents);
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  
  sheet.appendRow([
    new Date(),
    data.name || '',
    data.email || '',
    data.company || '',
    data.score || '',
    data.industry || '',
    data.intent || '',
    data.follow_up || ''
  ]);
  
  return ContentService.createTextOutput(JSON.stringify({
    success: true,
    message: 'Lead saved to Google Sheets'
  })).setMimeType(ContentService.MimeType.JSON);
}
Deploy as Web App with access: Anyone

🎯 Demo Use Case: Ultimate Watersports
This workflow was built as a portfolio piece for Ultimate Watersports — a watersports rental company in Western Australia.
Scenario: A customer fills out a booking inquiry for jet skis. Within 2 seconds:
Lead is AI-classified (hot/warm/cold)
Stored in Google Sheets CRM
Sales team gets instant notification
Customer receives auto-confirmation
📝 License
MIT — Free to use and modify for your own automation projects.

Built with 💡 by Lloyd Lisen





