# n8n AI Research Assistant

My first AI Agent, built with n8n as part of my Data Science & AI Master's journey — and the subject of a video on my YouTube channel, Michora.

## What this project is

An AI-powered research assistant. You give it a question (e.g. "Find the best free AI tools for students in 2026"), and it uses an LLM to reason about the task, searches the web for relevant information, and returns a structured answer with key findings, sources, a recommendation, and limitations.

## Why this counts as an AI Agent (not just an LLM workflow)

In a normal automation or LLM workflow, the sequence of steps is fixed by the person building it — data always flows through the same steps in the same order. In this project, the LLM itself decides *what to do*: it evaluates the question, decides whether it needs to search the web, calls that tool when needed, reads the results, and decides whether it has enough information to answer or needs to search again. That decision-making loop — reasoning about which tool to use and when, rather than following a hardcoded sequence — is what makes this an agent rather than a scripted pipeline.

## Status

🚧 Work in progress — building this milestone by milestone as I learn n8n and agentic workflows.

## Tech stack

- [n8n](https://n8n.io) — workflow orchestration
- LLM API (TBD)
- Web search API (TBD)

## Milestones

- [x] Milestone 0 — Understand the architecture
- [x] Milestone 1 — Set up n8n
- [ ] Milestone 2 — Create the basic workflow
- [ ] ...