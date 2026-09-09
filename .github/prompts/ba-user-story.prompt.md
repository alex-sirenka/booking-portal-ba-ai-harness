---
agent: 'agent'
description: 'Draft a user story (As a / I want / So that) with acceptance criteria for a Booking Portal change.'
tools: [read, search, edit]
---

# /ba-user-story

Draft a user story for: **${input:topic:the feature, change, or stakeholder ask}**. Follow the playbook at [skills/ba-user-story/SKILL.md](../skills/ba-user-story/SKILL.md). Use the template at [`../templates/user-story.md`](../templates/user-story.md).

## Required input

- Topic / feature / stakeholder ask (`${input:topic}`).
- A Confluence scan, or explicit BA acknowledgment and traceability note when it is unavailable or omitted in an ad-hoc flow.
- A gap analysis with verdict `clear to proceed` or `clear with assumptions`, or explicit BA acknowledgment that this is an ad-hoc story without full upstream analysis.
- Optional: a prior `/ba-repo-scan` or `/ba-impact-assessment` report. Use it.

## Required output

Save the story to `.github/ba-outputs/user-stories/<short-kebab-slug>.md`, following the template. Report back in chat with:

1. The path you saved to.
2. A one-paragraph summary of the story.
3. Up to three questions for the product owner if anything was underspecified.

## Constraints

- One persona per story. If the topic spans multiple roles (Traveller, Approver, Admin), produce one story file per role and list them all.
- Apply the preconditions from [`../agents/story-writer.agent.md`](../agents/story-writer.agent.md). Do not draft when a supplied gap analysis is `blocked`; return its blockers instead. For an ad-hoc story, record the BA acknowledgment and explain omitted upstream artifacts in Traceability.
- Acceptance criteria are plain declarative statements. Aim for 3–6 per story; split if more.
- No technical implementation in the story body. Architecture notes go in a separate "Technical Notes" section at the end, clearly fenced.
- Pull domain terms from `.github/references/glossary.md` verbatim. Don't paraphrase.
- Use Markdown. No emojis.

## Hand-off

After saving, suggest running `/ba-acceptance-criteria` if the user wants more exhaustive coverage, or open the saved file for review.
