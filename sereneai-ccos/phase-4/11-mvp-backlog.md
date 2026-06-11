# Phase 4 — MVP Backlog

> Epics → stories, mapped to releases (R0/R1/R2), MoSCoW priority, and FR references.
> Sizing: S (≤2 dev-days), M (≤1 week), L (1–2 weeks). This is the build-entry backlog;
> teams decompose L stories at sprint planning.

## Epic E0 — Engineering Foundation (R0)

| ID | Story | Pri | Size |
|---|---|---|---|
| E0-1 | Monorepo, CI pipeline (lint, typecheck, tests, dep scan), environments, Docker images + compose | M | M |
| E0-2 | PostgreSQL schema baseline, migration tooling, seed-data script | M | M |
| E0-3 | Tenancy layer: org_id enforcement in query layer + Postgres RLS | M | L |
| E0-4 | Cross-tenant leak test suite wired into CI (NFR-T3) | M | M |
| E0-5 | Internal event log + handler registration | M | M |
| E0-6 | Append-only audit store + write API used by all modules (FR-13.1) | M | M |
| E0-7 | Structured logging, correlation IDs, health endpoints, error tracking (NFR-A3) | M | S |

## Epic E1 — Identity & Access (R0)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E1-1 | Email/password auth, sessions, lockout, login audit | M | M | FR-1.1 |
| E1-2 | Password reset flow | M | S | FR-1.2 |
| E1-3 | SSO/MFA-ready identity model (user ↔ credential separation) | M | M | FR-1.3 |
| E1-4 | Org provisioning routine (internal, audited) | M | M | FR-14.4 |
| E1-5 | User CRUD + invite + deactivate | M | M | FR-2.1 |
| E1-6 | Teams & departments + membership | M | M | FR-3 |
| E1-7 | Permission engine (RBAC∩ABAC) + endpoint permission declarations + default-deny inventory test | M | L | FR-4.1 |
| E1-8 | Role assignment UI + permission-diff preview | M/S | M | FR-4.1/4.2 |
| E1-9 | User profile + preferences | M | S | FR-2.3 |
| E1-10 | CSV bulk user import with dry-run | S | M | FR-2.2 |
| E1-11 | TOTP MFA for admins | C | M | FR-1.4 |

## Epic E2 — Shell & Configuration (R0)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E2-1 | UI shell: top bar, side nav, responsive frame, shared component library | M | L | — |
| E2-2 | Branding configuration (logo, palette w/ contrast validation, display name) | M | M | FR-14.1 |
| E2-3 | Navigation configuration (rename/hide modules) | M | S | FR-14.1 |
| E2-4 | Validated, versioned config store + history | M | M | FR-14.1 |
| E2-5 | Feature flags per org + evaluation library | M | S | FR-14.2 |
| E2-6 | Audit log viewer + filters + CSV export (Admin/Auditor) | M | M | FR-13.2 |
| E2-7 | Accessibility baseline: keyboard nav, focus, landmarks, axe in CI | M | M | NFR-Q4 |

## Epic E3 — Home (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E3-1 | Widget framework with per-widget degradation + empty states | M | L | FR-5.1 |
| E3-2 | Widgets: announcements, my tasks, pinned knowledge, quick links | M | M | FR-5.1 |
| E3-3 | Role-variant default layouts (admin-configurable) | M | M | FR-5.1 |
| E3-4 | User personalisation (reorder/hide, persisted) | S | M | FR-5.2 |
| E3-5 | Team/department summary widget with drill-through | S | M | FR-5.3 |

## Epic E4 — My Work (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E4-1 | Task model + my-list views (status/due/priority) | M | M | FR-6.1 |
| E4-2 | Create/assign/reassign within ABAC scope + notifications | M | M | FR-6.2 |
| E4-3 | Status transitions, comments, immutable history | M | M | FR-6.3 |
| E4-4 | Personal tasks + escalation to Team Leader | M | S | FR-6.4 |
| E4-5 | Team & department roll-up views | M | M | FR-6.2 |
| E4-6 | Source badges + deep links for synced tasks | S | S | FR-6.5 |

## Epic E5 — Knowledge Hub (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E5-1 | Article model, rich-text editor, attachments, categories | M | L | FR-7.1 |
| E5-2 | Lifecycle states + explicit transition endpoints + version history | M | M | FR-7.1 |
| E5-3 | Audience ABAC visibility (browse/search/AI all filtered) | M | M | FR-7.1/7.2 |
| E5-4 | Review dates, overdue flags, owner review tasks | M | S | FR-7.1 |
| E5-5 | Flag-outdated → owner task | M | S | FR-7.3 |
| E5-6 | Usage event logging on view | M | S | FR-7.2 |
| E5-7 | Bulk import (Markdown/Word) | S | M | FR-7.4 |
| E5-8 | External knowledge references (SharePoint links) | S | M | FR-7.4/10.3 |

## Epic E6 — Comms & Notifications (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E6-1 | Announcement model + audience targeting + composer by permission | M | M | FR-9.1 |
| E6-2 | Required-acknowledgement + receipts + publisher reporting | M | M | FR-9.1/9.2 |
| E6-3 | In-app notification centre fed by event log | M | M | FR-9.3 |
| E6-4 | Notification preferences | S | S | FR-9.3 |
| E6-5 | Email digests | C | M | — |

## Epic E7 — Quick Links (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E7-1 | Curated org/role link sets (admin) | M | S | FR-8.1 |
| E7-2 | Personal pins + ordering | M | S | FR-8.2 |
| E7-3 | Click logging + report-broken-link task | M | S | FR-8.2 |

## Epic E8 — Search (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E8-1 | FTS indexing (tsvector+GIN) on knowledge/tasks/announcements/links | M | M | FR-11 |
| E8-2 | Unified search API + permission filtering + grouped results UI | M | L | FR-11 |
| E8-3 | Zero-result fallbacks (gap flag; assistant handoff in R2) | M | S | FR-11 |
| E8-4 | Search analytics events | M | S | FR-11 |

## Epic E9 — Integrations Foundation (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E9-1 | Connector framework: declarative definition, encrypted secrets, OAuth flows, health states | M | L | FR-10.1 |
| E9-2 | Sync runner: scheduled + manual, item-level reports, admin failure notifications | M | L | FR-10.1 |
| E9-3 | Planner connector (read-first task sync) | M | L | FR-10.2 |
| E9-4 | SharePoint connector (knowledge references) | S | M | FR-10.3 |
| E9-5 | Planner write-back behind flag | C | M | FR-10.2 |
| E9-6 | Webhook receiver scaffolding (foundation only) | S | S | FR-10.1 |

## Epic E10 — Onboarding & Admin polish (R1)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E10-1 | Guided onboarding checklist with progress | S | M | FR-14.3 |
| E10-2 | Onboarding playbook documentation (content seeding, consent guide — R4-01/R4-02 mitigations) | M | S | — |

## Epic E11 — AI Assistant (R2)

| ID | Story | Pri | Size | FR |
|---|---|---|---|---|
| E11-1 | Provider-agnostic AI gateway + org provider/model config | M | M | FR-12.1 |
| E11-2 | Embedding pipeline (event-driven; org-scoped pgvector) | M | L | FR-12.1 |
| E11-3 | RAG retrieval with org_id + ABAC hard filters + tests | M | L | FR-12.1 |
| E11-4 | Assistant panel UI (slide-over, citations, disclosures) | M | L | FR-12.1 |
| E11-5 | Citation/no-source enforcement in answer pipeline | M | M | NFR-AI3 |
| E11-6 | Full AI audit logging | M | M | FR-12.2 |
| E11-7 | Org enablement flag + governance checklist + settings page | M | M | NFR-AI1 |
| E11-8 | Answer rating 👍/👎 + comment | S | S | FR-12.3 |
| E11-9 | Safe summaries (article, my tasks) | S | M | FR-12.4 |
| E11-10 | Search zero-result → assistant handoff | S | S | FR-11 |
| E11-11 | Provider outage / disabled-org degradation paths | M | S | NFR-A5 |

## Epic E12 — Release hardening (each release exit)

| ID | Story | Pri | Size |
|---|---|---|---|
| E12-1 | E2E tests for journeys J1–J8 (built up across releases) | M | L |
| E12-2 | Load test: shift-start profile (NFR-P5) | M | M |
| E12-3 | Pre-pilot penetration test + remediation | M | M |
| E12-4 | Backup/restore drill (NFR-A2) | M | S |
| E12-5 | `docker compose up` clean-machine check per release (NFR-U3) | M | S |

---

### Backlog rules

1. Stories trace to FR/NFR IDs; orphan stories are challenged at planning.
2. Every feature story includes its events, audit coverage, permissions and seed data — no retrofit (release plan rule 2).
3. MoSCoW per `07-mvp-scope-definition.md`; Shoulds flex first when dates slip, Coulds drop entirely.
4. New scope enters only through the Project Bible Decision Register.
