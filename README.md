# Voice AI & CRM Post-Call Automation Pipeline (Vapi.ai + n8n + GoHighLevel)

![n8n](https://img.shields.io/badge/n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![Vapi](https://img.shields.io/badge/Vapi.ai-000000?style=for-the-badge&logo=vapi&logoColor=white)
![GoHighLevel](https://img.shields.io/badge/GoHighLevel-3B82F6?style=for-the-badge&logo=gohighlevel&logoColor=white)

An end-to-end event-driven integration pipeline that processes post-call webhooks from **Vapi.ai**, applies deduplication and security filtering in **n8n**, and dynamically updates lead opportunity stages and DNC status inside **GoHighLevel (GHL)**.

---

## 📌 Architectural Overview

1. **Voice AI Layer (Vapi.ai):** Initiates outbound AI call flows, captures call metrics, sentiment, and structured JSON transcripts.
2. **Middleware Layer (n8n):** Receives the `end-of-call-report` webhook payload, validates authentication, deduplicates incoming events, and routes data based on caller intent.
3. **CRM Endpoint (GoHighLevel):** Executes API requests to move opportunity pipeline cards (`Interested` vs. `Opted Out / DNC`) and update contact activity histories.

---

## 🛠️ Key Features & Guardrails

* **Idempotency & Event Deduplication:** Includes a dedicated **Dedupe by Call ID** node to prevent multi-pingback webhook execution loops from duplicating CRM updates.
* **Header-Based Authentication:** Configured for custom secret header authentication (`x-vapi-secret`) between Vapi.ai and n8n to restrict unauthorized endpoint triggers.
* **Intent Classification:** Parses call transcripts and structured sentiment outputs to accurately separate explicit opt-outs (`not interested`, `stop`, `remove me`) from qualified leads.
* **Dynamic Data Mapping:** Binds contact details dynamically from incoming Vapi objects (`message.call.customer`) to preserve data integrity across multi-lead campaigns.

---

## 📂 Repository Structure

```text
├── workflows/
│   └── vapi-n8n-ghl-pipeline.json    # Full exportable n8n workflow JSON
├── docs/
│   ├── n8n-workflow-canvas.png        # Workflow architecture diagram
│   └── vapi-advanced-settings.png     # Webhook server configuration guide
└── README.md                         # Technical documentation
