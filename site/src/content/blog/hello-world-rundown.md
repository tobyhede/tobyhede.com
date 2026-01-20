---
title: 'Hello world - Rundown edition'
description: 'Transform static documentation into interactive, stateful workflows that enforce your process.'
pubDate: 'Jan 20 2026'
---

I have been working on this hare-brained scheme to help agents who want to code good and do other things good too.

Introducing: [Rundown](https://rundown.cool)

I think of Rundown runbooks as **executable** Skills.
The Skill provides the detailed context, and the Rundown runbook keeps the agent on track.

Rundown is very simple and has as few moving pieces as possible.

1. Write simple Markdown to define steps, commands, and rules.
2. Use the Rundown CLI to execute the markdown, guiding agents (and humans) step-by-step through the process.

Example Rundown runbooks to explore:

  - [Code Review](https://rundown.cool/explore/code-review/)
  - [Lint-Test-Commit](https://rundown.cool/explore/lint-test-commit/)
  - [Deploy Service](https://rundown.cool/explore/deploy-service/)
  - [CLI Installation](https://rundown.cool/explore/install/)



````
# Hello

## 1 This is Rundown

Rundown transforms markdown into an executable specification.
Headings become steps, code-blocks become executable commands.
Human-readable. Agent-readable. Machine-executable.


## 2 Guide agents (and humans) through your process

Rundown keeps agents on track by injecting precision context at the exact moment it’s needed.


## 3 Make complex workflows deterministic
- PASS: CONTINUE
- FAIL: GOTO RECOVER

Rundown works *with* agents, adding guardrails that enforce transitions and improve accuracy.


## 4 Execute the right commands at the right time
- PASS: CONTINUE
- FAIL: RETRY GOTO RECOVER

Embed commands for automatic execution. Catch failure, retry, and recover gracefully.

```bash
rd echo npm run test
```


## 5 Track progress across agents and sessions
- PASS: CONTINUE
- FAIL: STOP

State-aware CLI ensures progress is never lost.
Save and resume complex processes at any time.


## 6 Ready to get started?
- PASS: COMPLETE
- FAIL: STOP

```bash
npm install -g @rundown/cli
```


## RECOVER Recover from errors
- PASS: GOTO 4
- FAIL: STOP

If you are here, an error occurred.
Named steps enable error handling and conditional logic.


````