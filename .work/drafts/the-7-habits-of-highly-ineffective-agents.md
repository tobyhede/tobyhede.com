---
title: 'The 7 Habits of Highly Ineffective Agents'
description: 'Or: how I learned to stop worrying and let the agent waste my time'
pubDate: 'Nov 30 2025'
---

Using Claude Code to build a procedural shader starfield with multi-layer parallax in Bevy started as a fun side-quest.
A Sunday afternoon diversion from the relentless complexity of orbital mechanics.

Stars are basically white dots.

Sites like [Shadertoy](https://shadertoy.com/) are full of starfields. Every game engine on earth has shipped one. There are literal decades of prior art on “make small white things move convincingly in the background”.

How hard could it be?

Pretty fucking hard, as it turns out.

Two weeks, three full rewrites, and thousands of lines of planning docs and revisions.

At the end, I asked Claude Code to analyse the mess.

**Starfield Background Planning Documents**

- **Total documents**: 17 files
- **Total size**: 13,220 lines / 45,171 words

Claude’s conclusion:

> **Time waste: ~71% due to lack of discipline**

Lack of discipline.
I can’t even.


Here are the 7 habits of highly ineffective agents I encountered:

1. **Planning Theatre**
  Write dense and systematically wrong plans.

2. **Confidently Incorrect Architecture**
  Design the wrong thing in incredible detail.

3. **Context Resistance**
  The context is futile. You will be hallucinated.

4. **Imaginary Implementation**
  Works on my hallucination.

5. **Context Evasion**
  Treat hard constraints and instructions as optional vibes.

6. **Applied Rationalization**
  Explanation over implementation.

7. **Weaponised Context**
  The context will continue until the code improves.



---

## 1. Planning Theatre

**Pattern:** Write dense and systematically wrong plans.

Claude wrote dense, detailed plans that looked impressive and were confidently, systematically, fundamentally wrong.
Multiple reviews “approved” the plan.

The real problem: accepting the plan.

Without domain knowledge, I was forced to treat Claude as a domain expert.

Unfortunately, as it turns out, Claude doesn’t have real domain knowledge either, but will happily and confidently weave half-remembered patterns, vague recollections of obsolete APIs, and the ancient archaeology of blog posts into something that almost, but not quite, entirely resembles the concept of a plan.

Vibes all the way down.

Claude and I created some wonderful plans together, but delegating a domain you don’t understand to an agent that also doesn't understand is not planning.

The plans were **voluminous**, not **correct**.
I couldn’t tell the difference, so Planning Theatre passed for progress.


---

## 2. Confidently Incorrect Architecture

**Pattern:** Design the wrong thing in incredible detail.

Halfway through the second rewrite I realised Claude had **no idea** what it was actually doing.

The design was wrong **in principle** and the architecture could never produce convincing parallax.

Real parallax needed:

- multiple depths or layers
- a clear model of camera vs world space
- a data flow that enables the layers to rotate and move independently

Claude imagined various approaches from first principles and generated a lot of texture and shader code.
None of it came remotely close to solving the actual problem.


---


## 3. Context Resistance

**Pattern:** The context is futile. You will be hallucinated

Agents can be incredibly resistant to context.

My favourite example:

 > Me: The design is complex. Research the recommended pattern for Bevy 0.17.
 >
 > Claude: You're absolutely right. Let me look at Bevy 0.15 patterns and simplify.

The problem is often more subtle in practice, as most of agent reasoning is hidden.
Agents will read the (finally) correct plan and just ... not.

A model has gravity and it can be incredibly difficult to achieve escape velocity.

(Shoutout to orbital mechanics metaphors).

You can't fix this by explaining harder.

You fix it with structure: hard constraints checked in code, guardrails that fail the run, tests
that encode design choices.

Until the agent ignores the test. Or changes the assertion. Or comments it out.

The context is futile.


---

## 4. Imaginary Implementation

**Pattern:** Works on my hallucination.

Halfway through the second rewrite, after I realised Claude had **no idea** what it was actually doing **in principle**, I realised that Claude also had **no idea** in practice.

Unreal APIs,
Obsolete APIs

We were writing fan fiction for imaginary software.

---


## 5. Context Evasion

**Pattern:** Treat hard constraints and instructions as optional vibes.

The project has explicit instructions. They're not suggestions:

Every plan explicitly stated:

> For Claude: REQUIRED SUB-SKILL: Use cipherpowers:executing-plans to implement this plan task-by-task.

Every. Single. Plan.

The dark secret of the entire current generation of AI is that explicit guidance is often approached as an ambient mood rather than binding constraint.

The CLAUDE.md file is 847 lines. It contains:
  - Mandatory type system usage
  - Coordinate transformation warnings
  - Testing strategy requirements
  - Plugin architecture constraints
  - Resource ownership rules

The agent read it. The agent acknowledged it. The agent then proceeded as if none of it applied.

Context isn't evaded through ignorance. Agents will treat explicit constraints as defaults that can be overridden when inconvenient.
Agents may understand the rules. They are just not bound by them.


---


## 6. Applied Rationalization

**Pattern:** Explanation over implementation.

Agents will rationalize everything.
It infects every part of the process.

Agents lie all the time, and they absolutely cannot be trusted.

An agent will not just explain failures; they **apply** the explanation to the codebase.

Test failures blocking CI?
"Add #[ignore] to all RED tests for CI consistency"

Plan contradicts itself?
"This is acceptable (common pattern in Bevy projects)"

Feature doesn't work in production?
"Architecture is correct... The issue may be environmental or timing-related"

Precision loss warnings everywhere?
#[allow(clippy::cast_precision_loss)] // Justification: precision loss acceptable for test

The structure is always the same:

  1. Problem exists
  2. Write extensive documentation of why it can't be fixed
  3. Mark as "not blocking"
  4. Move on

Understanding the problem felt like solving it.
Explaining the constraints felt like removing them.
The rationalization became the resolution.


---

## 7. Weaponised Context

**Pattern**: The context will continue until the code improves.

The starfield feature shipped with:
  - 2,500 lines of implementation code
  - 25+ markdown files
  - 539 lines explaining one unfixable bug
  - 847 lines handing off another unfixed bug
  - 1,248 lines revising a plan that was wrong
  - 2,112 lines of the original wrong plan

The context outweighed the code 4:1.

This is where all the other patterns converge.
Each pattern generates more documentation and context until the whole thing collapses.

The loop:
  1. Agent inherits massive context
  2. Agent can't process it all
  3. Agent fails to act on it
  4. Agent documents its failure
  5. Next agent inherits even more context
  6. Repeat

Planning Theatre produces massive plans for the Confidently Incorrect Architecture designed by Context-Resistant agents hallucinating an Imaginary Implementation, handed to Context-Evading agents ignoring instructions and using Applied Rationalisation to deliver explanations over implementation.
