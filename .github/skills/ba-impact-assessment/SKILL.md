---
name: ba-impact-assessment
description: "Assess services, data, contracts, integrations, workflows, and user flows affected by a proposed Booking Portal change."
user-invocable: false
---

# Skill: BA Impact Assessment

**When to use:** A change is on the table (new field, new flow, new integration, refactor with user-visible effects) and the team needs to know what would actually be affected before estimating or scheduling.

## Inputs

- A description of the proposed change.
- A Confluence scan for documented behaviour, decisions, ownership, and evidence gaps.
- Optional: a prior `/ba-repo-scan` report for the affected feature. Use it.
- Optional: stakeholder constraints (deadline, must-keep-backward-compatible, etc.).

## Procedure

1. **Establish the evidence basis.** Use the Confluence scan for Documented Behaviour and explicitly authoritative Approved Requirements, and the repo scan for Current Implementation. Cite each source, preserve conflicts, and do not treat publication or a stakeholder request as approval without explicit authority. If you can't restate the target concretely, ask one clarifying question before continuing.

2. **Run a repo scan if you don't have one.** Follow `.github/skills/ba-repo-scan/SKILL.md` first — you can't assess impact without knowing the current implementation.

3. **For each module identified by the scan, classify the impact:**
   - **Code change** — logic changes within an existing class or component.
   - **Schema change** — DB columns, EF migrations, request/response DTOs.
   - **Contract change** — gRPC / REST contracts, integration event payloads.
   - **Config change** — new env vars, feature flags, appsettings entries.
   - **No change** — module is touched by the feature but not by the change.
   Cite a file path for each.

4. **Integration impact.** Separate internal service dependencies from external systems, then decide whether the change affects each boundary:
   - Internal: Logistics, People, Settings, FileAttachments, Messages, and other repository-owned services.
   - External: iLogistics payloads/statuses, WaaS templates, Service Provider Portal event contracts, Google Maps, Azure OpenAI, and Azure AD claims/scopes.
   - Messaging infrastructure: Service Bus event schemas and every internal or cross-solution consumer.

5. **Data impact.**
   - New columns / tables / indexes?
   - Migration script needed?
   - Backfill for existing rows?
   - Reporting / analytics queries affected (PredictorRunner)?

6. **User-flow impact.** Walk through each affected user role (Traveller, Approver, Admin) and list which screens, notifications, or approval steps change.

7. **Cross-cutting checks.**
   - Audit / logging — does the change add a new event we should audit?
   - Accessibility — does any new UI element need a11y considerations?
   - Localization — new user-facing strings?
   - Feature flag — should this ship behind a flag?
   - Telemetry — Application Insights events to add or change.

8. **Risks and unknowns.** List explicitly. Each item: the risk, why it's a risk, and what would resolve it.

## Output template

See [`../../prompts/ba-impact-assessment.prompt.md`](../../prompts/ba-impact-assessment.prompt.md) — the "Required output" section is the contract. This skill's job is to populate it accurately.

## Stop conditions

- Stop and ask if the change is underspecified (e.g., "improve booking flow" — improve how?).
- Stop and ask if the change spans more than three feature areas — split it first.

## What this skill does NOT do

- Estimate effort. Use story points / hours conversations with the team.
- Decide priority. That's the PO's call.
- Write the implementation. Devs do that.
