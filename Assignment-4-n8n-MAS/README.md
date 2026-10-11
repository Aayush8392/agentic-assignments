# Assignment 4 – n8n Multi-Agent System

Two AI agents in **n8n** working one after the other: the first researches the question on the web, the second turns that research into the final answer.

## Why split the work

One agent doing everything tends to give shallow or unfocused answers. Giving each agent a single job – gather evidence, then interpret it – keeps both prompts simple and the result better organised. This is a **sequential** multi-agent design: the output of agent 1 becomes the input of agent 2.

## Architecture

```text
User (chat message)
       |
       v
Agent 1 – Information Research   <- Ollama chat model + Tavily search tool
       |  research output
       v
Agent 2 – Final Analysis         <- Ollama chat model
       |
       v
Final response
```

## The agents

**Agent 1 – Information Research** (`Agent 1 - Research.txt`)
Finds the key keywords, concepts and sub-topics in the question, searches the web with Tavily across several reliable sources, and hands over a well-organised set of findings. It collects; it doesn't conclude.

**Agent 2 – Final Analysis** (`Agent 2 - Analysis.txt`)
Takes agent 1's output as its main source, works out what kind of answer the user needs (summary, sentiment analysis, comparison, key findings, recommendation, insight or conclusion) and writes it. It returns the result directly to the user.

## Tools

| Tool | Use |
|---|---|
| n8n | Workflow, chat trigger, agent nodes |
| Ollama (`llama3.1:latest`) | Local LLM for the agents |
| Tavily | Web search for agent 1 |

## Objectives

- Understand multi-agent systems and sequential agent chains.
- Connect a local LLM and a web search tool in n8n.
- Pass one agent's output to another as context.
- Produce a combined, final answer and document the run.

## Files

| File | Purpose |
|---|---|
| `AI agent class assignment 4- multiagent.json` | Exported n8n workflow |
| `Agent 1 - Research.txt` | System prompt of the research agent (taken from the workflow) |
| `Agent 2 - Analysis.txt` | System prompt of the analysis agent (taken from the workflow) |
| `screenshots/` | Workflow, Tavily and chat screenshots *(to be added)* |

## Setup notes

Credential IDs in the JSON are placeholders. After importing, connect your own Ollama and Tavily credentials.
