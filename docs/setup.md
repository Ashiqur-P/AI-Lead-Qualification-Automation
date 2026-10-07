# Setup & Installation Guide

This guide explains how to set up and test the AI Lead Qualification & Automation workflow in a local n8n environment.

## Prerequisites
The following tools and services are required:
* **Docker Desktop** — To run n8n locally.
* **Google Account** — For Google Sheets and Gmail access.
* **Google AI Studio** — To generate the Gemini API key.
* **Postman** — Or any alternative REST client for webhook testing.

---

## 1. Start n8n
The project uses n8n running through a local Docker container environment.

### Run the Docker Container:
```bash
docker run -d \
  --name n8n \
  -p 5678:5678 \
  -e TZ=Asia/Dhaka \
  -v n8n_data:/home/node/.n8n \
  --restart unless-stopped \
  docker.n8n.io/n8nio/n8n
```

### Verify Container Status:
```bash
docker ps
```

### Open n8n Dashboard:
Once the container is running, access the local n8n interface at: [http://localhost:5678](http://localhost:5678)

---

## 2. Configure Google Gemini
1. Open **Google AI Studio**.
2. Create and copy a new **Gemini API key**.
3. Add the credential securely inside the n8n credential manager.
4. Select Google Gemini in the AI node and configure it to use the following model:
   `models/gemini-3-flash-preview`

> ⚠️ **Important:** Keep the API key private and do not commit it to the repository.

---

## 3. Configure Google Sheets
1. Create a new Google Spreadsheet named: **`Leads Tracking`**
2. Create a worksheet (tab) inside named: **`Qualified Leads`**
3. Setup the top row with the exact following columns:

| Lead ID | Date | Name | Email | Company | Website | Business Type | Message | Budget | Timeline | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |

4. Connect your Google Sheets authentication credential inside the n8n node settings.

---

## 4. Configure Gmail
1. Connect a valid Gmail/Google OAuth credential inside your n8n settings.
2. The Gmail node is pre-configured to trigger automatic email alerts whenever a lead is classified as: **`high`**

---

## 5. Import the Workflow
1. Open your n8n instance: `http://localhost:5678`
2. Click on the workflow menu and select **Import from File**.
3. Select the workflow JSON file from your project path: `workflow/lead-qualification-workflow.json`
4. Verify the structural pipeline layout:
   ```text
   Webhook ➔ Validation ➔ Gemini AI ➔ JSON Parsing ➔ Generate Lead ID ➔ Google Sheets ➔ Priority Check
                                                                                          ├── High ➔ Gmail Notification
                                                                                          └── Low ➔ End Workflow
   ```
5. Connect your saved credentials to the imported nodes.

---

## 6. Test the Workflow
1. Open the n8n **Webhook** node.
2. Select **Listen for test event**.
3. Copy the production/test Webhook URL.
4. Send a mock `POST` request payload using **Postman**.

### Example Postman Test Payload:
```json
{
  "name": "Ahsan Khan",
  "email": "ahsan.khan@techinnovate.com",
  "company": "TechInnovate Solutions",
  "website": "https://techinnovate.com",
  "business_type": "Software Agency",
  "budget": "$50,000 - $100,000",
  "timeline": "Urgent - Within 7 days",
  "message": "CRITICAL RISK: Our main e-commerce platform's customer support system is completely down and we are losing thousands of dollars every hour. We need an enterprise-grade AI chatbot integrated immediately."
}
```

---

## 7. Verify the Result
For the Postman test payload provided above, the expected output priority is: **`high`**.

### Verification Checklist:
* [ ] Webhook successfully receives the incoming payload.
* [ ] Input data validation succeeds.
* [ ] Gemini AI processes data and parses structured fields.
* [ ] A unique unique Lead ID is appended.
* [ ] Row is created in your **Leads Tracking** Google Sheet.
* [ ] Priority logic filters the high priority lead status.
* [ ] Automated confirmation email is routed to your Gmail inbox.

---

## 8. Troubleshooting
* **Gemini Error:** Double-check your Gemini API key credentials and confirm server runtime availability. Enable n8n automatic node retries for intermittent connectivity errors.
* **Google Sheets Error:** Confirm folder/account permissions and ensure sheet column structures match the required list exactly.
* **Gmail Error:** Verify Google secure authentication scopes and target receipt addresses.
* **Webhook Error:** Confirm the Docker instance container state and verify if you are using the active test endpoint link.
* **JSON Parsing Error:** Check raw Gemini responses to ensure no extra markdown formatting text blocks are included outside the valid JSON boundaries.

---

## 9. Security & Compliance Best Practices
* Never commit active production secret environmental `.env` files, actual credentials, or sensitive enterprise lead rows to public codebases.
* Use n8n credential managers to handle API integrations safely.
* Production webhooks must utilize proper `HTTPS` parameters alongside appropriate data schema validation layers and active rate-limiting features.

---

### Setup Complete!
Your local automation system is completely prepared for active portfolio deployment and software demonstrations. Feel free to scale this core structure into advanced enterprise-level pipelines using robust database infrastructures, multiple custom AI workflows, and dedicated CRM routing features!