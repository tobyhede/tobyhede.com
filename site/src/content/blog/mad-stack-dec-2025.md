---
title: 'Machine-Assisted Development stack'
description: 'The complicated incantantion of models and markdown that I have woven into a Machine-Assisted Development environment.'
pubDate: 'Nov 29 2025'
---

## December 2025 edition

Current setup:

* **Claude Code** – running via CLI
* **Cipherpowers** – commands, agents, and skills
* **Claudish + OpenRouter** – route Claude Code through different models
* **Codex** – plan & code review
* **Turboshovel** – workflow enforcement via gates and hooks

---

## Claude Code (CLI)

Claude Code is an agentic CLI tool and the hub of the current stack

All the other components (Cipherpowers, Claudish, Codex, Turboshovel) plug into this.

---

## Cipherpowers

**[Cipherpowers](https://github.com/cipherstash/cipherpowers)** is a plugin for Claude Code that provides the foundation context through commands, agents, skills, principles, and standards.

The core components are:
* **Commands** – starting prompts for a workflow or process (`/cipherpowers:code-review`)
* **Agents** – specialised subagents (`/cipherpowers:code-review-agent`)
* **Skills** – reusable capabilities (`conducting-code-review`)

Commands prompt the "Main" Agent with the specific skills to use and provide reinforcement to encourage compliance. Commands wrap the skills, all of the important context should be delegated.

Agent definitions are similar, and prompt subagents with the required skills and attempt to enforce compliance.

Skills contain the actual prompt instructions for a particular task.

Using subagents is esential for effectively managing context. The Main Agent can remain an orchestrator, and the subagents get loaded with the detailed context.

---

## Claudish + OpenRouter

**Claudish** is a recent addition, and enables the same workflow to run on different models via **[OpenRouter](https://openrouter.ai/)**.

Using other models to review and verify the output of the primary Claude Code models has **dramatically** improved the quality.

Primary models:

- `google/gemini-3-pro-preview` (rarely because expensive)
- `x-ai/grok-code-fast-1` (cheap and good enough for review)
- `minimax/minimax-m2` (even cheaper and good enough)

Using additional models for review will nearly always reveal issues that the primary working model cannot identify.

---

## Codex

**Codex** is the OpenAI version of Claude Code, used primarily for code review.
I will need to crunch the numbers, but it may be replaced by Claudish.

---

## TurboShovel

**[TurboShovel](https://github.com/tobyhede/turboshovel)** is another Claude Code plugin.

It adds some quality-of-life affordances to the default Claude Code hook engine.

- **automatic context injection** - inject project-specific prompts and context, using markdown and file naming conventions for zero-effort integration
- **quality gates** - verify and block task completion until pass


---

## Workflow Overview

The rough loop:

1. **Brainstorm** – explore options and constraints with Claude Code
2. **Plan** – create a concrete, stepwise plan (via Cipherpowers + Codex)
4. **Verify**
   - different agents and/or different models
   - collate analyses and resolve disagreements
   - iterate and refine
5. **Execute**
   - implement tasks in batches
   - with specialised agents (`rust-agent`)
   - TurboShovel gates enforce checks
   - code review after each batch

---

## What actually matters

The specific tools will change. The useful bits are:

- **Curated context** – commands, agents, and skills instead of one-shot prompts
- **Explicit workflows** - automatic gates for enforcement, and context injection for encouragement and reducing context load
- **Multiple models** – a second (or third) model whose job is to disagree



