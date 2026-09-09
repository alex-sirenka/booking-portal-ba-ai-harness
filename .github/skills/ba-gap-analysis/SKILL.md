---
name: ba-gap-analysis
description: "Identify blockers, missing information, assumptions, risks, and ownership gaps before drafting a Booking Portal story."
user-invocable: false
---

# Skill: BA Gap Analysis

**When to use:** After Confluence-Scout, Repo-Scout, and Impact-Assessor have run, before drafting stories. The point is to surface what's missing or risky so we don't draft on top of bad assumptions.

## Inputs

- Repo-scan output.
- Confluence-scan output, including an `Unavailable` result when access failed.
- Impact-assessment output.
- Original ask.

## Procedure

1. **Build the evidence comparison.** For each material topic, use the Confluence scan for Documented Behaviour or explicitly authoritative Approved Requirements and the repo scan for Current Implementation. Cite the source for each populated value, preserve contradictions as Conflict, and mark unavailable or stale documentation explicitly.

2. **Classify discrepancies.** A gap is a discrepancy, missing decision, or missing capability; it is not automatically a defect. Call it a defect only when an approved requirement establishes the expected behaviour.

3. **Re-read the impact-assessment.** For every claim, ask: is this supported, or an assumption? Flag assumptions explicitly.

4. **Identify blockers** — must resolve before the change can be designed:
   - Cross-team dependency without an identified owner.
   - Schema or contract change to an external system (Azure AD claims, Service Provider Portal event, iLogistics payload, WaaS template) without a coordination plan.
   - A new Authorization operation needed with no UI for managing it.
   - Workflow change requiring a new booking-request type with no WaaS template to map to.
   - Required upstream data not exposed by any service (e.g., a field we'd need from the People service that doesn't exist).

5. **Identify missing information** — questions only the PO can answer:
   - Business rules not visible in code (thresholds, escalation, OOO).
   - Volume / scale assumptions.
   - Priority and sequencing within the broader roadmap.
   - Audit / compliance requirements.
   For each: what's the question, why it matters (what changes in design), what's the default-if-unanswered.

6. **List cross-team dependencies** from the impact-assessment's external systems table, plus any internal service whose owner isn't on the BA's team. Owner, what we need, by when.

7. **Risks** — likelihood × impact, with mitigation.

8. **Assumptions** — every assumption baked into impact-assessment. State explicitly + how to validate.

9. **Permissions / operations gap** — does the impact require a new operation beyond the current `Operation.cs` enum? If yes, propose a name and which services would enforce it.

10. **Workflow / WaaS gap** — new template needed? New state-machine transition? Confirm owner.

11. **Glossary candidates** — terms encountered that aren't in `.github/references/glossary.md`. Propose definitions.

12. **Confidence summary** — high / medium / low confidence buckets for synthesized conclusions. For Medium and Low, state what evidence would raise confidence.

## Output

Use `.github/templates/gap-analysis.md`. Save to `.github/ba-outputs/analyses/gap-analyses/<slug>.md`.

## Verdict

Set one of:

- `clear to proceed` — no blockers, assumptions are reasonable.
- `clear with assumptions` — no blockers but several assumptions need PO confirmation; story can be drafted with caveats.
- `blocked` — at least one blocker. Story drafting is paused.

## Stop conditions

- Impact-assessment is empty or contradicts repo-scan. Loop back to Impact-Assessor.
- Original ask is too vague to identify gaps meaningfully. Surface as the primary blocker.
- Stop if the analysis would exceed 900 words. Summarize; offer to deep-dive a section (e.g., risks, or missing information) rather than expanding every section fully.

Before returning, self-check this output against the gap-analysis checklist items in `.github/skills/ba-quality-check/SKILL.md` (evidence comparison, gap classification, verdict, blockers with owners, missing-info items with rationale, assumptions with validation routes). The Orchestrator relies on this self-check and will not invoke Quality-Checker separately after this phase by default.

## What this skill does NOT do

- Resolve blockers (BA + PO).
- Decide priority or sequence.
- Draft stories.
