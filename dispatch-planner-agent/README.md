Dispatch Planner Agent
An AI agent that automates daily dispatch planning for a logistics/delivery operation — from reading open orders to notifying each driver of their route, with a human approval step in between.
What it does
Every morning, this workflow:
Reads live data — pulls open orders and available fleet/driver data from Google Sheets
Reasons over constraints — an AI model (Google Gemini) builds a route plan that respects vehicle capacity, delivery zones, and delivery time windows, and explicitly flags any order it can't reasonably fit rather than forcing a bad assignment
Writes the proposed plan back to a sheet for visibility and audit history
Routes it for human approval — emails a dispatcher a link to review the plan and approve or reject it (with comments) before anything goes live
On approval, writes the finalized routes and splits them per driver
Notifies each driver individually via WhatsApp with just their own stops — no manual messaging required
Nothing reaches a driver without a human explicitly approving the day's plan first.
Architecture
```
Schedule Trigger
  → Read Orders (Google Sheets)
  → Read Fleet (Google Sheets)
  → Combine data
  → Build prompt
  → Gemini (AI reasoning)
  → Parse plan (JSON)
  → Flatten routes
  → Write to "Proposed Plan" sheet
  → Build approval summary
  → Send email (dispatcher)
  → Wait (pauses for form submission)
  → IF (Approve / Reject)
      ├─ Approve → Prepare live routes → Write "Live Routes" sheet
      │            → Group stops by driver → Send WhatsApp (per driver)
      └─ Reject  → Build revision prompt (with dispatcher feedback) → loops back to Gemini
```
Key skills demonstrated
AI agent design with tool use and structured JSON output
Human-in-the-loop approval pattern (pause/resume via n8n's Wait node + form)
WhatsApp Business Cloud API integration (message templates, business-initiated messaging)
Google Sheets as a lightweight operational database
Prompt engineering for constraint-based reasoning (capacity, time windows, zones)
Error handling and retry logic (rejection loop with dispatcher feedback)
Setup
This workflow uses placeholder values that need to be replaced with your own before running:
`YOUR_PHONE_NUMBER_ID` — your WhatsApp Business phone number ID from Meta's developer dashboard
`YOUR_GOOGLE_SHEET_ID` — the ID of your own Google Sheet (see `sample-data.xlsx` in this folder for the expected structure: Orders, Fleet, Proposed Plan, Live Routes, and Run Log tabs)
A configured Google Sheets credential in n8n
A configured Google Gemini credential in n8n
A configured WhatsApp Business API credential in n8n, plus an approved message template for the driver notification
Sample data
`sample-data.xlsx` included in this folder shows the expected sheet structure with demo orders and fleet data, so you can test the workflow end to end before connecting your own operational data.
