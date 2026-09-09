---
agent: 'agent'
description: 'Assess what would change — code, data, integrations, user flows — for a proposed feature or modification.'
tools: [read, search, edit]
---

# /ba-impact-assessment

Assess the impact of the proposed change: **${input:change:describe the proposed change}**. Follow the playbook at [skills/ba-impact-assessment/SKILL.md](../skills/ba-impact-assessment/SKILL.md).

## Required input

- A description of the proposed change (passed as `${input:change}`). The more concrete, the better — "add an optional pet-friendly flag to hotel bookings" is better than "improve hotel bookings".
- A prior `/ba-confluence-scan` report, including an `Unavailable` report when access failed.
- Optional: a prior `/ba-repo-scan` report. If provided, build on it instead of re-scanning from scratch.

## Required output

Save the report to `.github/ba-outputs/analyses/impact-assessments/<short-kebab-slug>.md`. Use these sections:

Use every section and metadata field from [`../templates/impact-assessment.md`](../templates/impact-assessment.md), including source metadata, Integration events, Workflow impact, Cross-cutting, Suggested next BA artifact, and Change log. Populate `Not applicable` when a required section has no findings.

1. **Change in one sentence.** Restate what's being proposed in BA language.
2. **Affected modules.** Backend projects and frontend modules that would change. Cite paths. Mark each as `code change`, `schema change`, `contract change`, or `config change`.
3. **Integration impact.** Separate internal services, messaging infrastructure, and external systems. Explicitly address iLogistics, WaaS, Azure AD, and Service Provider Portal; for each applicable boundary, note the type of impact and coordination required.
4. **Data impact.** Schema changes, migration needs, backfill considerations.
5. **User-flow impact.** Which screens / approval steps / notifications change. Be specific about user roles affected (Traveller, Approver, Admin).
6. **Risks and unknowns.** Things that could go wrong; assumptions baked into the assessment. List explicitly.
7. **Suggested next BA artifact.** Usually a user story (run `/ba-user-story`) or a clarifying question to the product owner.

## Constraints

- Be conservative. If you're not certain a module is affected, say "likely affected — verify" rather than asserting.
- Cite files for every concrete claim.
- Keep it under 1000 words. Use bullets in the affected-modules / integration-impact sections; prose elsewhere.
- Do not estimate effort in hours or story points — that's the team's job.

## Hand-off

Output usually feeds `/ba-user-story` (one story per affected user flow) or back to stakeholders for refinement.
