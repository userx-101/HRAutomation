HR-Automation
An HR automation system built with n8n. Candidates apply with a PDF CV, the system screens each application with AI, and every failure is logged and reported. Visitors can try the live workflows from a public website.

Live site: https://abdallah-hamdy.github.io/HRAutomation/

The workflows run on my own computer (Docker + n8n, exposed through ngrok). When my machine is off, the site shows Offline and the form buttons are disabled.

Workflows
Workflow	Trigger	What it does
Recruitment 1 - Intake	Form	Candidates apply with a PDF CV. It is validated, saved to Google Drive and logged to a Google Sheet. Duplicate applications are rejected.
Recruitment 2 - AI Screening	Schedule (every hour)	Reads new applications, scores each CV with AI, and emails an invite or a decline.
Recruitment 3 - Error Handler	Error trigger + schedules	Logs failures, emails alerts, sends a daily digest, and checks that the queue is moving.
Job Application Coach	Form	Reads a CV against a job post and emails a coaching report.
RAG + GPT (coming soon)	Chat	HR policy assistant that answers only from company policy documents.
The website

One page (index.html, no frameworks) with a section for each workflow:

Apply for a job and Application coach open the real n8n forms.
AI screening and Error handler run in the background, so the site explains them step by step.
Ask HR is a live chat connected to the RAG workflow (coming soon).
A status badge shows Online or Offline, and the form buttons are disabled when n8n is off.
How it works
A visitor opens the website (GitHub Pages) and clicks a form.
The request goes through a fixed ngrok address to my n8n instance running in Docker.
n8n runs the workflow and talks to Google Workspace (Sheets, Drive, Gmail) and AI models (Groq, Gemini).
If any workflow fails, the error handler logs it and sends an alert.
Tech stack
n8n (self-hosted with Docker)
ngrok (static domain)
Google Sheets, Google Drive, Gmail
Groq and Google Gemini (free tiers)
GitHub Pages (static website, one index.html)
Repository contents
index.html - the project website
workflows/ - the cleaned n8n workflow JSON files (no IDs, emails or keys)
Import the workflows into your own n8n
In n8n, create a folder for the project.
For each file in workflows/: Create workflow → ... menu → Import from File.
Create your own credentials (Google Sheets, Drive and Gmail OAuth2, Gemini and Groq API keys) and select them in every node with a warning triangle.
Replace every YOUR_... placeholder with your own Sheet, Drive folder and email values.
Set Recruitment 3 - Error Handler as the error workflow in the other workflows' settings.
Activate the workflows and copy each trigger's Production URL.
Security

No API keys, Sheet or Drive IDs, or personal email addresses are stored in this repository. All credentials live only inside n8n.

Author

AbdAllah Hamdy - Penetration tester and automation builder LinkedIn: https://linkedin.com/in/abdallahhamdycs
