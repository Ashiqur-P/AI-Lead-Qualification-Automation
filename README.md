AI Lead Qualification & Automation

An AI-powered lead qualification and automation workflow built with n8n and Google Gemini. The system receives lead information through a webhook, validates the input, uses Gemini AI to determine lead priority and score, stores the results in Google Sheets, and supports priority-based routing.

Workflow Layout

Lead ➔ Webhook ➔ Validation ➔ Google Gemini AI ➔ JSON Parsing ➔ Generate Lead ID ➔ Google Sheets ➔ Priority Check
                                                                                                  ├── High ➔ Gmail Notification (Optional)
                                                                                                  ├── Medium ➔ End Workflow
                                                                                                  └── Low ➔ End Workflow

Key Features

Webhook-based Lead Intake — Captures incoming leads automatically via webhooks.

Input Validation — Checks data integrity before passing it to the AI.

AI-powered Lead Qualification — Integrates Google Gemini (models/gemini-3-flash-preview) for lead screening and scoring.

Structured JSON Processing — Ensures data is parsed into structured JSON format.

Automatic Lead ID Generation — Generates unique identifiers for incoming leads.

Google Sheets Storage — Stores lead information and AI qualification results.

Lead Scoring & Classification — Assigns numerical scores and classifies leads as Hot, Warm, or Cold.

Priority-based Routing — Classifies leads into High, Medium, or Low priority.

Optional Gmail Notification — Requires Gmail OAuth configuration; currently pending.

AI Qualification Criteria

Gemini evaluates incoming leads based on budget, urgency, and business requirements:

Condition

Assigned Priority

Action Taken

High-value or urgent requirement

High

Stores in Google Sheets + Optional Gmail Notification

Potential business opportunity

Medium

Stores in Google Sheets

Low-potential opportunity

Low

Stores in Google Sheets + Ends Workflow

Note: Priority classification is determined by the AI qualification prompt.

AI outputs can vary. Scores and priorities are suggestions for the sales team, not final decisions.

Expected Output Fields:

The AI returns a structured JSON object containing 12 fields:

name, email, company, message, budget, timeline, priority, lead_score, qualification, business_need, recommended_action, and ai_reason.

Example AI JSON Result:

{
  "name": "Ahsan Khan",
  "email": "test@example.com",
  "company": "TechInnovate Solutions",
  "message": "We urgently need an AI customer support chatbot.",
  "budget": "$50,000 - $100,000",
  "timeline": "Within 7 days",
  "priority": "high",
  "lead_score": 85,
  "qualification": "hot",
  "business_need": "AI customer support chatbot",
  "recommended_action": "Contact the lead immediately",
  "ai_reason": "High budget and urgent business requirement."
}

Example data is for demonstration purposes only.

Testing & Verification

Four lead qualification test cases have passed:

Test Case

Result

High Lead

Hot / High — PASS

Warm Lead

Warm / Medium — PASS

Cold Lead

Cold / Low — PASS

Bengali Lead

Hot / High — PASS

Bengali Lead Verification:

Lead Score: 85

Qualification: Hot

Priority: High

Bengali message successfully stored in Google Sheets.

Detailed test results: test-results/day3-lead-tests.md

Gmail Notification Status

Gmail notification is currently deactivated because OAuth credentials require configuration. It remains an optional integration and has not yet been verified.

Security Notes

API keys, OAuth credential IDs, Sheet ID, and instance ID in the exported workflow are replaced with placeholders.

The webhook currently has no authentication. Header Auth is planned for the next iteration.

Use dummy contact information in public test data.

Technology Stack

n8n — Core workflow automation.

Google Gemini — AI qualification engine (models/gemini-3-flash-preview, Preview model).

Google Sheets — Lead data storage.

Gmail — Optional high-priority notifications (OAuth setup pending).

Docker — Local n8n server environment.

PowerShell — Webhook/API testing.

Project Structure

AI-Lead-Qualification-Automation/
│
├── README.md
├── workflow/
│   └── lead-qualification-workflow.json
├── prompts/
│   └── lead-analysis-prompt.txt
├── test-data/
│   ├── high-priority-lead.json
│   └── low-priority-lead.json
├── test-results/
│   └── day3-lead-tests.md
├── docs/
│   ├── architecture.md
│   └── setup.md
└── screenshots/
    ├── 01-full-workflow.png
    ├── 02-gemini-ai-qualification.png
    └── 03-google-sheets-results.png

## Screenshots

### 📊 Full Workflow Layout
![Full Workflow](screenshots/01-full-workflow.png)

### 🧠 Gemini AI Qualification Setup
![Gemini AI Qualification](screenshots/02-gemini-ai-qualification.png)

### 📈 Google Sheets Results
![Google Sheets Results](screenshots/03-google-sheets-results.png)


Documentation Reference

docs/architecture.md — Detailed system architecture and structural design.

docs/setup.md — Installation, API credentials configuration, and webhook testing.

prompts/lead-analysis-prompt.txt — Core Gemini AI system instruction prompt template.

test-results/day3-lead-tests.md — Lead qualification test results.

Business Value

Identify High-Priority Leads Quickly — Helps identify promising business opportunities.

Reduce Manual Overhead — Automates lead screening and data transfer tasks.

Centralize Information — Keeps lead information organized in Google Sheets.

Future Extensions

The automation architecture supports potential extensions such as:

CRM platform integrations (Salesforce, HubSpot).

Database logging (PostgreSQL, MongoDB).

Multi-channel messaging (WhatsApp, Telegram, Slack).

AI-driven personalized automated follow-up emails.

License

This project is intended for portfolio demonstration and educational purposes.