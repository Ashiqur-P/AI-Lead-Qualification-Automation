# AI Lead Qualification & Automation — System Architecture

This document describes the architecture of the AI Lead Qualification & Automation workflow built with **n8n**.

The workflow receives lead information through a webhook, validates the input, uses Google Gemini to analyze and prioritize the lead, stores the result in Google Sheets, and sends Gmail notifications for high-priority leads.

## Architecture Overview

```text
Lead
  ↓
Webhook
  ↓
Validation
  ↓
Google Gemini AI
(models/gemini-3-flash-preview)
  ↓
JSON Parsing
  ↓
Generate Lead ID
  ↓
Google Sheets
  ↓
Priority Check
  ├── High → Gmail Notification
  └── Low → End
1. Lead Ingestion

The workflow receives lead information through an n8n Webhook.

Typical input fields include:

Name
Email
Company
Website
Business Type
Budget
Timeline
Message

The webhook can receive leads from forms, websites, CRM systems, or other external applications.

2. Input Validation

The incoming lead data is validated before AI processing.

Validation helps ensure that:

Required fields are available.
The incoming data has the expected structure.
Invalid or incomplete data is identified before further processing.
3. AI Lead Qualification

Provider: Google Gemini
Model: models/gemini-3-flash-preview

Gemini analyzes the lead information and assigns a priority based on the configured business rules.

Current Priority Rules
Budget over $20,000 → high
Urgent or critical requirement → high
Otherwise → low

The AI is instructed to return only a valid JSON object.

AI Output
{
  "name": "Ahsan Khan",
  "email": "ahsan.khan@techinnovate.com",
  "company": "TechInnovate Solutions",
  "message": "CRITICAL RISK: Our main e-commerce platform's customer support system is completely down.",
  "budget": "$50,000 - $100,000",
  "timeline": "Urgent - Within 7 days",
  "priority": "high"
}
4. JSON Processing

The Gemini response is parsed as JSON so that the individual fields can be used by downstream n8n nodes.

The processed fields include:

Name
Email
Company
Message
Budget
Timeline
Priority
5. Lead ID Generation

After JSON processing, the workflow generates a Lead ID.

The ID provides a unique reference for tracking the lead in the automation and Google Sheets.

6. Data Storage

The qualified lead information is stored in Google Sheets.

The current sheet contains:

Lead ID
Date
Name
Email
Company
Website
Business Type
Message
Budget
Timeline
Priority
7. Priority Decision

The workflow checks the AI-generated priority value using an IF node.

Priority	Workflow Action
high	Store in Google Sheets + Gmail notification
low	Store in Google Sheets + End workflow

This allows high-value or urgent leads to receive immediate attention.

8. Gmail Notification

When a lead is classified as high, the workflow sends a Gmail notification.

This helps sales teams or business owners respond quickly to important opportunities.

9. Security

Credentials are managed through n8n's credential system.

API keys, OAuth tokens, passwords, and other sensitive credentials should never be hard-coded or committed to the repository.

For production use, the webhook should be protected with HTTPS, authentication, input validation, and appropriate rate limiting.

Architecture Summary

The workflow follows a modular automation pipeline:

Ingestion
   ↓
Validation
   ↓
AI Qualification
   ↓
JSON Processing
   ↓
Lead ID Generation
   ↓
Storage
   ↓
Priority Decision
   ↓
Notification

This architecture demonstrates how AI and workflow automation can be combined to automatically qualify leads, identify urgent opportunities, and improve response time.