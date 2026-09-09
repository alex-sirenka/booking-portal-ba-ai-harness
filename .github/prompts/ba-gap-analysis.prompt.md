---
agent: 'agent'
description: 'Identify blockers, missing info, risks, and assumptions for a proposed change.'
tools: [read, search, edit]
---

# /ba-gap-analysis

Analyse gaps for the proposed change: **${input:topic:proposed change description or path to impact-assessment file}**. Play the role defined in [`../agents/gap-analyst.agent.md`](../agents/gap-analyst.agent.md) and follow [`../skills/ba-gap-analysis/SKILL.md`](../skills/ba-gap-analysis/SKILL.md).

## Required input

- The proposed change description, or a path to an existing impact-assessment file under `ba-outputs/analyses/impact-assessments/`.
- A prior Confluence-scan output, including its availability status.
- Optional: prior repo-scan output for additional context.

## Required output

Save to `.github/ba-outputs/analyses/gap-analyses/<slug>.md` using [`../templates/gap-analysis.md`](../templates/gap-analysis.md). Report back in chat with:

- The saved path.
- A summary of blockers (if any) — clearly labeled.
- A short list of the most-critical missing-information items.
- Verdict: `clear to proceed` / `clear with assumptions` / `blocked`.

## Constraints

- A blocker = something the BA cannot resolve alone. Don't escalate stylistic preferences to blockers.
- Each missing-info item must say why it matters (what changes in design depending on the answer).
- Be honest about confidence. Low confidence is information; faked confidence is harm.
- Keep it under 900 words. Summarize; offer to deep-dive a section rather than expanding every section fully.
- No emojis. Plain markdown.

## Hand-off

- If verdict is **blocked**: do not proceed to `/ba-user-story` until blockers resolved. Surface to the BA.
- Otherwise: pass forward to `/ba-user-story` with the gap-analysis path.
