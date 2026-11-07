# Day 4 — Lead Workflow Hardening Test Results

## 1. Input Validation
- Invalid or missing required input tested.
- Expected HTTP Status: 400 Bad Request.
- Result: Passed.

## 2. Header Authentication
- Correct API key tested.
- Expected HTTP Status: 200 OK.
- Result: Passed.

- Incorrect API key tested.
- Expected HTTP Status: 403 Forbidden.
- Result: Passed.

## 3. AI Failure Fallback
- Gemini API key temporarily made invalid.
- Unscored lead successfully saved to Google Sheets.
- Qualification: unscored.
- Lead Score: blank.
- Result: Passed.

## 4. Invalid AI JSON
- Non-JSON AI response tested.
- Parser error triggered.
- Lead routed to Unscored Lead fallback.
- Unscored lead saved to Google Sheets.
- Result: Passed.

## 5. Valid AI Processing
- Original Gemini API key restored.
- Normal lead processing tested.
- AI result successfully saved to Google Sheets.
- Result: Passed.