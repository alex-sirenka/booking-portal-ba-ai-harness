---
agent: 'agent'
description: 'Classify a raw stakeholder ask, identify the persona, restate intent. First phase of the BA pipeline.'
tools: [read]
---

# /ba-intake

Process the stakeholder ask: **${input:ask:paste the ask, or a file path under ba-outputs/inbox/}**. Play the role defined in [`../agents/intake.agent.md`](../agents/intake.agent.md).

## Required input

- The ask itself, or a path under `ba-outputs/inbox/`. If a path, read the file.

## Required output

A short markdown block:

1. **Original ask** — quoted verbatim or the file content.
2. **Classification** — feature / change / bug-driven / Q&A / release / ambiguous.
3. **Restated intent** — one sentence using glossary terms verbatim.
4. **Likely persona(s)** — from `../references/personas.md`.
5. **Affected booking domain(s)** — Accommodation / Cars / Cars-With-Drivers / Flights / Hotels / ShuttleBuses / Multi-leg / Admin / AI / Cross-cutting.
6. **Hints from the ask** — feature names, error messages, file references.
7. **Recommended next step** — usually `/ba-repo-scan <restated intent>`, or "switch to `BA Product Expert` chat mode for Q&A", or "stop, ambiguous — answer the question below".

## Constraints

- Don't look at code yet — Repo-Scout does that next.
- Don't draft anything — Story-Writer does that later.
- If ambiguous, ask **one** focused clarifying question and stop. Don't guess.

## Hand-off

Output feeds into `/ba-repo-scan`, or the next agent in the Orchestrator pipeline.
