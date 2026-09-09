---
name: ba-acceptance-criteria
description: "Expand a Booking Portal story or requirement into focused, declarative, independently testable acceptance criteria."
user-invocable: false
---

# Skill: BA Acceptance Criteria

**When to use:** You have a user story (or raw requirement) and need focused ACs for dev/QA hand-off.

## Inputs

- A user-story file path, or a story summary, or a raw requirement.

## Procedure

1. **If given a file path, read the file.** Pull the persona, capability, and any existing ACs.

2. **Inventory the scenarios.** Brainstorm scenarios across five buckets:
   - Happy path (primary success).
   - Alternate paths (valid variations — co-traveller present, multi-leg trip, different cost centers).
   - Edge cases (boundary values, near-empty / near-max).
   - Error cases (invalid input, downstream system failure, auth failure, timeout).
   - Non-functional (only if relevant — response time, audit logging, accessibility, localization).

3. **Write 3–6 numbered declarative criteria per story.** Each criterion states the triggering condition and one directly testable expected outcome without Given/When/Then syntax. If adequate coverage needs more than six criteria, recommend splitting the requirement into multiple stories before writing ACs.

4. **Use glossary terms verbatim.** Open `.github/references/glossary.md`. Match domain terms exactly.

5. **Self-check.** For each criterion:
   - Does it have a single, testable outcome?
   - Could a dev or QA write a test from this without further questions?
   - Did you avoid implementation details (HTTP status codes, class names)?

6. **If the source is too vague, list the questions.** Don't invent. Output a "Questions before AC generation" block instead.

## Output template

```
## Acceptance Criteria — <story title>

### Happy path

1. <Direct statement of expected behaviour.>

### Alternate paths

2. <Direct statement of expected behaviour.>

### Edge cases

...

### Error cases

...

### Non-functional (if applicable)

...
```

## Stop conditions

- Stop if the story doesn't have a clear persona — ACs need a subject.
- Stop if you'd need to invent business rules to write the ACs — list questions instead.

## What this skill does NOT do

- Write executable tests (Cucumber / SpecFlow).
- Decide which behaviours are in scope for v1 vs later.
