# Phase 4 — User Journey Mapping

> Each required journey is mapped as: Trigger → Steps → Decisions → Success state → Failure
> state. Failure states are product requirements: every one must have a designed experience,
> not a dead end.

## J1. Login journey

- **Persona:** all
- **Trigger:** start of shift / session expiry.
- **Steps:** open CCOS URL (customer-branded) → enter email/password → (if MFA enabled) second factor → land on role-appropriate Home.
- **Decisions:** forgotten password → self-service reset via email; deactivated account → contact-admin screen; first login → guided personalisation prompt (optional, skippable).
- **Success:** authenticated, on Home, in <10 seconds; `auth.login` audit event written.
- **Failure:** wrong credentials (rate-limited, generic error, no account enumeration); reset email not received (resend + admin contact path); session expiry mid-work returns user to the same screen after re-auth.

## J2. Dashboard (Home) journey

- **Persona:** all (role variants)
- **Trigger:** login or "Home" navigation.
- **Steps:** Home renders widgets — announcements (unread first), my open tasks (due-date order), pinned knowledge, quick links, assistant entry point. Team Leader/Ops variants add team/department work summary.
- **Decisions:** personalise layout (add/remove/reorder permitted widgets); dismiss vs open announcements; click-through to any module.
- **Success:** user reaches their first real action (task, article, link) in ≤2 clicks from login.
- **Failure:** a widget's data source is unavailable → widget shows inline degraded state ("Tasks temporarily unavailable — retry"), never blocks the rest of Home; empty states carry next-step guidance ("No tasks — here's how work gets assigned").

## J3. Task management journey

- **Personas:** Agent (own work), Team Leader (team work)
- **Trigger:** assigned work, follow-up from a customer interaction, or escalation.
- **Steps (Agent):** My Work list (due/priority sort) → open task (description, links, history) → work it → update status (in progress → done) or add comment/blocker → completion logged.
- **Steps (Team Leader):** Team view → create task (title, description, assignee, due date, priority, optional knowledge/link references) → assign → monitor status → reassign or chase overdue.
- **Decisions:** native CCOS task vs synced external task (Planner) — synced tasks show source badge and deep link; status pushed back to source only if connector write-back is enabled, else "update in source" affordance. Escalation = task flagged escalated + assigned to leader.
- **Success:** work item completed with full who/when/what history; assigner sees completion without asking.
- **Failure:** sync conflict (task changed in Planner and CCOS) → last-write-wins with conflict note in task history (MVP) and surfaced in connector health; assignee deactivated → task flagged "unassigned — needs owner" in team view.

## J4. Knowledge journey

- **Personas:** Agent (consume), Ops Manager (govern)
- **Trigger:** mid-call question; policy change; scheduled review.
- **Steps (consume):** search from anywhere (global search or Knowledge Hub) → scan results (title, summary, category, freshness) → open article → follow procedure → optionally flag "needs update".
- **Steps (govern):** create article (title, body, category, audience attributes) → publish → article becomes searchable/AI-retrievable → periodic review prompt → re-confirm or archive.
- **Decisions:** article not found → fallback paths: assistant ask (R2), raise knowledge-gap flag (routes a task to knowledge owner); outdated content found → "flag for review" creates owner task.
- **Success (consume):** correct, current article found in <30s; usage event logged (feeds analytics).
- **Success (govern):** lifecycle states accurate; nothing published is stale beyond its review date without a visible overdue-review flag.
- **Failure:** zero search results → suggested categories + gap-flag prompt; permission-filtered article never appears in results at all (no teasing locked content); archived article reached via old link → "archived" banner with pointer to replacement if set.

## J5. Quick links journey

- **Personas:** all; Admin curates
- **Trigger:** need to reach an external business system (CRM, telephony console, WFM…).
- **Steps:** Admin defines org/role-scoped link sets (name, icon, URL, audience) → user sees curated links on Home → user pins personal links → click opens system in new tab; click event logged (tool-switching metric).
- **Decisions:** role-scoped visibility (agents see agent tools); personal vs curated ordering.
- **Success:** user reaches any routine external system from Home in one click.
- **Failure:** dead link reported via "report link" → task to admin; no links configured → empty state instructs admin (admin view) or explains pending setup (user view).

## J6. Integration journey

- **Persona:** Platform Administrator
- **Trigger:** onboarding or adding a connector (MVP: Microsoft Graph — SharePoint, Planner).
- **Steps:** Admin → Integrations → choose connector → authenticate (OAuth admin consent) → configure scope (which plans/sites) → field mapping defaults shown → choose sync mode (manual/scheduled) → run first sync → review sync report.
- **Decisions:** partial consent → connector enters links-only degraded mode with explicit capability list; sync scope changes → re-sync prompt.
- **Success:** connector status Healthy; external items visible in CCOS with source badges; `integration.connected` and sync events audited.
- **Failure:** auth failure → actionable error (which permission missing, who can grant); sync error → item-level error report, partial results kept, retry affordance; token expiry → connector flagged Unhealthy + admin notification, never silent staleness.

## J7. AI assistant journey (Release 2)

- **Personas:** all (if org has assistant enabled)
- **Trigger:** question typed into assistant panel (or routed from failed search).
- **Steps:** user asks → backend resolves tenant + user permission context → retrieval over *published, permission-visible* knowledge only → answer generated with citations → user opens cited article or rates answer (👍/👎) → full interaction audited (prompt, response, sources, user, org, provider/model).
- **Decisions:** no adequate source → assistant says so and offers search/gap-flag (never fabricates); question out of scope (e.g. asks for an action) → explains MVP capability boundary.
- **Success:** answer with ≥1 verifiable citation; user proceeds without raising a ticket/asking a colleague.
- **Failure:** provider outage → graceful "assistant unavailable, search still works"; org has assistant disabled → entry points hidden entirely; low-confidence retrieval → present sources without synthesised claim ("Here's what I found, I can't confirm an answer").

## J8. Administration journey

- **Persona:** Platform Administrator
- **Trigger:** new customer onboarding; ongoing user lifecycle; configuration change.
- **Steps (onboarding):** org created (by Serene AI provisioning) → admin first login → guided setup checklist: branding → teams → roles review → users (invite/bulk CSV) → quick links → knowledge categories + seed content → announcements welcome post → (optional) connectors → go-live.
- **Steps (ongoing):** add/deactivate users; change roles; adjust navigation/modules; review audit log.
- **Decisions:** deactivation vs deletion (deactivate keeps history; deletion is a governed GDPR-erasure flow); role change → permission diff preview before confirm.
- **Success:** org fully set up via checklist with progress indicator; every admin action audited; no admin action requires Serene AI support intervention.
- **Failure:** bulk import errors → row-level error report, valid rows applied; admin locks self out of a module → second admin or Serene AI support recovery path (documented); config validation rejects breaking values rather than applying them.

---

### Cross-journey requirements

1. Every failure state above maps to a story in the backlog — no unhandled dead ends.
2. Every journey emits its canonical events (Bible §16) for audit and analytics.
3. All journeys operate within one organisation's boundary; cross-tenant access is impossible by construction, not by UI omission.
