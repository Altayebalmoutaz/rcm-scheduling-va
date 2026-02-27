Altayeb, [2/27/2026 3:40 AM]
# 🏥 RCM Patient Journey Automation — Scheduling VA

> An AI-powered virtual assistant automating the full patient journey from front desk intake to provider notification — built in n8n as a proof of concept for AI-first Revenue Cycle Management.

![Status](https://img.shields.io/badge/Status-Demo%20%2F%20Proof%20of%20Concept-orange)
![n8n](https://img.shields.io/badge/Built%20with-n8n-EA4B71?logo=n8n&logoColor=white)
![AI](https://img.shields.io/badge/AI-OpenRouter%20LLM-blueviolet)
![RCM](https://img.shields.io/badge/Domain-Revenue%20Cycle%20Management-blue)
![HIPAA](https://img.shields.io/badge/HIPAA-Aware%20Design-green)

-----

## 🧠 What This Does

Most clinic front desks run on phone calls, faxes, and manual data entry. This workflow replaces that with an intelligent automation pipeline that:

1. Receives patient intake via webhook
1. Validates incoming data and checks cache for existing records
1. Calls a Mock Payer API to simulate insurance eligibility verification
1. Uses an AI model (OpenRouter LLM) to evaluate patient urgency and triage priority
1. Checks Google Calendar for provider availability
1. Creates the appointment automatically based on urgency + availability
1. Stores the patient record for downstream RCM processing
1. Notifies admin staff and providers via Slack in real time

-----

## 🗺️ Workflow Architecture
Webhook (Patient Intake)
    │
    ├──► [TOP BRANCH: Insurance Eligibility]
    │         Validate Input → Check Cache → Mock Payer API
    │         → Save Verification → Notify via Slack
    │
    └──► [MAIN BRANCH: AI Scheduling]
              Edit Fields → AI Urgency Evaluation (OpenRouter LLM)
                  │
                  ├── Google Calendar Tool (availability check)
                  ├── Structured Output Parser
                  └── Create Emergency Appointment
                            │
                            └── Prepare Record → Store Patient Record
                                    → Notify Admin Staff (Slack)

-----

## 🛠️ Tech Stack

|Component      |Tool                    |Purpose                            |
|---------------|------------------------|-----------------------------------|
|Workflow Engine|n8n (self-hosted)       |Orchestration                      |
|AI / LLM       |OpenRouter Chat Model   |Urgency evaluation & triage        |
|Scheduling     |Google Calendar API     |Provider availability              |
|Notifications  |Slack                   |Admin & provider alerts            |
|Insurance Sim  |Mock Payer API          |Eligibility verification sandbox   |
|Data Handling  |Structured Output Parser|Clean LLM output for downstream use|
|Trigger        |Webhook                 |Patient intake entry point         |

-----

## 🤖 AI Urgency Evaluation — The Key Node

The core intelligence of this workflow is the AI Urgency & Triage Evaluator:

- Receives structured patient data (symptoms, visit reason, demographics)
- Uses an LLM via OpenRouter to classify urgency (Emergency / Urgent / Routine)
- Routes the patient to the appropriate scheduling path
- Outputs structured JSON consumed by downstream nodes

This simulates what a clinical triage nurse does — but automated, consistent, and instant.

-----

## 📋 Workflow Nodes Breakdown

### Insurance Eligibility Branch

- Validate Input — Sanitizes and validates incoming patient data
- Check Cache — Prevents duplicate eligibility checks for returning patients
- Mock Payer API — Simulates real-time payer eligibility verification (Availity/Change Healthcare pattern)
- Save Verification — Persists eligibility result for claims processing
- Send Message (Slack) — Alerts billing team of eligibility status

### AI Scheduling Branch

- Edit Fields — Structures patient data for LLM consumption
- Evaluate Urgency (OpenRouter + Chat Memory) — AI triage classification
- Google Calendar Tool — Real-time provider availability lookup
- Structured Output Parser — Converts LLM response to structured data
- Create Emergency Appointment — Books appointment in calendar

Altayeb, [2/27/2026 3:40 AM]
- Prepare Record Data — Formats data for EHR/RCM storage
- Store Patient Record — Persists encounter data
- Notify Admin Staff (Slack) — Real-time provider and staff notification

-----

## 🎯 RCM Impact (Simulated)

|Manual Process                 |This Workflow                    |
|-------------------------------|---------------------------------|
|Front desk phone intake        |Webhook — instant, 24/7          |
|Manual insurance verification  |Mock Payer API — seconds         |
|Nurse triage call              |AI urgency evaluation — real-time|
|Manual calendar check          |Google Calendar API — automated  |
|Phone/fax provider notification|Slack alert — instant            |

-----

## 🚧 Current Limitations (Sandbox)

- No live payer integration — uses mock API (real Availity/Change Healthcare integration in development)
- No live EHR connection — patient records stored locally (Epic/Athenahealth integration planned)
- Self-hosted n8n instance — not yet deployed to production environment
- Designed as a proof of concept to demonstrate RCM automation architecture

-----

## 🔭 Roadmap

- [ ] Connect to live Availity API for real eligibility verification
- [ ] Epic EHR integration via FHIR API
- [ ] Prior authorization submission node
- [ ] Denial management feedback loop
- [ ] HIPAA-compliant cloud deployment
- [ ] Dashboard for RCM analytics (Power BI)

-----

## 👨‍⚕️ Built By

Dr. Altayeb Almoutaz — MD turned AI Automation Engineer

This project is built from 6 years of clinical experience watching RCM fail in real time. Every node in this workflow maps to a real bottleneck I observed in hospital billing operations.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?logo=linkedin)](https://linkedin.com/in/altayeb-almoutaz-1161b9237)
[![Email](https://img.shields.io/badge/Email-Contact-D14836?logo=gmail)](mailto:altayebalmoutaz72@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-Altayebalmoutaz-181717?logo=github)](https://github.com/Altayebalmoutaz)

-----

> 💡 *This is a sandbox demonstration. All patient data used in testing is synthetic and HIPAA-compliant by design.*
