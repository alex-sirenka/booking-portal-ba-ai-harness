---
agent: 'agent'
description: 'Review a BA artifact (story, impact, gap, scan, release notes, or BRD) for completeness, glossary, template, and traceability.'
tools: [read, search]
---

# /ba-quality-check

Review the artifact: **${input:artifact:path to the file to review}**. Play the role defined in [`../agents/quality-checker.agent.md`](../agents/quality-checker.agent.md) and follow [`../skills/ba-quality-check/SKILL.md`](../skills/ba-quality-check/SKILL.md).

## Required input

- A file path under `.github/ba-outputs/`.

## Required output

A reviewer's note in the four-section response contract:

1. **What I did** — artifact type and checks performed. Acceptance criteria are reviewed as part of a story rather than as a standalone artifact type.
2. **What I found** — issues with locations and fixes, glossary compliance, permissions, traceability, and the verdict (`pass`, `pass with minor fixes`, or `revise and resubmit`).
3. **What I'm passing forward** — approved content or corrections required from the originating agent.
4. **What stopped me** — blockers, or `Nothing`.

## Constraints

- Don't rewrite the artifact. Only flag.
- Be specific: "section X has Y problem because Z, fix by W" — not "this is bad".
- Apply the matching template's required sections from `../templates/`.

## Hand-off

- **Pass** → done.
- **Revise and resubmit** → suggest running the original prompt again (`/ba-user-story`, `/ba-impact-assessment`, etc.) with the issue list as input.
