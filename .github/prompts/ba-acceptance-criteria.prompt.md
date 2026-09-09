---
agent: 'agent'
description: 'Expand a user story or requirement into focused, declarative acceptance criteria.'
tools: [read, search, edit]
---

# /ba-acceptance-criteria

Generate acceptance criteria for: **${input:story:user story path, story summary, or raw requirement}**. Follow the playbook at [skills/ba-acceptance-criteria/SKILL.md](../skills/ba-acceptance-criteria/SKILL.md).

## Required input

- A user story file path, a story summary, or a raw requirement (`${input:story}`).
- If a file path: read the file first.

## Required output

A markdown block of numbered, declarative acceptance criteria. Cover:

1. **Happy path** — the primary success case.
2. **Alternate paths** — valid variations (e.g., a co-traveller is added, a flight is changed mid-booking).
3. **Edge cases** — boundary inputs, near-empty / near-max values.
4. **Error cases** — invalid inputs, downstream system failures, auth failures.
5. **Non-functional** — only if relevant (response time, audit logging, accessibility, localization).

## Constraints

- Each criterion states one directly testable expected behaviour. Do not use Given/When/Then or other Gherkin syntax.
- Produce 3–6 criteria per story. If adequate coverage needs more than six, recommend how to split the story instead of extending its AC section.
- Use domain terms from `.github/references/glossary.md` verbatim.
- If the source story is too vague to produce concrete ACs, list the questions you need answered first instead of inventing.
- No emojis. Plain markdown.

## Hand-off

If the source was a user-story file, offer to replace or extend that file's "Acceptance Criteria" section.
