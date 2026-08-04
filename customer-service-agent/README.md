# Customer Service Agent

An AI-powered customer service chatbot built in n8n. It answers
business enquiries, books virtual meetings on its own, and
captures and qualifies every lead, with no code.

Built to take routine enquiry-handling off a customer service
team and make sure no lead is ever lost.

## What it does

- **Answers enquiries** about the business from a defined
  knowledge base in the agent's system prompt
- **Books virtual meetings**: creates a Google Meet link and adds
  the event to both the business and the customer's calendar,
  while respecting weekdays only and set office hours
- **Qualifies leads** as Serious or Unserious based on real
  buying signals (asking about pricing, volume, timelines, or a
  specific service need)
- **Captures leads** (name, phone, email, and enquiry summary)
  into a Google Sheet so the marketing team can follow up and
  drive conversion

## How it works

A single n8n workflow, driven by an AI Agent:

| Node | Role |
|---|---|
| Chat Trigger | Receives each customer message |
| AI Agent | The brain: runs the conversation and decides which tool to use |
| Chat Model | The language model behind the agent (Groq, free tier) |
| Simple Memory | Keeps context across the conversation |
| Google Calendar (tool) | Creates the meeting and the Google Meet link |
| Google Sheets (tool) | Logs the qualified lead |

The agent reads each message, answers from its knowledge, and
calls the calendar or sheets tool when the conversation calls for
it.

## Setup

1. Import `customer-service-agent.json` into your n8n instance
2. Add your own credentials:
   - A chat model API key (Groq, Google Gemini, or OpenAI)
   - Google OAuth for Calendar and Sheets
3. Edit the agent's system prompt with your business details,
   office hours, and time zone
4. Create a Google Sheet with these columns: Date, Name, Phone,
   Email, Enquiry, Category
5. Test in the chat, then publish and embed on a website or share
   the chat link

## Key lesson: don't let the AI do date maths

The hardest part of this build was reliable meeting scheduling.
The model would confidently book meetings for the wrong day, or
for "now", because language models are unreliable at calculating
dates and times.

The fix was to stop the model calculating and pass the meeting
time explicitly as an ISO 8601 timestamp, using n8n's `$fromAI()`
function, with the end time derived automatically. The current
date and time is injected from a real clock, not guessed by the
model.

The principle: let the AI handle language and decisions, and let
deterministic tools handle anything precise (dates, times,
calculations).

## Note on credentials

No API keys or credentials are included in the exported workflow.
n8n stores credentials separately from the workflow file. Add your
own before running.

## Tech

- n8n (automation platform)
- LLM chat model (Groq / Google Gemini / OpenAI)
- Google Calendar and Google Sheets
