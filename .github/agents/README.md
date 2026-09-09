# BA Agent Orchestration

This folder defines discoverable **custom agents** for BA orchestration. The Orchestrator invokes the phase agents as isolated subagents, validates each hand-off, and keeps the user-facing pipeline state in the parent session.

Each role is a markdown file (`<name>.agent.md`) with: responsibility, inputs, procedure (linked to a skill playbook under `../skills/`), outputs, hand-off contract, stop conditions.

## The roster

| Agent | Role | File |
|---|---|---|
| Orchestrator | Router. Reads the raw ask, picks the pipeline, sequences agents, narrates hand-offs. | `orchestrator.agent.md` |
| Intake | Classifies the ask, identifies the persona, restates intent in BA language. | `intake.agent.md` |
| Confluence-Scout | Searches ADNOC Confluence for documented behaviour, decisions, ownership, and constraints. | `confluence-scout.agent.md` |
| Repo-Scout | Finds where the feature lives today (modules, flow, integrations). | `repo-scout.agent.md` |
| Impact-Assessor | Identifies what would change for a proposed modification. | `impact-assessor.agent.md` |
| Gap-Analyst | Identifies blockers, missing info, risks, assumptions. | `gap-analyst.agent.md` |
| Story-Writer | Drafts user stories with acceptance criteria. | `story-writer.agent.md` |
| Quality-Checker | Reviews outputs against glossary, template, completeness. | `quality-checker.agent.md` |
| Release-Historian | Compiles release notes from a commit/PR range. | `release-historian.agent.md` |

## Pipelines

### Feature / change pipeline

```
Raw ask (in `.github/ba-outputs/inbox/` or pasted in chat)
   │
   ▼
Intake ──▶ Confluence-Scout ──▶ Repo-Scout ──▶ Impact-Assessor ──▶ Gap-Analyst ──▶ Story-Writer ──▶ Quality-Checker
                           │                                                                 │
                           ▼                                                                 ▼
   .github/ba-outputs/analyses/confluence-scans/<slug>.md        .github/ba-outputs/user-stories/<slug>.md
```

The Orchestrator runs the pipeline phase-by-phase. After each phase it summarizes ("phase X complete — here's what I found"), then continues. If a phase hits a stop condition, the Orchestrator surfaces the blockers and asks the BA — it does not proceed on assumption.

### Release pipeline

```
Range (commits / PRs / milestone)
   │
   ▼
Release-Historian
   │
   ▼
.github/ba-outputs/release-notes/<release>.md
```

### Ad-hoc Q&A

Not an orchestrated pipeline — use the `BA Product Expert` chat mode directly.

## How to invoke

- **Full pipeline:** select the `orchestrator` agent or switch to the legacy `BA Orchestrator` chat mode, then paste the ask or reference a file in `.github/ba-outputs/inbox/`.
- **Single phase (ad-hoc):** invoke the matching prompt: `/ba-intake`, `/ba-confluence-scan`, `/ba-repo-scan`, `/ba-impact-assessment`, `/ba-gap-analysis`, `/ba-user-story`, `/ba-acceptance-criteria`, `/ba-quality-check`, `/ba-release-notes`.
- **Q&A:** switch to the `BA Product Expert` chat mode.

## Hand-off contract (shared by all agents)

Each agent's output must include:

1. **What I did** — one sentence summary of the phase.
2. **What I found** — the substantive output (report, list, draft).
3. **What I'm passing forward** — explicit list of artifacts (paths, key terms, follow-up questions) the next agent will need.
4. **What stopped me, if anything** — gaps, ambiguities, missing inputs. If non-empty, the Orchestrator must escalate to the BA.

## Extending

To add a new agent:

1. Write `agents/<name>.agent.md` using the existing files as a pattern.
2. If it needs a procedure, write `skills/<name>/SKILL.md` with the steps.
3. Optionally add a prompt at `prompts/<name>.prompt.md` so the agent can be invoked ad-hoc.
4. Update the orchestrator's roster and the pipelines in this README.
