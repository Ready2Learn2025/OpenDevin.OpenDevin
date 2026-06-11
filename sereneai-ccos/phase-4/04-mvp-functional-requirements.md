# Phase 4 — MVP Functional Requirements

> Format: user stories with acceptance criteria (AC), business rules (BR) and dependencies.
> Story IDs are stable and referenced by the backlog (`11-mvp-backlog.md`). Priorities use
> MoSCoW per `07-mvp-scope-definition.md`. All requirements are tenant-scoped and
> server-side enforced (Bible §15, §16).

## FR-1 Authentication

**FR-1.1 (Must)** As a user, I can log in with email and password so that I can access my organisation's workspace.
- AC: valid credentials → session issued and Home rendered; invalid → generic error; ≥5 failures in 15 min → temporary lockout; `auth.login` / `auth.login_failed` audit events.
- BR: passwords ≥12 chars, hashed with a modern KDF (argon2id/bcrypt); sessions httpOnly, secure, expiring; no account enumeration in any response.

**FR-1.2 (Must)** As a user, I can reset my password via email so that I can recover access myself.
- AC: single-use, time-limited token; all sessions invalidated on reset; audit event.

**FR-1.3 (Must)** As the platform, authentication is MFA-ready and SSO-ready so that V1 Entra ID/OIDC lands without auth-model rework.
- AC: identity model separates *user* from *credential/identity-provider link*; adding an OIDC identity requires no user-table migration.

**FR-1.4 (Could)** TOTP MFA enrolment for admins.

## FR-2 User Management

**FR-2.1 (Must)** As an admin, I can create, invite, edit and deactivate users in my organisation.
- AC: invited users set their own password via emailed link; deactivation kills sessions immediately and preserves history; all changes audited.
- BR: users belong to exactly one organisation; email unique per organisation; deactivated users keep task/knowledge attribution.

**FR-2.2 (Should)** As an admin, I can bulk-import users via CSV.
- AC: dry-run validation report; row-level errors; valid rows applied atomically per row.

**FR-2.3 (Must)** As a user, I can edit my profile (display name, preferences) but not my own role or team.

## FR-3 Teams

**FR-3.1 (Must)** As an admin, I can create teams within departments and assign users.
- AC: hierarchy is Organisation → Department → Team → User (department optional for small orgs); a user has one primary team; team changes audited.
- BR: team membership drives ABAC filters for tasks, announcements and knowledge audiences.

**FR-3.2 (Must)** As a Team Leader, I am linked to my team(s) so that team-scoped views and permissions resolve correctly.

## FR-4 Roles & Permissions

**FR-4.1 (Must)** The platform ships with system roles — Agent, Team Leader, Operations Manager, Executive, Admin, Auditor — with the permission matrix defined in `02-user-ecosystem.md`.
- AC: every API endpoint declares required permissions; requests without them → 403 + audit event; permissions evaluated server-side only.
- BR (RBAC+ABAC): effective access = role permissions ∩ attribute scope (org, department, team, audience). System roles are not editable in MVP (custom role builder is V1).

**FR-4.2 (Must)** As an admin, I can assign roles to users and preview the permission diff before confirming.

## FR-5 Dashboard (Home)

**FR-5.1 (Must)** As a user, I see a role-appropriate Home with widgets: announcements, my tasks, pinned knowledge, quick links, assistant entry (R2).
- AC: renders in ≤2s P95; widget failures degrade individually; empty states carry guidance.

**FR-5.2 (Should)** As a user, I can personalise Home: reorder, hide/show permitted widgets; layout persists per user.

**FR-5.3 (Should)** As a Team Leader/Ops Manager, my Home includes a team/department work summary (open, overdue, completed-this-week counts with drill-through).

## FR-6 Tasks (My Work)

**FR-6.1 (Must)** As a user, I can view my tasks sorted/filtered by status, due date and priority.

**FR-6.2 (Must)** As a Team Leader, I can create and assign tasks (title, description, assignee, due date, priority, optional links/knowledge references) within my scope.
- AC: assignee notified; full state history retained; `task.created`/`task.assigned` events.
- BR: assignment scope = assigner's team/department attributes; only assigner, assignee, or scope-superior roles can edit; statuses: Open → In Progress → Done (+ Blocked flag, Escalated flag); completed tasks immutable except comments.

**FR-6.3 (Must)** As a user, I can update status and comment on my tasks.

**FR-6.4 (Must)** As a user, I can create a personal task or an escalation to my Team Leader.

**FR-6.5 (Should)** Synced external tasks (Planner) appear in My Work with source badge and deep link; read-only in MVP unless write-back enabled (FR-10).

## FR-7 Knowledge

**FR-7.1 (Must)** As a knowledge manager (Ops Manager/Admin), I can create and publish articles (rich text, title, summary, category, audience attributes, attachments).
- AC: lifecycle Created → Published → Reviewed → Archived; only Published is visible to consumers/AI; every version retained and viewable; events audited.
- BR: publishing requires `knowledge.publish` permission; audience attributes (role/department/team) drive ABAC visibility; review date required at publish (default 6 months); overdue review flags the article to its owner.

**FR-7.2 (Must)** As a user, I can browse knowledge by category and read articles I'm permitted to see.
- AC: permission-filtered everywhere (browse, search, AI); article view logs a usage event.

**FR-7.3 (Must)** As a user, I can flag an article as outdated, creating a review task for the owner.

**FR-7.4 (Should)** As a knowledge manager, I can bulk-import content (Markdown/Word) and register SharePoint links as external knowledge references.

## FR-8 Quick Links

**FR-8.1 (Must)** As an admin, I can curate org- and role-scoped quick links (name, icon, URL, audience).
**FR-8.2 (Must)** As a user, I can pin/reorder personal quick links alongside curated ones.
- AC: click-throughs logged (tool-switching metric); links open in new tab; "report broken link" routes a task to admins.

## FR-9 Announcements & Notifications

**FR-9.1 (Must)** As a publisher (Team Leader: team; Ops Manager: dept/org; Admin: org), I can publish announcements with title, body, audience and optional acknowledgement requirement.
- AC: audience resolution via ABAC attributes; unread-first on Home; acknowledgement-required announcements track per-user acknowledgement with timestamp.

**FR-9.2 (Must)** As a publisher, I can see read/acknowledgement status for my announcements (counts + user list within my scope).

**FR-9.3 (Must)** As a user, I receive in-app notifications for: task assigned/reassigned, task completed (assigner), announcement published to me, knowledge review due (owner), integration failure (admin).
- BR: notification fan-out is driven by the internal event log (Bible §16); email digests are Could-have.

## FR-10 Integrations (Foundation)

**FR-10.1 (Must)** As an admin, I can connect, configure, monitor and disconnect connectors through a uniform framework (auth, config, field mapping, sync rules, error handling, audit).
- AC: connector states Healthy/Degraded/Unhealthy/Disconnected; sync runs produce item-level reports; secrets encrypted at rest and never returned by any API; all lifecycle changes audited.

**FR-10.2 (Must)** Microsoft Planner connector: scheduled + manual sync of assigned tasks into My Work (read-first; write-back behind a flag, Could).

**FR-10.3 (Should)** SharePoint connector: register document libraries/pages as external knowledge references discoverable in search.

**FR-10.4 (Won't — MVP)** Jira connector (fast-follow Release 1.x, per P4-02); telephony/CRM connectors.

## FR-11 Global Search

**FR-11.1 (Must)** As a user, I can search knowledge, tasks, announcements and quick links from one box, with type-grouped, permission-filtered results.
- AC: PostgreSQL FTS; P95 < 1s on pilot-scale data; zero-result state offers assistant (R2) and knowledge-gap flag; search events logged (success metric).
- BR: results never include items the user couldn't open directly; external knowledge references searchable by registered title/metadata only.

## FR-12 AI Assistant (Release 2)

**FR-12.1 (Must)** As a user, I can ask the assistant questions answered via RAG over published, permission-visible knowledge of my organisation only, with citations.
- AC: approved RAG flow (Bible §12) implemented exactly; every answer carries ≥1 citation or an explicit "no source found" response; provider-agnostic abstraction; per-org enable/disable flag; org-level provider/model configuration.
- BR: no destructive/autonomous actions; no cross-tenant retrieval (enforced at retrieval query level, tested in CI); embeddings stored in pgvector with organisation scope.

**FR-12.2 (Must)** Every interaction logs prompt, response, retrieved sources, user, organisation, provider and model to the AI audit trail.

**FR-12.3 (Should)** As a user, I can rate answers 👍/👎 (with optional comment) feeding the usefulness metric.

**FR-12.4 (Should)** Safe summaries: summarise an article or my open tasks on request.

## FR-13 Audit Logging

**FR-13.1 (Must)** The platform writes append-only audit events for: logins, permission/user/team changes, task changes, knowledge changes, integration changes, AI prompts/responses/sources, exports, admin and configuration actions.
- AC: audit entries immutable from UI and application APIs; include actor, org, timestamp, action, entity, before/after summary where applicable.

**FR-13.2 (Must)** As an Admin/Auditor, I can filter audit logs (date, actor, action type, entity) and export to CSV (export itself audited).

## FR-14 Administration & Configuration

**FR-14.1 (Must)** As an admin, I can configure branding (logo, colours, product display name) and navigation (rename/hide modules) within a validated configuration schema.
- BR: configuration is versioned data, never code (Bible §20); invalid configs rejected, not applied.

**FR-14.2 (Must)** As an admin, I can enable/disable modules per organisation via feature flags (e.g. Assistant off until Release 2 enablement checklist passes).

**FR-14.3 (Should)** Guided onboarding checklist with progress tracking (journey J8).

**FR-14.4 (Must — platform-side)** Serene AI operators can provision a new organisation (org record, initial admin, default config) via a controlled provisioning routine, fully audited.

## Dependency map

| Requirement area | Depends on |
|---|---|
| FR-2..4 (users/teams/roles) | FR-1 (auth), org provisioning (FR-14.4) |
| FR-5 (Home) | FR-6, FR-7, FR-8, FR-9 widgets |
| FR-6 (tasks) | FR-3 (team scope), FR-9 (notifications) |
| FR-7 (knowledge) | FR-4 (ABAC attributes) |
| FR-10 (integrations) | FR-13 (audit), secret storage |
| FR-11 (search) | FR-6, FR-7, FR-8, FR-9 content + permission filters |
| FR-12 (assistant) | FR-7 (published knowledge), FR-11 (fallbacks), FR-13 (AI audit), pgvector |
| FR-13 (audit) | event log foundation — built in Release 0 |
