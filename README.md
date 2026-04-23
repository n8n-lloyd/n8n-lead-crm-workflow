# 🤖 AI Lead Classifier + CRM Sync Workflow

An end-to-end AI automation pipeline built with **n8n** that captures leads via webhook, classifies them using **Google Gemini LLM**, and syncs structured data to **Notion CRM** automatically.

---

## ⚡ What It Does

1. **Receives lead data** via HTTP webhook (name, email, company, message)
2. **AI-powered classification** using Google Gemini to score leads (hot/warm/cold), detect industry, and generate follow-up recommendations
3. **Structured data parsing** via custom JavaScript node with fallback handling
4. **Auto-sync to Notion CRM** — creates a new database row with all lead details

---

## 🏗️ Architecture
┌─────────────┐     ┌─────────────────┐     ┌──────────────────┐
│   Webhook   │────▶│  Basic LLM Chain │────▶│ Code: JavaScript │
│  (Trigger)  │     │ (Google Gemini)  │     │  (Data Parser)   │
└─────────────┘     └─────────────────┘     └──────────────────┘
│
┌────────────────────┘
▼
┌──────────────────┐
│  Notion CRM      │
│ (Create DB Page) │
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
- Notion integration with database access
- Google Gemini API key

### Setup
1. Import `AI Lead Classifier + CRM Sync Workflow.json` into n8n
2. Configure credentials:
   - Notion account connection
   - Google Gemini Chat Model API key
3. Update the Notion Database ID in the "Create a database page" node
4. Activate the workflow

### Test the Webhook
```bash
curl -X POST "https://your-n8n-instance.com/webhook/your-path" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "email": "jane@company.com",
    "company": "Acme Inc",
    "message": "I need help automating my sales pipeline"
  }'
| Field         | Type                   | Source       |
| ------------- | ---------------------- | ------------ |
| **Name**      | Title                  | Webhook      |
| **Email**     | Email                  | Webhook      |
| **Company**   | Text                   | Webhook      |
| **Score**     | Select (hot/warm/cold) | LLM Analysis |
| **Industry**  | Text                   | LLM Analysis |
| **Intent**    | Text                   | Raw Message  |
| **Follow Up** | Text                   | LLM Analysis |
