---
name: ba-release-notes
description: "Translate Booking Portal commits, pull requests, milestones, or date ranges into concise business-facing release notes."
user-invocable: false
---

# Skill: BA Release Notes

**When to use:** A release is being prepared and needs business-language notes for stakeholders, customers, or internal audiences.

## Inputs

- A commit range, PR list, milestone name, or date range.
- Optional: target audience — `internal`, `stakeholder`, or `customer`. Default `stakeholder`.

## Procedure

1. **Enumerate the changes.**
   - Commit range: `git log <range> --oneline --no-merges` (or use the `githubRepo` tool).
   - PR list: pull each PR title + body + the "What changed" section if present.
   - Milestone: list closed issues in the milestone.

2. **Classify each item.** Each change goes in exactly one bucket:
   - **New capability** — users can do something they couldn't.
   - **Improvement** — existing flow got better.
   - **Fix** — bug resolved.
   - **Breaking / behavior change** — anything that changes how users interact.
   - **Internal-only** — refactor, infra, dependency bump (only included if audience is `internal`).

3. **Translate to business language.** Rules:
   - "Refactored AccommodationAllocationService" → omit unless audience = internal.
   - "Added isPetFriendly to HotelBookingDto" → "Hotels can now be filtered by pet-friendly when searching."
   - "Fixed null-ref in ServiceProvider integration" → "Resolved an issue causing intermittent hotel-request processing failures for some users."

4. **Group similar items.** Five hotel fixes? "Five hotel-booking fixes (#PR list)."

5. **Tag the user role.** For new capabilities and breaking changes, name the affected role (Traveller / Approver / Admin).

6. **Lead with the headline.** One sentence at the top: what this release means for the business. Make it scannable — a stakeholder should be able to forward this and have it make sense.

7. **Save or return.** Save to `.github/ba-outputs/release-notes/<release-name>.md` if requested; otherwise return in chat. Use the template at `.github/templates/release-notes.md`.

## Output template

Use `.github/templates/release-notes.md` as the sole output structure. Populate every required metadata field and section; only `Internal-only` may be omitted for stakeholder or customer audiences.

## Stop conditions

- Stop if the commit range is empty or the milestone is open with no closed items.
- Stop if more than 40 PRs are in scope — ask the BA whether to summarize at a higher level.

## What this skill does NOT do

- Write marketing copy.
- Decide release timing.
- Generate changelog entries (that's a dev tool — `auto-changelog`, `release-please`).
