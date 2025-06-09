# 🛡️ n8n Phishing URL Checker (VirusTotal API)

This project uses [n8n](https://n8n.io) to detect whether a submitted URL is **malicious, suspicious, or harmless** using the [VirusTotal API](https://www.virustotal.com/).

## 🔍 Features

- Accepts URLs via a public **webhook**
- Sends URLs to VirusTotal for analysis
- Extracts scan results (harmless, suspicious, malicious)
- Sends an email with the final report

## 🧱 Tools Used

- [n8n](https://n8n.io) (Self-hosted)
- VirusTotal API
- Gmail SMTP
- curl (for testing the webhook)

---

## 🧪 How It Works

### 1. Webhook

Receives input like:

```json
{
  "suspicious_url": "http://example.com/phish"
}


2. Function Node
Encodes the URL using Buffer.from(url).toString("base64").

3. HTTP Request
Sends the encoded URL to:

bash
Copy
Edit
https://www.virustotal.com/api/v3/urls/{{encoded_url}}
With headers:

http
Copy
Edit
x-apikey: YOUR_API_KEY
4. Set Node
Extracts this data:

data.attributes.last_analysis_stats.malicious

suspicious, harmless, undetected

5. Email Node
Sends report to your email.

🔁 Sample Output
makefile
Copy
Edit
Malicious: 0
Suspicious: 0
Harmless: 85
Undetected: 8
📷 Workflow

▶️ Running the Workflow
Clone this repo

Import workflow.json into n8n

Replace the email and API credentials

Start the workflow in test mode

Trigger using curl:

bash
Copy
Edit
curl -X POST http://localhost:5678/webhook-test/phishing-check \
  -H "Content-Type: application/json" \
  -d '{"suspicious_url": "http://example.com/phish"}'
🛠️ Setup Notes
You need a Gmail app password (not your normal password)

VirusTotal free tier has request limits

📁 Files
workflow.json – Exported n8n logic

sample_payload.json – Sample webhook input

images/flow-diagram.png – Screenshot of workflow