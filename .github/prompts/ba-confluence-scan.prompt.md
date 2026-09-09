---
agent: confluence-scout
description: "Search ADNOC Confluence for documented behaviour, decisions, owners, and constraints related to a Booking Portal feature."
---

# /ba-confluence-scan

Search ADNOC Confluence for evidence related to **${input:feature:feature name or stakeholder request}**. Follow [`../skills/ba-confluence-scan/SKILL.md`](../skills/ba-confluence-scan/SKILL.md).

Save the result to `.github/ba-outputs/analyses/confluence-scans/<short-kebab-slug>.md` using [`../templates/confluence-scan.md`](../templates/confluence-scan.md).

Identify sources by page key and title without including Confluence URLs. Keep documented behaviour, approved requirements, historical context, conflicts, and evidence gaps distinct. If Confluence is unavailable, save an `Unavailable` artifact rather than silently skipping the phase.