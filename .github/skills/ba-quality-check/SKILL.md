---
name: ba-quality-check
description: "Review Booking Portal BA artifacts for structure, terminology, traceability, permissions, completeness, and cross-artifact consistency."
user-invocable: false
---

# Skill: BA Quality Check

**When to use:** Right after any BA artifact is drafted, before sharing it with stakeholders or devs.

## Inputs

- A path to the artifact (story, impact, gap, scan, release notes, BRD).
- Optional: paths to upstream phase outputs (for traceability check).

## Procedure

Detect the artifact type from filename / location, then run the matching checklist:

### Universal checks (every artifact)

1. **Template adherence** — every required section from the matching `.github/templates/<type>.md` is present and non-empty. Sections marked optional or conditional may be omitted or state `Not applicable`.
2. **Glossary compliance** — every domain term matches `.github/references/glossary.md` verbatim. Flag paraphrases.
3. **No emojis. Plain markdown. No marketing fluff.**
4. **Change-log entry present** with date and author.
5. **Source basis identified** — stories and analyses reference the source ask; release notes identify the commit/PR range or milestone; BRDs identify sponsor and business context.
6. **Evidence support** — every material project-specific finding identifies a source, and the cited evidence supports the conclusion. Reject unsupported findings.
7. **Classification integrity** — Current Implementation, Documented Behaviour, Requested Target, Approved Requirement, Assumption, Conflict, Gap, and Recommendation are not presented as one another.
8. **Assumptions marked** — inferred or uncertain conclusions are explicit and include a validation route.
9. **Conflicts preserved** — contradictions between documentation, stakeholder asks, and implementation remain visible until an authoritative decision resolves them.
10. **Recommendations separated** — recommendations are not written as approved requirements.
11. **Confluence evidence** — feature/change pipelines include a genuinely completed Confluence scan; an `Unavailable` scan is a pipeline blocker, not an acceptable input to downstream analysis. If downstream artifacts (repo scan, impact assessment, gap analysis, story) exist alongside an `Unavailable` Confluence scan, flag this as a process violation — the pipeline should have stopped before producing them. Sources use page key/title without URLs; search scope, retrieval date, freshness, and authority are visible. Confluence evidence is not used to prove Current Implementation.

### User-story checks

12. **Persona** — one per file. Drawn from `.github/references/personas.md`. If multi-persona, recommend split.
13. **As-a / I-want / So-that** — all three present and non-vacuous. "So-that" must name a business outcome, not restate the capability.
14. **Acceptance criteria**:
   - 3–6 criteria total.
   - Plain declarative statements; no Given/When/Then.
   - One directly testable outcome per criterion.
   - Covers happy + alternate + edge + error behaviour where relevant.
15. **Permissions** — required `Operation` enum values listed (matching the personas guidance).
16. **Implementation-free body** — no class names, no HTTP status codes, no DB column names outside of Technical Notes.
17. **Open questions** — labeled section, even if empty (write "None at this time").
18. **Story traceability** — orchestrated stories cite the source ask, Confluence scan, approval or decision source when one exists, repo scan, impact assessment, and gap analysis. Ad-hoc stories cite the source ask and available analyses, with an explicit reason for each omitted analysis.
19. **Story provenance** — the requested capability and acceptance criteria originate from an approved requirement, an explicitly identified stakeholder request, or an approved decision resolving a confirmed gap.

### Impact-assessment checks

20. **Service classifications** — each affected service classified as code / schema / contract / config / no change.
21. **External systems** — WaaS, Azure AD, Service Provider Portal, and iLogistics explicitly addressed (yes / no / not affected). Don't allow these to be silently omitted.
22. **Integration events** — affected events listed with subscribers.
23. **Workflow impact** — explicit yes/no on state machine, mapping, and WaaS template changes.

### Gap-analysis checks

24. **Comparison** — documented behaviour, current implementation, and requested or approved target are compared where evidence exists; unavailable dimensions are marked explicitly.
25. **Gap classification** — discrepancies are not labeled as defects unless an approved requirement establishes expected behaviour.
26. **Verdict** — one of `clear to proceed` / `clear with assumptions` / `blocked`.
27. **Blockers** — each has an owner.
28. **Missing-info items** — each says why it matters.
29. **Assumptions** — each says how to validate.

### Repo-scan checks

30. **Citations** — every claim has a file path or, for Confluence evidence, a page key/title plus the Confluence-scan path.
31. **Current-state boundary** — observations are not presented as intended or approved business behaviour.
32. **Workflow involvement** — explicit yes/no.
33. **Configuration surface** — non-empty if any feature flags / settings are involved.
34. **Reference maps consulted** — scan shows evidence of checking `service-map.md` / `frontend-map.md` / `integration-map.md` (and `workflows.md` when approvals are involved) before raw search; any discrepancy is listed under "Reference corrections", not silently resolved.

### Confluence-scan checks

35. **Search scope** — search terms, spaces or page families, and scope limitations are recorded.
36. **Source identity** — every used source has a page key and title; no Confluence URL is included.
37. **Freshness and authority** — search date, page last-modified date when available, and approval authority/status are explicit.
38. **Evidence boundary** — findings are classified as Documented Behaviour, Approved Requirement, Historical Context, Conflict, or Evidence Gap; none are presented as Current Implementation.
39. **Unavailable behavior** — an unavailable scan records attempted scope and reason and is always treated as a blocker requiring the BA to fix access, regardless of whether a specific page was flagged as authoritative.

### Release-notes checks

40. **Headline present** at top.
41. **Grouping** — capabilities / improvements / fixes / breaking — items aren't mixed.
42. **PR / commit references** — every item has one.
43. **Persona tags** on new capabilities and breaking changes.

## Output

A reviewer's note:

- Detected artifact type.
- Issues found (location + issue + suggested fix), grouped by severity.
- Verdict: `pass` / `pass with minor fixes` / `revise and resubmit`.

## Stop conditions

- File doesn't exist. Surface.
- Artifact type can't be detected (wrong filename, missing template structure). Surface.

## What this skill does NOT do

- Rewrite the artifact. Flag, don't fix.
- Approve the substantive correctness of business decisions. Verify whether claims are supported and classified, not whether stakeholders made the right decision.
