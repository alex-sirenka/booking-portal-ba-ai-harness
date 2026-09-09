---
name: ba-user-story
description: "Draft a standard Booking Portal user story with one persona, declarative acceptance criteria, dependencies, and technical notes."
user-invocable: false
---

# Skill: BA User Story

**When to use:** You have a feature, a stakeholder ask, or an impact assessment, and you need a story file in the team's standard form.

## Inputs

- The topic / feature / ask (one or two sentences is fine).
- A Confluence scan from the full pipeline, or an explicit reason it is unavailable or omitted in an ad-hoc flow.
- A gap analysis with a clear verdict, or explicit BA acknowledgment that the request is an ad-hoc story without the full upstream pipeline.
- Optional: prior repo scan or impact assessment.

## Procedure

1. **Check drafting preconditions.** If a supplied gap analysis is `blocked`, stop and return its blockers; do not create a story. If no gap analysis exists, require explicit BA acknowledgment of an ad-hoc flow and record why omitted analyses are not applicable in the story's Evidence and traceability section.

2. **Establish provenance.** The requested capability and acceptance criteria must originate from an approved requirement, an explicitly identified stakeholder request, or an approved decision resolving a confirmed gap. A confirmed gap alone does not authorize a change. Record whether approval is confirmed, pending, or not applicable.

3. **Identify the persona.** Use one of the team's standard personas:
   - **Traveller** — the employee booking travel.
   - **Co-traveller** — family member or guest on a booking.
   - **Initiator on behalf of** — creates and submits requests for another traveller.
   - **Approver** — line manager or budget owner who approves requests.
   - **Allocator** — logistics user who assigns resources.
   - **Admin** — configures settings, roles, cost centers.
   - **Dashboard reader** — reviews transportation dashboard information.
   - **Service Provider (SP)** — hotel chain, airline, vendor — usually a system actor, not a user.
   If the topic touches multiple personas, produce one story per persona.

4. **Write the As-a / I-want / So-that.**
   - As-a: the persona above.
   - I-want: the capability in plain language. No implementation.
   - So-that: the business outcome.

5. **Acceptance criteria.** Write 3–6 plain declarative, independently testable statements covering the happy path and the most important alternate and error behaviour. Never use Given/When/Then. If more than 6 are needed, split the story. Each criterion must be supported by the identified source basis; do not invent behaviour to fill a coverage category.

6. **Out of scope.** List what this story does NOT cover. This is often the most useful section.

7. **Evidence and traceability.** Identify the source ask, Confluence scan, approval or decision source when one exists, and the repo scan, impact assessment, and gap analysis. Identify Confluence sources by page key and title without URLs. Preserve pending approval, unavailable evidence, conflicts, and unresolved assumptions explicitly.

8. **Technical notes.** A separate, fenced section at the end. Architecture pointers, affected modules from the impact assessment. The story body must remain implementation-free.

9. **Save it.** File path: `.github/ba-outputs/user-stories/<short-kebab-slug>.md`. Use the template at `.github/templates/user-story.md`.

10. **Report back.** In chat: the saved path, one-paragraph summary, up to three questions for the PO.

## Stop conditions

- Stop if the topic is too vague to identify a persona — ask the BA which role.
- Stop if there's no clear business outcome — ask "so-that what?".
- Stop if the requested capability or an acceptance criterion has no identifiable source. Ask for clarification rather than inventing it.

## What this skill does NOT do

- Generate exhaustive acceptance-criteria coverage — that's `/ba-acceptance-criteria`.
- Decide priority or sequencing.
- Write technical specs.
