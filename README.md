# n8n Automation Projects: HR and Pharmacy

Two business automation projects built with **n8n**, shown on one public website.

- **HR automation:** candidates apply with a PDF CV, AI screens each application, an HR assistant answers policy questions, and a coach reviews CVs against job posts.
- **Pharmacy automation (Dawaa):** customers send a medicine name or a photo of a prescription on WhatsApp. The system reads it, checks the catalog, reserves the stock, and asks a pharmacist on Telegram when a human check is needed.

**Live site:** https://userx-101.github.io/HRAutomation/

> The workflows run on my own computer (Docker + n8n, exposed through ngrok). When my machine is off, the site shows **Offline** and the form buttons are disabled.

## Project 1: HR automation

| Workflow | Trigger | What it does |
|---|---|---|
| Recruitment 1 - Intake | Form | Candidates apply with a PDF CV. It is validated, duplicates are rejected, the CV is saved to Google Drive and the application is logged to a Google Sheet. |
| Recruitment 2 - AI Screening | Schedule (every hour) | Reads new applications, scores each CV with AI, and emails an invite, a follow-up or a decline. |
| Recruitment 3 - Error Handler | Error trigger + schedules | Logs failures, emails alerts, sends a daily digest, and checks that the queue is moving. |
| Job Application Coach | Form | Reads a CV against a job post and emails a coaching report. |
| RAG (HR policy assistant) | Chat | Answers employee questions only from the company policy documents (HR Policy Handbook and IT and Security Policy), names the source, and suggests contacting HR or IT when the answer is not in the documents. It remembers the last 10 messages of a conversation. |

## Project 2: Pharmacy automation (Dawaa)

| Workflow | Trigger | What it does |
|---|---|---|
| Dawaa 1 - Requests & Reservations | WhatsApp + Telegram | AI reads a medicine name or a prescription photo and matches it to the catalog. In-stock medicines are reserved straight away and the stock is updated. Controlled or low-stock medicines are sent to a pharmacist on Telegram, who approves or rejects with a button. The customer is told the result on WhatsApp. |
| Dawaa 2 - Error handler and maintenance | Error trigger + schedules | Classifies every error (credentials, rate limit, network, Google Sheets, messaging, code), logs it, and alerts the team on Telegram. Critical errors also send an email. Every 15 minutes it cancels expired reservations, tells the customer and returns the stock. Every day at 22:00 it sends a low-stock report to Telegram and to purchasing by email. |

## The website

One page (`index.html`, no frameworks) with a section for every workflow. Each section has three panels:

- **Phone:** shows what the customer or the team sees, with buttons to run sample actions.
- **n8n execution log:** shows the nodes running one by one.
- **Google Sheet:** shows the rows changing, with tabs.

The pharmacy section adds a Telegram panel where the pharmacist can approve or reject a request.

The phone, log and sheet panels run a **simulation with sample data**, and the page says so. The real forms (Apply and Coach) open the live n8n workflows. The Ask HR chat connects to the live assistant once its chat URL is added to the page.

## How it works

1. A visitor opens the website (GitHub Pages) and uses a form or the chat.
2. The request goes through a fixed ngrok address to my n8n instance running in Docker.
3. n8n runs the workflow and talks to Google Workspace (Sheets, Drive, Gmail), WhatsApp, Telegram and AI models.
4. If a workflow fails, the error handler logs it and alerts the team.

## Tech stack

- n8n (self-hosted with Docker)
- ngrok (static domain)
- Google Sheets, Google Drive, Gmail
- WhatsApp Business and Telegram bots
- AI models on free tiers (Google Gemini and OpenRouter, among others)
- GitHub Pages (static website, one `index.html`)

## Repository contents

- `index.html` - the project website
- `workflows/` - the cleaned n8n workflow files (no IDs, emails or keys):
  - the Recruitment 1, 2 and 3 files and the Job Application Coach file
  - `RAG.json`
  - `Dawaa-Requests&Reservations_from_pharmacy.json`
  - `Dawaa_error_workflow.json`

## Import the workflows into your own n8n

1. In n8n, create a folder for the project.
2. For each file in `workflows/`: **Create workflow → ... menu → Import from File**.
3. Create your own credentials (Google Sheets, Drive and Gmail OAuth2, WhatsApp, Telegram, and your AI model keys) and select them in every node with a warning triangle.
4. Replace every `YOUR_...` placeholder with your own Sheet, Drive folder, chat and email values.
5. Set the error workflow in each workflow's settings (the HR error handler for the HR workflows, the Dawaa error workflow for the pharmacy ones).
6. Add your policy documents to the RAG workflow through its upload form.
7. Activate the workflows and copy each trigger's **Production URL**.

## Security

No API keys, Sheet or Drive IDs, chat IDs or personal email addresses are stored in this repository. All credentials live only inside n8n.

## Author

**AbdAllah Hamdy** - Penetration tester and n8n automation builder
LinkedIn: https://linkedin.com/in/abdallahhamdycs
