---
name: ba-repo-scan
description: "Trace the current Booking Portal implementation of a capability across code, workflows, permissions, events, and integrations. Use during repository analysis."
user-invocable: false
---

# Skill: BA Repo Scan

**When to use:** A stakeholder or PO asks "how does feature X work today?" or "where in the code is feature X?". You need a grounded map before drafting stories or assessing impact.

**Owner of this skill:** the BA. Updated when the team agrees on a better procedure.

## Inputs

- A feature name, keyword, or stakeholder phrase.
- A prior Confluence scan from `.github/ba-outputs/analyses/confluence-scans/`. In the full pipeline this is required, including an `Unavailable` scan.
- Optionally: a Jira link, a related PR, a prior BA artifact.

## Procedure

1. **Read the Confluence scan.** Use documented terms, page titles, integrations, owners, and constraints as focused search anchors. Treat them as Documented Behaviour, Approved Requirement only when explicit authority is recorded, Historical Context, Conflict, or Evidence Gap. Never use them as proof of Current Implementation.

2. **Anchor on the glossary.** Open `.github/references/glossary.md`. Note any term in the feature description that has a definition. Use the canonical spelling in your output.

   Treat source code, configuration, and tests as evidence of Current Implementation only. Do not infer intended or approved requirements. If business intent is unknown, preserve that as an open question for a downstream phase.

3. **Check the curated reference maps before searching raw code.** Open `.github/references/service-map.md`, `.github/references/frontend-map.md`, and `.github/references/integration-map.md`; open `.github/references/workflows.md` too if the feature touches approvals. Use them to identify which backend service(s), frontend module(s), and integration boundaries are likely involved. Treat the maps as a starting index, not ground truth: if code contradicts a map or the Confluence scan, keep searching and record the discrepancy — don't silently choose a source or edit it yourself.

4. **Locate the backend entry points.** Using the service(s) identified in step 3, search `booking-backend/src/Services/` for:
   - Class names matching the feature (e.g., `AccommodationRequest`, `FlightConfirmation`).
   - Controller / gRPC service files containing the feature's verbs (Create, Approve, Allocate).
   - MediatR commands and handlers (search for `: IRequest<` and `: IRequestHandler<`).
   - Integration event publishers and subscribers under `Common.ServiceProviderEvents` and per-module `IntegrationEvents/` folders.

5. **Locate the frontend entry points.** Using the module(s) identified in step 3, search `booking-client/src/modules/` for:
   - Module folders matching the feature.
   - Route definitions (look for `path:` in `routes` files).
   - API client calls (search for the relevant axios call or RTK Query endpoint).
   - State store slices (Zustand stores, TanStack Query keys).

6. **Trace the data flow.**
   - User action → frontend component → API client → backend controller/gRPC handler → MediatR command → domain service → repository → DB.
   - Note where Service Bus events are published or subscribed.
   - Note internal service calls separately from external boundaries, per `.github/references/integration-map.md`. Logistics, People, Settings, and FileAttachments are internal services; iLogistics, WaaS, Service Provider Portal, and Azure AD are external.

7. **Compare documentation with implementation.** Record where code confirms, does not evidence, or contradicts the Confluence scan. Keep `Documented Behaviour` and `Current Implementation` separate.

8. **Identify ownership signals.** README files, code-owners, comments, or the Confluence scan may identify an owner. Preserve the source and authority; don't guess.

9. **Check the configuration surface.**
   - `booking-backend/**/appsettings*.json` — external URLs, feature flags, Service Bus subscription names.
   - `booking-client/**/.env*` or `config.ts` — feature flags (e.g., `REACT_APP_FEATURE_AI_TRIP_PLANNER`), API base URLs.

10. **Cross-check the glossary.** If you encountered a domain term that isn't in `.github/references/glossary.md`, list it under "New glossary candidates" in your output.

## Output template

```
## <Feature> — Repo Scan

**In one sentence:** ...

### Backend modules

- `booking-backend/src/Services/Booking/<Module>/` — <role in one line>
- ...

### Frontend modules

- `booking-client/src/modules/<module>/` — <role>
- ...

### Data flow

1. User clicks ... in `<file>`
2. Frontend calls `<endpoint>`
3. Backend `<controller>` dispatches `<command>` (`<file>`)
4. Handler `<HandlerClass>` writes to `<DbContext>` and publishes `<IntegrationEvent>` on topic `<topic>`
5. ...

### External integrations

- iLogistics: `<client, endpoint, or event>` from `<file>`
- ...

### Configuration

- `<feature flag>`: defined in `<file>`, default `<value>`
- ...

### New glossary candidates

- `<Term>`: <draft definition for BA review>

### Reference corrections

- `<references/file.md>`: <what it says> vs. `<file path>`: <what the code actually does> — reference needs updating.

### Open questions

- Who owns the approval policy for ...?
- Where is the SLA documented?
- ...
```

## Stop conditions

- Stop if the feature isn't found in the repo. Don't speculate — say "feature not found by name; possible matches: ..." and list near-misses.
- Stop if the scan would exceed 800 words of output. Summarize and offer to deep-dive on a section.

## What this skill does NOT do

- Estimate effort or timeline.
- Decide whether a change is good or bad.
- Draft a user story (that's `/ba-user-story`).
- Assess impact of a proposed change (that's `/ba-impact-assessment`).
