# Booking Portal — Copilot Instructions

This repository hosts the **Booking Portal**, an enterprise travel booking platform for ADNOC Group employees and their families. It coordinates multi-leg travel (flights, accommodations, ground transportation, shuttle buses, cars-with-drivers) with approval workflows (external WaaS), resource allocation, fine-grained operations-based RBAC, and AI-assisted trip planning.

These instructions apply to every Copilot chat in this workspace. Path-scoped guidance is in `.github/instructions/`. Agent role specs live in `.github/agents/`. Reusable workflows live in `.github/prompts/` (invoke with `/name`). Personas live in `.github/chatmodes/`. Templates for BA artifacts live in `.github/templates/`. BA outputs land in `.github/ba-outputs/`.

## Repository layout

- `booking-backend/` — .NET 8 microservices. Eleven top-level service areas under `src/Services/`, plus the `Common/` shared libraries. Solution: `booking-backend/src/Services/BookingPortal.sln`.
- `booking-client/` — React 18 + TypeScript frontend. Six top-level modules under `src/modules/`. Vite migration planned (see `booking-client/documentation/MIGRATION_TO_VITE.md`).
- `.github/` — BA orchestration. See `.github/agents/README.md` for the orchestration map.

## Primary audience

The user is a **Business Analyst**. Default to BA framing (business language, user roles, flows) unless the request is explicitly developer-focused. Always cite file paths for non-obvious claims — the BA will verify.

## When to invoke what

- **Full pipeline from a raw ask** → select the `orchestrator` custom agent (or the legacy `BA Orchestrator` chat mode). It delegates Intake → Confluence-Scout → Repo-Scout → Impact-Assessor → Gap-Analyst → Story-Writer, with Quality-Checker gates.
- **Product Q&A** → chat mode `BA Product Expert`.
- **Single-phase work** → slash prompts: `/ba-intake`, `/ba-confluence-scan`, `/ba-repo-scan`, `/ba-impact-assessment`, `/ba-gap-analysis`, `/ba-user-story`, `/ba-acceptance-criteria`, `/ba-quality-check`, `/ba-release-notes`.

## Source-of-truth rules

- Curated summary: `.github/references/` (product-overview, service-map, frontend-map, integration-map, workflows, personas, glossary). Read these first for grounding.
- Canonical: the code. When in doubt, search the repo.
- ADNOC Confluence: search during every full feature/change pipeline before repository analysis. Confluence unavailability for any reason is a pipeline blocker — the Orchestrator stops and asks the BA to fix access before continuing (see `.github/agents/orchestrator.agent.md`). Treat retrieved content as Documented Behaviour, Historical Context, or Approved Requirement only when explicit authority is recorded; never as proof of Current Implementation. Cite page keys and titles without URLs.
- Don't invent class names, endpoint URLs, env vars, or domain terms. If you can't find it, say so and point at where it would live.
- Ground project-specific conclusions in project evidence. Keep current implementation, documented behaviour, requested target, approved requirement, assumption, recommendation, and conflict distinct.
- A stakeholder request describes a requested target; treat it as approved only when an authoritative source explicitly confirms approval.
- Preserve conflicts between documentation, stakeholder asks, and implementation instead of silently choosing one source.

## Important facts that are easy to get wrong

- The **Workflows** service does **not** decide approval chains. Chains live in **external WaaS**. Workflows holds: per-tenant mappings to WaaS templates, approver groups, and per-request state machines. See `references/workflows.md`.
- People, Settings, Logistics, FileAttachments, Messages, Analytics are **internal services in this repo**, not external dependencies. See `references/integration-map.md`.
- Authorization is **operations-based**, not role-name-based. `Common.Contracts.Authorization/Enums/Operation.cs` is the canonical operation list; roles are admin-created at runtime. See `references/personas.md`.
- The **Aggregator** is the only externally-reachable backend service. Frontend API traffic, including AI Trip Planner messages, enters here; Aggregator calls internal services over gRPC.

## Output conventions

- User stories save to `.github/ba-outputs/user-stories/<slug>.md`.
- Confluence scans save to `.github/ba-outputs/analyses/confluence-scans/<slug>.md`.
- Repo scans, impact assessments, gap analyses save to `.github/ba-outputs/analyses/<type>/<slug>.md`.
- Release notes save to `.github/ba-outputs/release-notes/<release>.md`.
- Decisions save to `.github/ba-outputs/decisions/YYYY-MM-DD-<slug>.md`.
- Raw asks live in `.github/ba-outputs/inbox/YYYY-MM-DD-<slug>.md`.
- Templates for each artifact live in `.github/templates/`.
- Plain prose, paragraphs by default. Bullets only for genuine lists (modules, ACs, integrations). No emojis. No marketing language.
- Business language first. Surface technical detail only when the BA needs it to decide.
- Re-run Quality-Checker after every scope change. Cross-check the story against the Confluence scan, repo scan, impact assessment, and gap analysis before responding; keep evidence classifications, counts, risks, dependencies, and missing-information items consistent across all artifacts.

## When unsure

Ask the BA exactly one focused clarifying question. Underspecified asks cost more to redo than to clarify.
