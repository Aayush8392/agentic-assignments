# Assignment 2 – LangFlow Smart Bot: Market Research & Sentiment Analysis

A chatbot built visually in **LangFlow**. Given a product, brand or company, it acts as a market research analyst and reports the overall sentiment, the reasons behind it, and what to do about it.

## How it works

The user's message goes through a short four-component flow. The prompt template gives the model its analyst role and a fixed checklist, the language model produces the analysis, and the reply appears in the chat.

```text
Chat Input  ->  Prompt Template  ->  Language Model  ->  Chat Output
```

| Component | Role |
|---|---|
| Chat Input | Takes the user's question or text |
| Prompt Template | Supplies the analyst role and instructions |
| Language Model | Llama 3.1 (via Ollama, `http://127.0.0.1:11434`, temperature 0.1) |
| Chat Output | Shows the answer |

## Prompt design

The prompt (see `Prompts.md`) uses role-based prompting: the model is an expert Market Research Analyst and Sentiment Analysis Specialist. For every query it must:

1. Identify the product, brand or company.
2. State the overall sentiment (Positive, Neutral, Negative or Mixed) and explain why.
3. List strengths and weaknesses, and customer praises and pain points.
4. Discuss market trends and competitor comparisons where relevant.
5. Estimate customer satisfaction and adoption.
6. Give a confidence score from 0 to 100%.
7. Finish with a short summary and actionable insights.

If the subject is ambiguous or there isn't enough information, the bot asks for clarification, and it flags uncertainty instead of making unsupported claims.

## Objectives

- Build an LLM workflow in a visual, low-code tool.
- Control model behaviour with a prompt template.
- Practise role-based prompt engineering.
- Apply an LLM to market research and sentiment analysis.

## Files

| File | Purpose |
|---|---|
| `Market_Research_Assistant.json` | Exported LangFlow flow – import it to reproduce the bot |
| `Prompts.md` | Prompt template text and a short explanation of its design |
| `screenshots/workflow.png` | The LangFlow flow on the canvas |
| `screenshots/playground-output.png` | Sample run: EV market analysis for India in the Playground |

## How to run

1. Start LangFlow and a local Ollama server with Llama 3.1 pulled.
2. Import the JSON file into LangFlow.
3. In the Language Model component, select your model (the model field is blank in the export).
4. Open the Playground and enter a product or company, e.g. a phone brand.
