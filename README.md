# Booking Portal BA AI Harness

Reusable GitHub Copilot custom-agent orchestration for a Business Analyst pipeline:

```
Intake -> Confluence-Scout -> Repo-Scout -> Impact-Assessor -> Gap-Analyst -> Story-Writer -> Quality-Checker
```

## What's included

- `.github/agents/` - custom agent role definitions (the `orchestrator` plus each phase agent)
- `.github/skills/` - step-by-step playbooks referenced by the agents
- `.github/prompts/` - `/ba-*` slash-command entry points for single-phase, ad-hoc use
- `.github/chatmodes/` - legacy chat-mode definitions for the same pipeline
- `.github/templates/` - output templates for each BA artifact (repo scan, impact assessment, gap analysis, Confluence scan, user story, release notes, BRD)
- `.github/copilot-instructions.md` - project-level grounding instructions (adapt to your own project)

## Not included (bring your own)

- `.github/references/` - your project's service map, frontend map, integration map, personas, and glossary. The agents read these for grounding; without them they'll say so and ask.
- `.github/ba-outputs/` - generated artifacts (inbox asks, analyses, stories, decisions, release notes) are project-specific outputs, not part of the harness.
- `.github/instructions/*.instructions.md` - path-scoped conventions; add your own for your codebase's folder structure.

## Setup

1. Copy the `.github/` folder from this repo into your own repository.
2. Rewrite `.github/copilot-instructions.md` for your project (persona, source-of-truth rules, output paths).
3. Add your own `.github/references/*.md` files that the agents expect (see references throughout `.github/agents/` and `.github/skills/`).
4. If you want the Confluence-Scout phase to do real searches, configure an Atlassian MCP server and confirm its tool-grant name matches what `.github/agents/confluence-scout.agent.md` expects (`mcp-atlassian/*`); otherwise it degrades gracefully to a documented `Unavailable` result.
5. Invoke the `orchestrator` custom agent with a raw stakeholder ask, or use an individual `/ba-*` slash prompt for single-phase work.

## Design notes

- Every phase agent returns a four-part hand-off contract: what I did, what I found, what I'm passing forward, what stopped me.
- Evidence is classified as Current Implementation, Documented Behaviour, Requested Target, Approved Requirement, Assumption, Conflict, Gap, or Recommendation - never presented as one another.
- Quality-Checker runs once per pipeline run across the full artifact set rather than after every phase, to keep cost bounded; each phase agent self-checks its own output first.
- Analysis artifacts are versioned (`-v2`, `-v3`) only for genuine re-runs (pipeline change, scope change, or a status transition like `Unavailable` to completed) - incremental additions to a still-`Draft` artifact are edited in place.
