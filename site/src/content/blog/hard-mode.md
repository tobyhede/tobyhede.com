---
title: 'Working with Claude Code in hard mode'
description: 'Using Claude Code to create a Rust Bevy 0.17 space simulation using full orbital mechanics and real physics.'
pubDate: 'Nov 23 2025'
---

After a long time working deep in the computering mines I recently decided to drag myself kicking and screaming into the Century of the Fruitbat, and learn how to machine whisper with AI.

My explicit goal is to push beyond **“vibe coding”** and understand how to hold the technology for actual AI-assisted engineering.

I want to understand how AI fits into the RealWorld™ and actual software engineering work.

To find out, I decided to run in **Hard Mode** - using Claude Code to create a Rust Bevy 0.17 space simulation using full orbital mechanics and real physics.


---

## What makes this “Hard Mode”

The experiment looks like this:

1. **Claude Code CLI**
Plus extensive collection of custom agents, commands, and skills.

2. **Bleeding edge technology**
Bevy 0.17. An infamously moving target that changes, sometimes radically, on a ~3-month cadence.

3. **Zero physics domain knowledge**
Plus full orbital mechanics.

4. **Zero game development domain knowledge**
Plus 3D rendering, and fragment & compute shaders

5. **Strict RealWorld™ quality standards**
Rigorously and relentlessly idiomatic, modular, tested, documented, and etc and etc.

Perhaps this is slightly unhinged way to evaluate the machine, but that is the point:

> What better way to find out than to deliberately turn the difficulty up to 11?


---

## Bleeding edge technology

Bevy is excellent, but it is also famous for its ruthlessly fast-moving API.

The [Bevy Quick Start](https://bevy.org/learn/quick-start/introduction/) has a disclaimer that should strike fear into the heart of any LLM:

> Bevy is still in the early stages of development… A new version of Bevy containing breaking changes to the API is released approximately once every 3 months.

Perfect.

Agents rely heavily on **implicit knowledge**. Pattern matching against the billions of lines of code they were trained on. Vibe coding works because the model is optimised for shovelling large volumes of code that are a very slight variation of the large volumes of code it has seen before.

When working with Bevy, the machine is always working with outdated knowledge with hilariously infuriating consequences:

- It confidently suggests APIs that no longer exist
- It emits syntactically correct Rust that simply doesn’t compile
- It blends together blog posts, old examples and hallucinations into something that looks real until you hit `cargo build`
- It will happily acknowledge the correct version, read the correct documentation, and promptly implement code for an imagined Bevy 0.17/16.15.14 API.

This isn’t a bug; it’s the default state when you put a probabilistic model on an unstable stack.

Hard mode means: **the model is (nearly) always wrong**


## Zero domain knowledge

**Domain knowledge**
  - physics: nope
  - games: nope

I don’t know any physics.
I don’t know anything about game development.
(Note: I do know Rust, and have been Rusting in production for a couple of years).

Zero domain knowledge means:
- I have no prior knowledge of any physics concepts
- I have no understanding of any of the math(s) involved
- I have no intuition for the orbital calculations
- I have no mental model for how game rendering pipelines are usually structured
- I have minimal understanding of what makes an idiomatic Bevy application

I can’t help the machine.
I have zero implicit knowledge that I can rely on to shape context.
I have no intuition that the output is good, or yet another delivery of HallucinatedHotMess™.

Hard mode means: **the model is (nearly) always wrong, but I don't know if it is wrong**


## To infinity and beyond

I am trying to answer the question: What even is AI-assisted engineering in this, the Century of the Fruitbat, CE 2025?

Hard mode is an experiment. A lab.
  1. Pick a hard, unstable stack (Bevy 0.17 + orbital mechanics)
  2. Enforce rigorous RealWorld™ quality standards
  3. Use the machine for AI-assisted collaboration and engineering
  4. Discover and refine our machine-whispering techniques and processes

I have spent a long career working deep in the computering mines, shovelling code by hand.
It is time to embrace the machine-powered TurboShovel™.

Maybe it works. Maybe it doesn’t.
Either way, I want more than “vibes”.

