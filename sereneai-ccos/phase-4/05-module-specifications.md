# Phase 4 — Detailed MVP Module Specifications

> The nine approved MVP modules (Bible §18). Each spec: purpose, primary users, core
> capabilities, data, API surface, events, permissions, and explicit non-goals.
> Functional detail cross-references `04-mvp-functional-requirements.md` (FR-x).

## 1. Home

- **Purpose:** the operational front door — role-appropriate dashboard assembling the day's work, comms and tools. The screen that proves "one workspace" in the first five seconds.
- **Users:** all personas (role variants: Agent / Team Leader / Ops / Executive).
- **Capabilities:** widget grid (announcements, my tasks, pinned knowledge, quick links, team summary, assistant entry); per-user personalisation; per-org default layouts by role (admin-configured); individual widget degradation.
- **Data:** `dashboard_layout` (org defaults + user overrides, JSON against versioned schema). Widgets read from their owning modules — Home stores no operational data.
- **API:** `GET/PUT /api/v1/users/me/dashboard`, `GET /api/v1/orgs/{id}/dashboard-defaults`.
- **Events consumed:** task, announcement, knowledge events (for live counts).
- **Non-goals (MVP):** custom widget development, cross-org benchmarking, BI-style charts.
- FR refs: FR-5.

## 2. My Work

- **Purpose:** single place for operational work — native tasks plus synced external tasks.
- **Users:** Agent (own), Team Leader (team), Ops Manager (department).
- **Capabilities:** personal task list (status/due/priority views); create/assign/reassign within scope; comments and state history; escalation flag; blocked flag; team and department roll-up views; synced Planner tasks with source badge + deep link.
- **Data:** `task` (org_id, title, description, status, priority, due_at, assignee, creator, team_id, source [native|planner|jira], external_ref, flags), `task_comment`, `task_history`.
- **API:** `GET/POST/PATCH /api/v1/tasks`, `POST /api/v1/tasks/{id}/comments`, team/department list endpoints with ABAC filters.
- **Events:** `task.created`, `task.assigned`, `task.completed`, `task.escalated`.
- **Permissions:** matrix in `02-user-ecosystem.md`; scope checks server-side.
- **Non-goals (MVP):** Gantt/dependencies, recurring tasks, SLA timers, workload balancing (V1 native task management territory).
- FR refs: FR-6, FR-10.2.

## 3. Knowledge Hub

- **Purpose:** trusted, governed source of operational knowledge; the substrate for Release 2 AI.
- **Users:** all consume; Ops Manager/Admin govern.
- **Capabilities:** rich-text articles with attachments; categories; audience attributes (role/dept/team) for ABAC visibility; lifecycle Created → Published → Reviewed → Archived; version history; review dates with overdue flags; "flag outdated" → owner task; bulk import (Markdown/Word); external knowledge references (SharePoint links) registered as searchable entries.
- **Data:** `knowledge_article` (org_id, title, summary, body, category_id, status, owner, review_at, audience attrs), `knowledge_version`, `knowledge_category`, `knowledge_usage_event`, `external_knowledge_ref`.
- **API:** `GET/POST/PATCH /api/v1/knowledge/articles`, lifecycle transitions as explicit endpoints (`/publish`, `/archive`, `/review`), `GET /api/v1/knowledge/categories`.
- **Events:** `knowledge.created`, `knowledge.updated`, `knowledge.published`, `knowledge.archived`, `knowledge.flagged`.
- **Non-goals (MVP):** multi-step approval workflow (future lifecycle), authoring collaboration/co-editing, automatic content ingestion from SharePoint bodies (links/metadata only in MVP).
- FR refs: FR-7.

## 4. Global Search

- **Purpose:** one search box over everything the user may see.
- **Capabilities:** unified query across knowledge, tasks, announcements, quick links and external knowledge refs; type-grouped results; permission filtering at query level; zero-result fallbacks (assistant in R2, knowledge-gap flag); search analytics events.
- **Implementation:** PostgreSQL FTS (`tsvector` columns + GIN indexes) per searchable entity; a thin search service composes per-entity queries with the caller's ABAC context. pgvector semantic search arrives with R2 for knowledge only.
- **API:** `GET /api/v1/search?q=&types=`.
- **Non-goals (MVP):** external-system live search (e.g. querying Jira at search time), spelling correction, dedicated search infrastructure (Elastic et al.).
- FR refs: FR-11.

## 5. sereneAI Assistant (Release 2)

- **Purpose:** governed, permission-aware operational assistant foundation — Knowledge Q&A with citations, safe summaries, navigation help.
- **Capabilities:** chat panel available from shell; RAG over published+visible knowledge (approved flow, Bible §12); citations on every answer; explicit no-source response; article and my-tasks summaries; answer rating; session memory only (plus user preferences); per-org enablement flag + provider/model configuration; "AI may be inaccurate, check sources" persistent disclosure.
- **Architecture:** provider-agnostic gateway (single internal interface; adapters per provider); embedding pipeline triggered by `knowledge.published`/`updated` events; org-scoped pgvector store; retrieval query carries org_id + ABAC attributes as hard filters.
- **Data:** `assistant_session`, `assistant_message` (with retrieved-source list), `knowledge_embedding` (org_id, article_id, chunk, vector), `assistant_feedback`.
- **API:** `POST /api/v1/assistant/messages`, `GET /api/v1/assistant/sessions`, `POST /api/v1/assistant/feedback`.
- **Events:** `assistant.response_generated`, `assistant.rag_source_used`.
- **Non-goals (MVP):** actions of any kind (create/update/delete), autonomous behaviour, long-term memory, cross-system answers, voice.
- FR refs: FR-12.

## 6. Announcements

- **Purpose:** governed organisational communication with evidence of receipt.
- **Capabilities:** audience-targeted posts (team/dept/org via ABAC attributes); unread-first surfacing on Home; optional required acknowledgement with per-user timestamped evidence; publisher read/ack reporting within scope; pinned/expiry dates.
- **Data:** `announcement` (org_id, title, body, audience attrs, requires_ack, publish_at, expires_at, author), `announcement_receipt` (user, read_at, acked_at).
- **API:** `GET/POST /api/v1/announcements`, `POST /api/v1/announcements/{id}/ack`, `GET /api/v1/announcements/{id}/receipts`.
- **Non-goals (MVP):** comments/reactions, scheduling campaigns, push/email channels (in-app + notification entry only).
- FR refs: FR-9.1, FR-9.2.

## 7. Administration

- **Purpose:** complete in-app organisational self-management; the auditor's window into the platform.
- **Capabilities:** user lifecycle (create/invite/edit/deactivate, CSV import); teams & departments; role assignment with permission-diff preview; branding & navigation configuration (validated schema); module feature flags; audit log viewer with filters + CSV export; onboarding checklist; AI governance settings page (R2: enable flag, provider/model, data boundary notice).
- **Data:** `org_configuration` (versioned), `feature_flag`, plus user/team/role tables owned by their modules; `audit_event` (append-only).
- **API:** `/api/v1/admin/*`, `/api/v1/audit`, `/api/v1/orgs/{id}/config`.
- **Permissions:** Admin full (own org); Auditor read-only audit+config; Ops Manager limited delegation (own department's users/teams).
- **Non-goals (MVP):** custom role builder (V1), cross-org Serene AI back-office console (internal provisioning routine only, FR-14.4), self-service org signup.
- FR refs: FR-1.2, FR-2, FR-3, FR-4, FR-13, FR-14.

## 8. Integrations Foundation

- **Purpose:** the connector framework — central to Integrate First — plus the first Microsoft connectors.
- **Framework (declarative, marketplace-ready per O4-03):** each connector defines auth method, config schema, entity mappings, sync rules, webhook endpoints (foundation only in MVP), error taxonomy. Runtime provides: encrypted secret storage, OAuth flows, scheduled sync runner, manual sync trigger, item-level sync reports, health states (Healthy/Degraded/Unhealthy/Disconnected), admin notifications on failure, full audit.
- **MVP connectors:** Microsoft Planner (tasks, read-first; write-back flagged), SharePoint (knowledge references). Jira fast-follow R1.x (P4-02).
- **Sync models:** manual + scheduled in MVP; webhook receiver scaffolding only.
- **Data:** `connector_instance` (org_id, type, status, config, secret refs), `sync_run`, `sync_item_result`, `external_ref` links on tasks/knowledge.
- **API:** `GET/POST/PATCH/DELETE /api/v1/integrations`, `POST /api/v1/integrations/{id}/sync`.
- **Events:** `integration.connected`, `integration.sync_started`, `integration.sync_completed`, `integration.sync_failed`.
- **Non-goals (MVP):** bidirectional conflict resolution beyond last-write-wins + history note; connector SDK for third parties; telephony/CRM connectors.
- FR refs: FR-10.

## 9. Configuration Foundation

- **Purpose:** make "configuration before customisation" real — one mechanism through which an org's branding, navigation, roles, dashboards, modules and AI settings are expressed as versioned, validated data.
- **Capabilities:** JSON configuration documents against versioned schemas; validation on write (reject, don't apply); change history (who/when/diff) in audit; export/import org configuration (deployment portability across SaaS → private cloud → self-hosted); feature-flag evaluation library used by backend and shell.
- **Data:** `org_configuration` (current + history), `config_schema_version`.
- **Why it matters:** this is the upgrade-safety mechanism (Bible §20) and the answer to configuration-drift risk R4-06. Schema versioning lets core upgrades migrate customer config deterministically.
- **Non-goals (MVP):** per-customer code plugins, theming beyond tokens (logo, colour palette, display name), customer-authored workflow config (V1 Workflow Engine).
- FR refs: FR-14.

---

## Cross-module foundations (Release 0 infrastructure)

| Foundation | Used by |
|---|---|
| Tenancy layer: org_id on every tenant-owned row, enforced centrally (ORM middleware) + Postgres RLS defence-in-depth + CI cross-tenant leak tests | all modules |
| Internal event log (DB-backed) with simple synchronous/queued-later handlers | notifications, audit, analytics, embeddings |
| Append-only audit store | all modules |
| Notification service (in-app) | tasks, announcements, knowledge, integrations |
| Permission engine (RBAC role grants ∩ ABAC attribute scope), single evaluation path | every endpoint |
| Validated configuration store + feature flags | shell, all modules |
