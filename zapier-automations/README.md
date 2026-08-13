# Zapier Automations

Two automations built in Zapier, one for capturing and triaging
client inquiries, one for generating and publishing content with
AI. Together they show both classic workflow automation and an
AI-powered content pipeline.

> Zapier does not support exporting a Zap as a shareable file, so
> each automation is documented here with an annotated screenshot
> of its trigger and action steps.

---

## 1. New Client Inquiry Workflow

Captures every inquiry from a web form, logs it, and sends the
right emails, so no inquiry is missed and follow-up is automatic.

![New Client Inquiry Workflow](./new-client-inquiry.png)

**How it works**

| Step | App | Action |
|---|---|---|
| Trigger | Google Forms | New form response comes in |
| 2 | Google Sheets | Log the inquiry as a new row |
| 3 | Gmail | Send an acknowledgement email |
| 4 | Filter by Zapier | Continue only when the inquiry meets set conditions |
| 5 | Gmail | Send a second, conditional email |

**Why it is built this way**

- Logging the inquiry first means every submission is recorded,
  even if a later step is filtered out.
- The Filter step means the second email only goes out for
  inquiries that qualify, rather than to everyone.

---

## 2. Blog Idea Generator

An AI content pipeline: it takes an idea from a sheet, expands it
with ChatGPT, saves the result, and publishes it to LinkedIn,
turning a one-line prompt into a posted update.

![Blog Idea Generator](./blog-idea-generator.png)

**How it works**

| Step | App | Action |
|---|---|---|
| Trigger | Google Sheets | A new or updated row is detected |
| 2 | Filter by Zapier | Continue only for rows marked ready |
| 3 | ChatGPT (OpenAI) | Generate content from the row |
| 4 | Google Sheets | Write the generated content back to the row |
| 5 | LinkedIn | Publish the content as a share update |

**Why it is built this way**

- The Filter step means only rows you have marked ready are
  processed, so nothing is posted by accident.
- Writing the result back to the sheet keeps a record of what was
  generated before it is published.

---

## Note on credentials

Screenshots use no real customer data. No API keys, tokens, or
account details are shown. To rebuild these, connect your own
Google, Gmail, OpenAI, and LinkedIn accounts in Zapier.
