# Assignment 3 – n8n RAG Chatbot: Credit Appraisal Policy Assistant

A Retrieval-Augmented Generation (RAG) chatbot built in **n8n**. It answers loan-eligibility questions strictly from a policy document, and says so when the document doesn't cover something, rather than guessing from the model's general knowledge.

## Domain

Financial services – credit appraisal for two products, **Car Loan** and **Home Loan**. Typical questions are about:

- minimum bureau (credit) score
- maximum Loan-to-Value (LTV)
- Fixed Obligations to Income Ratio (FOIR)
- maximum tenor
- applicant age at maturity
- any other criteria written into the policy

The bot applies only the rules for the loan product being asked about and never fills in missing values.

## Knowledge base

`Credit_Appraisal_RAG_Knowledge_Base.docx` contains the car and home loan eligibility policies, guidelines on applying them, grounding instructions for the RAG, sample test questions (including ones that must return "not available in policy"), a quick-reference section, and a statement of scope.

## Approach

Vector-based RAG using n8n's in-memory **Simple Vector Store**. Documents are embedded once; at question time the closest chunks are retrieved by semantic similarity and passed to the agent. This suits policy text, where exact values matter, the document may change, and answers must stay grounded.

| Part | Choice |
|---|---|
| Chat model | `llama3.1:latest` (Ollama) |
| Embedding model | `mxbai-embed-large:latest` (Ollama) |
| Vector store | n8n Simple Vector Store |
| Memory | Simple Memory |

## Pipelines

**Ingestion (run manually)**

```text
Manual Trigger -> Google Drive (download file) -> Default Data Loader (chunks)
              -> Embeddings (Ollama) -> Simple Vector Store (insert)
```

**Question answering**

```text
Chat Trigger -> AI Agent <- Ollama Chat Model
                    ^   <- Simple Memory
                    ^   <- Vector Store Tool (retrieve) <- Embeddings (Ollama)
                    v
              Final answer
```

For each question the agent calls the vector store tool, which embeds the question, finds the most similar policy chunks and returns them. The agent then answers using only that text.

## Prompts

**Vector store tool description** – tells the agent when to search: for any car or home loan eligibility question. It must use only what is in the knowledge base, apply only the relevant product's rules, and never invent, estimate or substitute values.

**Agent system prompt** – the agent is a Credit Appraisal Policy Assistant whose only source of facts is the policy search tool. It must search before answering, quote exact values, keep car and home loan rules separate, and reply "I couldn't find this information in the provided policy." when the answer isn't there.

## Files

| File | Purpose |
|---|---|
| `RAG Assignment.json` | Exported n8n workflow |
| `Credit_Appraisal_RAG_Knowledge_Base.docx` | Policy knowledge base |
| `screenshots/` | Workflow, configuration and chat screenshots *(to be added)* |

## Setup notes

Credential IDs and the Google Drive file ID in the JSON are `YOUR_…` placeholders. After importing:

1. Upload the `.docx` to your Google Drive and pick it in the *Download file* node.
2. Connect your Google Drive and Ollama credentials.
3. Pull `llama3.1` and `mxbai-embed-large` in Ollama.
4. Run the ingestion flow once, then chat. The store is in memory, so re-run ingestion after n8n restarts.
