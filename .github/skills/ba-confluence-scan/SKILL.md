---
name: ba-confluence-scan
description: "Search ADNOC Confluence for documented Booking Portal behaviour, decisions, ownership, constraints, and integration details before repository analysis."
user-invocable: false
---

# Skill: BA Confluence Scan

**When to use:** After Intake classifies a Booking Portal feature or change and before Repo-Scout examines the implementation.

## Inputs

- Original ask or inbox artifact.
- Intake restatement, personas, affected domains, and search hints.
- Any Confluence page keys/titles Intake explicitly flagged as authoritative — these govern the `blocked` stop condition below; other pages found during search never do.

## Procedure

1. **Prepare focused search terms.** Use the feature name, glossary terms, affected domains, screen or service names, external integrations, and aliases from the ask. Record the terms used.

2. **Search ADNOC Confluence.** Start with exact phrases and named systems, then broaden once if needed. Prefer pages that appear to contain requirements, decisions, workflows, integration contracts, support procedures, or ownership information.

3. **Read relevant pages.** For each page used, capture:
   - page key and title;
   - relevant section or heading;
   - last-modified date when available;
   - concise evidence summary;
   - named owner, approver, status, or decision date when explicit.

4. **Classify evidence.** Use only these classifications:
   - **Documented Behaviour** — describes intended or operating behaviour but does not prove the deployed implementation.
   - **Approved Requirement** — explicitly records approval and identifies an authoritative approver, decision body, or approved status.
   - **Historical Context** — explains prior behavior or rationale but is not current authority.
   - **Conflict** — contradicts another Confluence source or the stakeholder request. Repo-Scout may later add code conflicts.

5. **Assess freshness and authority.** Flag stale, undated, draft, archived, ownerless, or superseded pages. Never infer approval from publication alone.

6. **Record negative evidence carefully.** List searches that produced no relevant result. Say `Not found in searched Confluence scope`, not `Confluence has no documentation`.

7. **Handle access failure.** If authentication or page access fails, save an `Unavailable` scan with the failed scope and reason. This is a **blocker** — report it so the Orchestrator stops the pipeline before Repo-Scout, regardless of whether the ask named a specific page as authoritative.

8. **Save the artifact.** Use `.github/templates/confluence-scan.md` and save to `.github/ba-outputs/analyses/confluence-scans/<slug>.md`. Keep it under 500 words; summarize and offer to deep-dive a section rather than reproducing page content.

## Evidence rules

- Identify pages by key and title; do not output Confluence URLs.
- Quote only the minimum text required to preserve meaning. Prefer concise summaries.
- Keep documentation evidence separate from source-code evidence.
- Do not expose page content unrelated to the Booking Portal request.
- Do not claim that documented behaviour is implemented until Repo-Scout verifies it in code, configuration, or tests.

## Stop conditions

- A page Intake flagged as explicitly authoritative cannot be retrieved: return `blocked` and identify the page key/title and access problem.
- Confluence is unavailable for any reason — tool not registered, authentication failure, network/server error: save an `Unavailable` artifact and return `blocked`. This stops the pipeline before Repo-Scout; it is not a result to silently continue past.
- Search results remain ambiguous after one broader search: record likely candidates and continue without asserting their content.

## What this skill does NOT do

- Determine current implementation.
- Resolve contradictions or approve requirements.
- Edit Confluence pages.
- Perform an unbounded knowledge-base audit.