# Repo Scan — <Feature or area>

**Status:** Draft  
**Source ask:** <inbox link>  
**Related Confluence scan:** <path and status>  
**BA initials / date:** <XX> / <YYYY-MM-DD>

## Feature in one sentence

Plain business language.

## Backend services touched

For each, cite the path under `booking-backend/src/Services/`:

- `<Service / sub-domain>` — role in this feature.

## Frontend modules touched

For each, cite the path under `booking-client/src/modules/`:

- `<Module / sub-feature>` — role.

## Data flow (end-to-end)

1. User action in `<file>`.
2. Frontend calls `<Aggregator endpoint>`.
3. Aggregator proxies via gRPC to `<service>`.
4. Service handler `<HandlerClass>` writes to `<DbContext>` and publishes `<IntegrationEvent>` on topic `travel`.
5. Subscribers: `<services>`.
6. (Continue.)

## Documentation comparison

- **Confirmed by implementation:** <Confluence finding and supporting code path, or `None identified`>.
- **Conflict:** <Confluence finding versus code/config/test evidence, or `None identified`>.
- **Not evidenced in code:** <documented statement not found in implementation, or `None identified`>.

## Workflow involvement

- Does this feature involve approvals? If yes:
  - Which `BookingRequestType` flows through Workflows?
  - WaaS template (if known): ...
  - Approver groups involved: ...

## External integrations

- WaaS: yes / no — details.
- Azure AD: claims used.
- Service Provider Portal: yes / no — directional event contract(s).
- iLogistics: yes / no — client, payload/status, and failure handling.
- Other: ...

## Permissions / operations enforced

- Backend: which operations gate which endpoints. Cite the `[Authorize]` attributes or in-handler checks.
- Frontend: which `UserPermissions` gate which UI.

## Configuration surface

- `appsettings.json` entries.
- Feature flags.
- Settings-service config keys.

## New glossary candidates

Terms encountered that aren't yet in `references/glossary.md`. List with one-line definitions for BA review.

## Open questions

- ...

## Change log

- YYYY-MM-DD — Created by <BA>.
