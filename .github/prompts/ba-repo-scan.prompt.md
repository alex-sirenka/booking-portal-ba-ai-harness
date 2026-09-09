---
agent: 'agent'
description: 'Surface how a feature is implemented today — modules touched, integrations, data flow, gaps.'
tools: [read, search, edit]
---

# /ba-repo-scan

Scan the repo for how the **${input:feature:feature name or keyword}** feature is implemented today. Follow the playbook at [skills/ba-repo-scan/SKILL.md](../skills/ba-repo-scan/SKILL.md) step by step.

## Required input

- Feature name or keyword (passed as `${input:feature}`).
- Confluence scan path. For an orchestrated flow this is required, including an `Unavailable` scan; for an ad-hoc scan, run `/ba-confluence-scan` first or explicitly record why it was omitted.
- Optional extra context the user provides (a Jira link, a stakeholder note, a PR number). Treat any extra text in the chat as additional context.

## Required output

Save the report to `.github/ba-outputs/analyses/repo-scans/<short-kebab-slug>.md`. Produce it with these sections, in this order:

Use every section and metadata field from [`../templates/repo-scan.md`](../templates/repo-scan.md), including source metadata, Workflow involvement, Permissions / operations enforced, Configuration surface, Open questions, and Change log. Populate `Not applicable` when a required section has no findings.

1. **Feature in one sentence.** Plain business language.
2. **Backend modules touched.** List each project under `booking-backend/src/Services/` involved, with a one-line role. Cite paths.
3. **Frontend modules touched.** Same for `booking-client/src/modules/`. Cite paths.
4. **Data flow.** Walk the path from user action to persistence and back, mentioning gRPC / REST / Service Bus boundaries when crossed.
5. **Dependencies and integrations.** Separate internal services (including Logistics, People, Settings, and FileAttachments), messaging infrastructure, and external systems (including iLogistics, WaaS, Service Provider Portal, and Azure AD). Include only the boundaries this feature actually uses.
6. **Domain glossary.** Any new term you encountered that isn't yet in `.github/references/glossary.md`. List them with a one-line definition for the BA to confirm.
7. **Open questions.** What you couldn't answer from the repo alone. Suggest where the answer lives.

## Constraints

- Cite a file path for every non-obvious claim. The BA will check.
- Do **not** invent endpoints, env vars, or class names. If you can't find it, say so in "Open questions".
- Keep the report under 800 words. The BA can ask for depth on any section.
- Default to business framing. Surface technical detail only when the BA needs it to make a decision.

## Hand-off

This report typically feeds into `/ba-impact-assessment` (for a proposed change) or `/ba-user-story` (for a new piece of work).
