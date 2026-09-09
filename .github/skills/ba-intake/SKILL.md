---
name: ba-intake
description: "Classify and restate a raw Booking Portal stakeholder ask before repository analysis. Use for the intake phase of the BA pipeline."
user-invocable: false
---

# Skill: BA Intake

**When to use:** A new ask lands. You need to classify it before doing real work.

## Inputs

- A raw ask: text, file path under `ba-outputs/inbox/`, or a link.

## Procedure

1. **Read fully.** If it's a file, read every line. If it's a link, ask the BA to summarize (since you can't fetch arbitrary links).

2. **Classify into one bucket:**
   - **Feature** — new capability for users.
   - **Change** — modification of existing capability.
   - **Bug-driven change** — a defect surfaces that requires BA work (not just a code fix).
   - **Question** — pure information request, no artifact needed.
   - **Release** — summarize what shipped.
   - **Ambiguous** — stop, ask one question.

3. **Identify persona(s)** from `.github/references/personas.md`. Note when multiple personas are likely affected.

4. **Restate intent** in one sentence:
   - Use glossary terms verbatim from `.github/references/glossary.md`.
   - Lead with the persona ("Travellers should be able to ...").
   - No implementation hints.

5. **Identify affected booking domain(s)** — Accommodation / Cars / Cars-With-Drivers / Flights / Hotels / ShuttleBuses / Multi-leg / Admin / AI / Cross-cutting. This tells Repo-Scout which sub-services to look in first.

6. **Extract direct hints** from the ask — file names, feature names, error messages, screen names. These are search anchors for Repo-Scout.

7. **Recommend next step:**
   - Most asks → `/ba-repo-scan <restated intent>`.
   - Pure Q&A → "switch to `BA Product Expert` chat mode".
   - Release ask → `/ba-release-notes <range>`.
   - Ambiguous → ask one clarifying question, stop.

## Output

A short markdown block (using `.github/templates/repo-scan.md` would be wrong here; Intake has no artifact template):

```
## Intake — <one-line title>

**Original ask:** <quoted text or file path>

**Classification:** feature | change | bug-driven | Q&A | release | ambiguous

**Restated intent:** <one sentence>

**Likely persona(s):** <list>

**Affected booking domain(s):** <list>

**Hints from the ask:** <list>

**Recommended next step:** <command or guidance>
```

## Stop conditions

- Ask is genuinely ambiguous — ask one focused question and stop.
- Ask spans multiple unrelated features — recommend splitting before continuing.
- Ask is a Q&A — route to chat mode, don't run the pipeline.

## What this skill does NOT do

- Look at code (that's Repo-Scout).
- Draft anything.
- Decide priority.
