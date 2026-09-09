# Impact Assessment — <Change in one sentence>

**Status:** Draft  
**Source ask:** <inbox link>  
**Related Confluence scan:** <path and status>  
**Related repo scan:** <path to repo-scan output>  
**BA initials / date:** <XX> / <YYYY-MM-DD>

## Proposed change (BA framing)

Restate the requested target in one sentence. Record whether it is approved or pending confirmation.

## Evidence basis

- Current implementation: <repo scan and supporting code/config/test paths>.
- Documented behaviour: <Confluence page keys/titles and scan path, other product documentation paths, or `Not found in searched scope`>.
- Requested target: <source ask>.
- Approved requirement: <authoritative approval or decision source, or `Not confirmed`>.
- Conflicts: <contradictions between sources, or `None identified`>.

## Affected internal services

For each service touched, classify the impact:

| Service | Impact type | Notes |
|---|---|---|
| <Service> | code / schema / contract / config / no change | cite file path |

(Use the service-area inventory from `.github/references/service-map.md`.)

## Affected frontend modules

| Module | Impact type | Notes |
|---|---|---|

## Integration events

For each event affected (search `Common.Contracts.*` and per-module `IntegrationEvents/`):

| Event | Type of impact | Subscribers affected |
|---|---|---|

## External system impact

For each external system from `references/integration-map.md` that might be affected:

| System | Impact | Coordination required? |
|---|---|---|
| Azure AD | claims/scopes change? | |
| WaaS | template change? new booking-request type? | |
| Service Provider Portal | schema or event-contract change? | |
| iLogistics | payload, status, allocation, or reconciliation change? | |
| Azure OpenAI / Cognitive Search | prompt / index schema? | |
| Service Bus | new event types? | |

## Data impact

- Schema changes (per affected `DbContext`): ...
- Migrations required: ...
- Backfill considerations: ...
- Reporting / dashboard impact: ...

## User-flow impact

For each affected persona (`references/personas.md`):

- **<Persona>:** which screens change, which steps gain/lose, which permissions matter.

## Workflow impact

- Does the state machine change? (new transitions, new exit paths)
- Does the WaaS template need updating?
- Does a new booking-request type need adding (new mapping)?

## Cross-cutting

- Audit / logging: ...
- Accessibility: ...
- Localization: ...
- Feature flag: should this ship behind one? Name suggestion: `REACT_APP_FEATURE_<X>`.
- Telemetry: Application Insights events to add or change.

## Risks and unknowns

- **<Risk>** — Why: ... Resolution: ...

## Suggested next BA artifact

- One user story per affected user flow (run `/ba-user-story` per flow).
- Or: a clarifying note back to stakeholder, listing what we'd need answered first.

## Change log

- YYYY-MM-DD — Created by <BA>.
