# n8n AI Research Assistant

My first AI Agent, built with n8n as part of my Data Science & AI Master's journey — and the subject of a video on my YouTube channel, Michora.

## What this project is

An AI-powered research assistant. You give it a question (e.g. "Find the best free AI tools for students in 2026"), and it uses an LLM to reason about the task, searches the web when needed, and returns a structured answer with a short answer, key findings, sources, a recommendation, and limitations.

## Why this counts as an AI Agent (not just an LLM workflow)

In a normal automation or LLM workflow, the sequence of steps is fixed by the person building it — data always flows through the same steps in the same order. In this project, the LLM itself decides what to do: it evaluates the question, decides whether it needs to search the web, calls that tool when needed, reads the results, and decides whether it has enough information to answer or needs to search again. That decision-making loop — reasoning about which tool to use and when, rather than following a hardcoded sequence — is what makes this an agent rather than a scripted pipeline.

## Architecture

**Flow:**

1. **Chat Trigger** — user asks a question
2. **AI Agent** (Groq `gpt-oss-120b`) — reasons about the question and decides what it needs
3. Agent selectively calls a tool if needed:
   - **Calculator** — for math
   - **Tavily** — for web search / current information
4. Tool results are fed back to the Agent, which decides whether it has enough to answer
5. **Structured Output Parser** — enforces a consistent JSON schema (`short_answer`, `key_findings`, `sources`, `recommendation`, `limitations`)
6. **Edit Fields** — formats that JSON into readable text
7. Reply is sent back to the chat

```
Chat Trigger → AI Agent → [Calculator | Tavily] → Structured Output Parser → Edit Fields → Chat Reply
```

## Features

- Reasons about the user's question before acting (agentic loop, not a fixed pipeline)
- Selectively calls tools only when needed (verified: skips the calculator on simple math, uses it on harder math)
- Searches the live web via Tavily for current information the LLM alone doesn't know
- Returns a consistently structured answer via a Structured Output Parser
- Honest about uncertainty — explicitly instructed to admit when it doesn't know rather than guess

## Tech stack

- **[n8n](https://n8n.io)** — workflow orchestration (self-hosted locally via Docker)
- **[Groq](https://console.groq.com)** — LLM inference (`openai/gpt-oss-120b`), free tier, no card required
- **[Tavily](https://tavily.com)** — web search API built for LLM agents, free tier (1,000 requests/month), no card required
- **n8n Calculator tool** — built-in, for numeric reasoning
- **n8n Structured Output Parser** — enforces consistent JSON response format

## Setup

1. Run n8n locally (Docker recommended):
```bash
   docker volume create n8n_data
   docker run -it --rm --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```
   Open `http://localhost:5678`.

2. Import `workflow.json` from this repo into n8n (Workflows → Import from File).

3. Get free API keys:
   - **Groq**: sign up at [console.groq.com](https://console.groq.com), create an API key (no card required). *Verify current free-tier limits — they can change.*
   - **Tavily**: sign up at [tavily.com](https://tavily.com), copy your API key from the dashboard (no card required). *Verify current free-tier limits — they can change.*

4. In n8n, add each key as a new credential on the corresponding node (Groq Chat Model, Tavily). **Credentials are never stored in this repo** — they live encrypted inside your own local n8n instance.

5. Open the chat panel and ask a question.

## Example

**Input:** `What is the best free AI tool for students in 2026?`

**Output:**
- **Short answer:** [concise direct answer]
- **Key findings:** [bullet points from research]
- **Sources:** [real URLs from Tavily]
- **Recommendation:** [actionable suggestion]
- **Limitations:** [honest caveats, e.g. results may shift as new tools launch]

## Limitations

- Free-tier rate limits apply: Groq caps requests at 8,000 tokens/minute, which can be hit by very long questions combined with large search results (observed during testing).
- No dedicated error-handling branches yet — API failures currently surface as raw n8n errors rather than a graceful fallback message. Planned for a future iteration.
- Single-agent architecture — no multi-agent handoff or long-term memory across sessions.
- Answer quality depends on what Tavily's search surfaces; obscure or very recent topics may return limited results.

## Future improvements

- Add graceful error handling (fallback messages on tool/API failure)
- Add conversation memory for follow-up questions
- Add a second search tool as a fallback if Tavily is rate-limited
- Deploy publicly (currently local-only)

## Milestones

- [x] Milestone 0 — Understand the architecture
- [x] Milestone 1 — Set up n8n
- [x] Milestone 2 — Create the basic workflow
- [x] Milestone 3 — Connect the LLM
- [x] Milestone 4 — Turn it into an agent
- [x] Milestone 5 — Give the agent a tool
- [x] Milestone 6 — Add web research
- [x] Milestone 7 — Improve the output
- [x] Milestone 8 — Error handling *(skipped for v1 — see Limitations)*
- [x] Milestone 9 — Testing


## About

Built by Chaimaa matrag as a learning project for a Master's in Data Science & AI.