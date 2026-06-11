# Phase 4 — MVP Release Plan

> Three approved releases (Bible §19). Durations are planning estimates for a small product
> team (≈3–5 engineers + 1 product + 1 design); calendar dates are set at build kickoff.
> Releases ship sequentially; each has hard exit criteria — scope flexes before exit criteria do.

## Release 0 — Foundation (≈ 6–8 weeks)

**Theme:** a secure, multi-tenant, auditable, configurable shell. Nothing demo-flashy;
everything load-bearing.

**Scope:** platform setup (repo, CI, environments, Docker), authentication (FR-1.1–1.3),
organisation provisioning (FR-14.4), users (FR-2.1, 2.3), teams (FR-3), roles + permission
engine (FR-4.1), tenancy layer + RLS + leak-test suite, audit log + viewer (FR-13), internal
event log, UI shell + navigation, branding/navigation config + feature flags (FR-14.1, 14.2),
seed-data script.

**Exit criteria**
1. Two seeded organisations coexist with zero cross-tenant leakage (NFR-T3 suite green).
2. An admin can: log in, brand the org, create teams, invite users, assign roles — all in-app, all audited.
3. Permission engine blocks every undeclared endpoint access (NFR-S4 inventory test green).
4. CI pipeline (NFR-Q1) green and mandatory.
5. Restore-from-backup demonstrated (NFR-A2).

**Key risks:** tenancy/permission engine design errors are catastrophic-later — this release
deliberately overweights engineering review on those two components.

## Release 1 — Operational Hub MVP (≈ 8–10 weeks)

**Theme:** the daily-habit workspace. After this release a pilot contact centre can run real
work in CCOS with no AI involved.

**Scope:** Home + role variants + degradation (FR-5.1), personalisation (FR-5.2, 5.3 —
Should), tasks (FR-6.1–6.4, 6.5 Should), knowledge (FR-7.1–7.3, 7.4 Should), quick links
(FR-8), announcements + receipts (FR-9.1, 9.2), notifications (FR-9.3), global search
(FR-11), connector framework + Planner connector (FR-10.1, 10.2), SharePoint references
(FR-10.3 — Should), CSV import (FR-2.2 — Should), onboarding checklist (FR-14.3 — Should).

**Exit criteria**
1. End-to-end journeys J2–J6 and J8 pass automated E2E tests and a scripted demo run.
2. A pilot-shaped org (50 users, 200 articles, 500 tasks, 1 Planner plan) onboarded from empty in ≤2 weeks elapsed, using only in-app admin + documented playbook.
3. Search P95 ≤1s on seeded pilot-scale data (NFR-P2).
4. Planner sync runs scheduled + manual with item-level reports; failure modes produce admin notifications, not silence.
5. Shift-start load profile passes (NFR-P5).

**Go-to-market milestone:** first design-partner pilot can start on Release 1 — AI is
explicitly not required for pilot value (Bible: MVP is not an AI transformation project).

## Release 2 — AI Enabled Operations (≈ 6–8 weeks)

**Theme:** governed AI on a trusted foundation — the proof of Transform Fourth credibility.

**Scope:** provider-agnostic AI gateway, embedding pipeline (event-driven, org-isolated
pgvector), assistant panel with RAG Q&A + citations (FR-12.1), full AI audit (FR-12.2), AI
governance settings + org enablement checklist (NFR-AI1), answer rating (FR-12.3 — Should),
safe summaries (FR-12.4 — Should), search zero-result → assistant handoff.

**Exit criteria**
1. Journey J7 passes E2E including no-source, provider-outage and org-disabled paths.
2. 100% of assistant answers in test runs carry citations or explicit no-source response (NFR-AI3 pipeline-enforced).
3. Cross-tenant retrieval probes return nothing, at FTS and vector layers (NFR-T4).
4. AI audit trail demonstrably reconstructs any answer: who asked what, what was retrieved, what model said what (auditor walkthrough).
5. Provider switch (e.g. between two configured providers) is a config change with no deploy.

**Go-to-market milestone:** the governed-AI demo for regulated buyers (O4-01).

## Post-MVP runway (context, not commitment)

V1 (per Bible §19): SSO/Entra ID, workflow engine, reporting module, notification centre,
native task management (first Replace milestone, P4-04), enhanced knowledge management,
role-specific dashboards, improved connector framework + Jira GA, queue-based processing,
better AI governance.

## Cross-release engineering rules

1. **No release skips its exit criteria** — dates move or Should/Could scope drops; quality and security criteria never do.
2. **Audit, tenancy and events are never retrofitted** — every new feature lands with its events and audit coverage in the same PR.
3. **Demo dataset maintained continuously** — every feature lands with seed-data support so demos and E2E tests never go stale.
4. **Self-hosted honesty check** at each release exit: `docker compose up` on a clean machine produces a working platform (NFR-U3), even though SaaS ships first.
