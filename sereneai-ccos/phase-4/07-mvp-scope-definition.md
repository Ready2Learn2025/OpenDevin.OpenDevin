# Phase 4 — MVP Scope Definition (MoSCoW)

> Scope is protected aggressively. Anything not listed as Must/Should/Could is **Won't (this
> MVP)** by default. Changes to Must-have scope require a Decision Register entry in the
> Project Bible.

## Must Have (MVP fails without these)

**Foundation (Release 0)**
- Email/password authentication, password reset, secure sessions, lockout (FR-1.1, 1.2)
- SSO/MFA-ready identity model (FR-1.3)
- Organisation provisioning (internal routine) (FR-14.4)
- Users: create/invite/edit/deactivate (FR-2.1); profiles (FR-2.3)
- Teams & departments (FR-3)
- System roles + RBAC∩ABAC permission engine, server-side everywhere (FR-4)
- Tenancy layer: org scoping + RLS + CI cross-tenant leak tests
- Append-only audit log + admin/auditor viewer + CSV export (FR-13)
- Internal event log foundation
- UI shell: top bar, side nav, branding, responsive frame
- Branding & navigation configuration, feature flags, validated config store (FR-14.1, 14.2)

**Operational Hub (Release 1)**
- Home with role-variant widgets and per-widget degradation (FR-5.1)
- Tasks: my list, create/assign/reassign in scope, status+comments+history, escalation (FR-6.1–6.4)
- Knowledge: authoring, lifecycle, audience ABAC, versioning, review dates, flag-outdated (FR-7.1–7.3)
- Quick links: curated + personal, click logging (FR-8)
- Announcements: targeted, ack-required option, receipts (FR-9.1, 9.2)
- In-app notifications (FR-9.3)
- Global search, permission-filtered, with analytics (FR-11)
- Planner connector (read-first, manual+scheduled sync) on the connector framework (FR-10.1, 10.2)

**AI Enabled Operations (Release 2)**
- Assistant: RAG Q&A with citations over published+visible org knowledge (FR-12.1)
- Full AI audit: prompt/response/sources/user/org/provider/model (FR-12.2)
- Per-org assistant flag + provider/model config + provider-agnostic gateway
- pgvector embedding pipeline, org-isolated

## Should Have (high value; first to flex if dates slip)

- Home personalisation (FR-5.2) and team/department summary widgets (FR-5.3)
- CSV user bulk import (FR-2.2)
- Knowledge bulk import + SharePoint external references (FR-7.4, FR-10.3)
- Synced-task badges + deep links surfaced in My Work views (FR-6.5)
- Onboarding checklist (FR-14.3)
- Assistant answer rating (FR-12.3) and safe summaries (FR-12.4)
- Permission-diff preview on role change (FR-4.2)

## Could Have (only if ahead of schedule)

- TOTP MFA for admins (FR-1.4)
- Planner write-back behind flag
- Email notification digests
- Announcement pinning/expiry refinements
- Dark mode

## Won't Have (this MVP) — explicit exclusions

Per Bible §18 plus Phase 4 clarifications:

- Native CRM, telephony, WFM, QA
- Advanced AI agents, autonomous/destructive AI actions, long-term AI memory
- Full automation engine, complex workflow orchestration, workflow engine (V1)
- Reporting module / report builder (V1) — MVP ships only Home widget counters + audit/search analytics events
- Enterprise event bus, queues beyond minimal need, microservices, Kubernetes
- SSO/Entra ID login (V1 — MVP is SSO-*ready* only)
- Custom role builder (V1) — system roles only
- Jira connector (fast-follow R1.x), telephony/CRM/WFM connectors
- Marketplace, connector SDK
- Native mobile apps
- Self-service org signup; cross-org back-office console
- Co-editing, approval workflows for knowledge (future lifecycle)
- Localisation beyond architecture readiness

## Scope-protection rules

1. **Trade, don't add.** A new Must enters only if something of equal size leaves, via Decision Register.
2. **Future-proofing is design, not build.** We design so V1/V2 features fit (e.g. SSO-ready identity), but build none of them early.
3. **Demo pressure is not scope pressure.** Demo gaps are met with roadmap narrative, not unplanned features.
4. **The fastest route to proving market value wins** every prioritisation argument (Bible: MVP Scope Discipline).
5. Each release has exit criteria (`09-mvp-release-plan.md`); scope flexes before quality or exit criteria do — Shoulds flex first, then Coulds are dropped entirely.
