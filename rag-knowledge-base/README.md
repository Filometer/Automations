# RAG Knowledge Base Assistant

An AI assistant that answers questions strictly from a company's
own documents, using retrieval-augmented generation (RAG). Built
for a logistics business, it stays grounded in real company
information instead of inventing answers, and it improves itself
by logging the questions it cannot answer.

## What it does

- **Answers from documents**: retrieves relevant passages from a
  vector database and answers only from what it finds
- **Stays honest**: if the documents do not contain the answer,
  it says so and offers to connect the customer to a human,
  rather than guessing
- **Improves itself**: logs every unanswered question to a sheet,
  so the team knows what content to add next

## Architecture

RAG works in two stages, built as two workflows that share one
vector database.

**1. Ingestion (run when documents change)**
Google Drive → download documents → extract clean text →
split into chunks → generate embeddings → store in Pinecone.

![RAG ingestion workflow in n8n](./rag-ingestion.png)

**2. Retrieval (runs on every question)**
Chat message → AI Agent → searches Pinecone for relevant chunks →
answers from them → logs the question if unanswered.

![RAG assistant workflow in n8n](./rag-assistant.png)

| Component | Choice |
|---|---|
| Automation | n8n |
| Vector database | Pinecone |
| Embeddings | Google Gemini (gemini-embedding-001) |
| Chat model | Groq (Llama 3.3 70B) |
| Documents | Google Drive |

## Setup

1. Import both workflows into n8n
2. Add your credentials: Google (Drive, Sheets), Pinecone, and a
   chat model key
3. Create a Pinecone index matching your embedding model's
   dimensions (3072 for gemini-embedding-001), metric cosine
4. Put your documents in a Google Drive folder as Google Docs
5. Run the ingestion workflow to load your documents
6. Test the assistant, then publish

## Key lessons

**RAG lives or dies on clean text.** The hardest part was not the
AI or the vector search, it was extracting readable text from the
source files. Documents arriving as binary or being split into
useless fragments produced garbage in the database. An explicit
text-extraction step, and correct chunking, was most of the work.

**Grounding and honesty matter.** The assistant is instructed to
answer only from retrieved content and to admit when it does not
know. This is what makes a document assistant trustworthy rather
than a confident guesser.

## Refreshing the knowledge base

When the source documents change, re-run the ingestion workflow so
the vector database reflects the updates. The assistant only knows
what has been ingested.

## Note on credentials

No API keys are included. n8n stores credentials separately from
the workflow files. Add your own before running.
