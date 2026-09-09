# Gap Analysis — <Topic / proposed change>

**Status:** Draft  
**Source ask:** <inbox link>  
**Inputs used:** <Confluence scan path and status>, <repo scan path>, <impact assessment path>  
**BA initials / date:** <XX> / <YYYY-MM-DD>

## Purpose

What is the proposed change, and what could prevent it from being delivered cleanly? This document lists **blockers, missing information, risks, and assumptions** — explicitly.

## Evidence comparison

Use `Not found` when a dimension has no available evidence. A discrepancy is not automatically a defect; identify an approved requirement before classifying one as a defect.

| Topic | Documented behaviour | Current implementation | Requested / approved target | Classification | Evidence |
|---|---|---|---|---|---|
| <Material topic> | <documentation or Not found> | <code/config/tests or Not found> | <request/decision or Not found> | Conflict / Gap / Assumption / No gap | <source paths or artifact references> |

## Blockers (must resolve before estimating)

- **<Blocker>** — Description. Owner. Estimated resolution time. What it blocks.

## Missing information (the PO / stakeholder must answer)

- **<Topic>** — Specific question. Why we need it (what changes in the design depending on the answer). Default-if-unanswered (if any).

## Cross-team dependencies

For each external system touched (`references/integration-map.md`), note coordination required:

- **<System>** — Who owns it. What we need from them. SLA / timeline.

## Risks (could cause delivery to slip)

| Risk | Likelihood (L/M/H) | Impact (L/M/H) | Mitigation |
|---|---|---|---|

## Assumptions we're making

If these are wrong, the story is wrong. Validate explicitly:

- **<Assumption>** — Where it comes from. How to validate.

## Permissions / operations not yet defined

- Are new operations needed beyond the current `Operation.cs` enum? If yes, list them with proposed names.

## Workflow / WaaS implications

- Does this need a new WaaS template? Who configures it?
- Does the state machine in `BookingRequestWorkflow` need new transitions or exit paths?

## Glossary candidates

New terms that came up but aren't yet in `references/glossary.md`. List with proposed definitions for BA confirmation.

## Confidence summary

- High confidence in (we know): ...
- Medium confidence in (we have evidence, need to confirm): ... Evidence needed to raise confidence: ...
- Low confidence in (open questions, assumptions): ... Evidence needed to raise confidence: ...

## Verdict

`clear to proceed` / `clear with assumptions` / `blocked` — <brief rationale>.

## Recommended next steps

1. ...
2. ...

## Change log

- YYYY-MM-DD — Created by <BA>.
