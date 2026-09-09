---
agent: 'agent'
description: 'Summarize what changed in a release in business language, from a commit/PR range or milestone.'
tools: [read, search, edit, execute, web]
---

# /ba-release-notes

Produce release notes covering: **${input:range:commit range, PR list, milestone, or date range}**. Follow the playbook at [skills/ba-release-notes/SKILL.md](../skills/ba-release-notes/SKILL.md).

## Required input

- A commit range (`v1.42..v1.43`), a list of PR numbers, a milestone name, or a date range (`${input:range}`).
- Optional: target audience — `internal` (devs + BAs), `stakeholder` (business owners), or `customer` (end users). Default: `stakeholder`.

## Required output

Use every applicable section and metadata field from [`../templates/release-notes.md`](../templates/release-notes.md), including Range, Configuration changes, Known issues, and Change log. Only `Internal-only` may be omitted for stakeholder/customer audiences; use `Not applicable` for other empty sections.

Markdown release notes with these sections:

1. **Headline.** One sentence on what this release means for the business.
2. **New capabilities.** What users can do now that they couldn't before. Group by user role (Traveller / Approver / Admin).
3. **Improvements.** Existing flows that got better — faster, clearer, more reliable.
4. **Fixes.** Bugs resolved. Use plain language, not stack traces.
5. **Breaking / behavior changes.** Anything that changes how users interact. Call this out prominently if present.
6. **Internal-only** (only if audience = `internal`). Refactors, infra moves, dependency bumps.

## Constraints

- Business language. Translate code-speak: "Refactored AccommodationAllocationService" → don't include unless audience is `internal`.
- Each item: one sentence of what + one phrase of why-it-matters.
- Cite the PR number for each item.
- Group, don't dump. If there are five hotel fixes, summarize: "Five hotel-booking fixes (#PR list)".
- No emojis. No marketing language.

## Hand-off

Save to `.github/ba-outputs/release-notes/<release-name>.md` if the user asks; otherwise return in chat. Use the template at `.github/templates/release-notes.md`.
