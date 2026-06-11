# Phase 4 — UX & Navigation Architecture

> Goal: the product must *feel like a single operational workspace* — clear navigation,
> role-specific views, personalisation, fast access to daily work, low cognitive load,
> mobile-aware, accessible. MVP avoids navigation depth (Bible: "UX & Navigation Direction").

## 1. Layout system

```
┌──────────────────────────────────────────────────────────────┐
│ Top bar: [customer logo] [Global Search……………] [🔔] [👤]      │
├───────────┬──────────────────────────────────────┬───────────┤
│ Side nav  │  Module content area                 │ Assistant │
│  Home     │                                      │ panel     │
│  My Work  │                                      │ (slide-   │
│  Knowledge│                                      │  over,    │
│  Announce.│                                      │  R2)      │
│  ────────│                                      │           │
│  Integr.* │                                      │           │
│  Admin*   │                                      │           │
└───────────┴──────────────────────────────────────┴───────────┘
```

- **Top bar (persistent):** customer branding, global search (focusable with `/`), notification bell, user menu (profile, preferences, logout).
- **Side nav (persistent, collapsible):** maximum 7 primary items for any role; admin-only items separated below a divider. `*` = visible only with permission.
- **Assistant:** slide-over panel, never a separate page — reachable from any screen (R2; hidden when org flag off).
- **Navigation depth rule:** any daily-work destination ≤2 clicks from Home; nothing in MVP deeper than 3 levels (module → list → detail).

## 2. Navigation model (MVP)

| Item | Audience | Notes |
|---|---|---|
| Home | all | role-variant dashboard |
| My Work | all | personal; Team/Department tabs appear by role |
| Knowledge | all | browse + manage tab for knowledge managers |
| Announcements | all | list + composer by permission |
| Search | all | top-bar invocation; results as overlay/page |
| Assistant | all (R2, org-flagged) | slide-over |
| Integrations | Admin | |
| Admin | Admin (limited delegation for Ops) | |

Renaming/hiding modules per customer is configuration (Bible §20). "Reports/Insights" enters
the nav in V1 — MVP operational counters live inside Home widgets to avoid promising a
reporting module that isn't there.

## 3. Role-specific experience

| Role | Home emphasis | Extra surfaces |
|---|---|---|
| Agent | today's tasks, unread/ack-required announcements, pinned knowledge, quick links | — |
| Team Leader | team work summary + overdue first, then own work | Team tab in My Work; announcement composer; receipts view |
| Ops Manager | department roll-up, knowledge-review-due, announcement reach | Department tab; Knowledge manage tab; limited Admin |
| Executive | adoption/operational summary widgets, org announcements | read-only |
| Admin | onboarding checklist (until complete), connector health, recent audit | Admin, Integrations |

Defaults are admin-configurable per role (dashboard defaults, §Module 1); users personalise
within permitted widgets.

## 4. Key interaction principles

1. **Start-of-shift test:** login → know what to do today, within 10 seconds, zero clicks.
2. **Mid-call test:** find a procedure in <30s without leaving context — global search opens as overlay, article opens in slide-over when invoked from search, preserving the screen behind.
3. **Evidence by default:** actions that create accountability (assign, publish, acknowledge) always show who/when affordances.
4. **Degrade, never block:** widget/connector/AI failures degrade that surface inline; the workspace itself never white-screens.
5. **Empty states teach:** every empty list explains how it gets filled and offers the permitted action.
6. **Source badges:** anything synced from an external system is visibly badged (Planner, SharePoint) with deep link — honesty about where data lives builds trust in Integrate First.
7. **AI humility:** assistant answers always show citations, the model disclosure, and a rate control; "no source" is a designed, respectable answer.

## 5. Personalisation (MVP boundaries)

Users may: reorder/hide permitted Home widgets; pin quick links; pin knowledge articles; set
notification preferences (in-app granularity). Users may not: change navigation, create
widgets, or alter anything affecting other users. Everything else is admin configuration.

## 6. Mobile-aware behaviour

MVP is responsive web, not native apps. Priorities on small screens: Home (announcements +
tasks), task status updates, knowledge reading, acknowledgements. Admin and Integrations are
desktop-optimised only (functional but not tuned). Native/PWA decision deferred to V1
with usage data.

## 7. Accessibility

- WCAG 2.2 AA target for all MVP user-facing surfaces.
- Full keyboard navigation; visible focus; `/` to search, `g h / g w / g k` quick-nav (documented, optional).
- Customer branding colour palettes validated for contrast at config time (Configuration Foundation rejects failing palettes) — accessibility cannot be configured away.
- Screen-reader landmarks per module; live-region announcements for notifications.

## 8. Visual language

- Calm, operational, dense-but-legible: this is an 8-hour-a-day work surface, not a marketing site. Neutral base + customer accent colour; status semantics (overdue/escalated/blocked) consistent across all modules.
- Shared component library (shell-owned) from day one — Bible's frontend architecture: shell app + module packages + shared UI components. No module ships bespoke variants of core components.

## 9. UX debt explicitly accepted for MVP

- Single default theme (light) — dark mode V1.
- Admin screens are functional CRUD-grade, not delight-polished.
- No in-app product tours beyond the onboarding checklist and empty states.
- Localisation architecture (string externalisation) in place from R0, but English-only shipped.
