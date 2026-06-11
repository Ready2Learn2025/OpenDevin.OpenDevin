# sereneAI.CCOS — Living Project Bible

> **Version:** 4.0 (Phase 4 working draft)
> **Last updated:** 2026-06-11
> **Status:** Phases 0–3 approved. Phase 4 deliverables drafted (see `phase-4/`). Items marked **PROPOSED** await sponsor approval; everything else is approved baseline.

This document is the single source of truth for the sereneAI.CCOS programme. Every
recommendation, design, plan, prompt, build instruction and deliverable must build on this
baseline. Decisions may be challenged with clear justification, but never silently ignored.

---

## 1. Identity

| Item | Value |
|---|---|
| Project name | sereneAI.CCOS |
| Also known as | Serenade AI, sereneAI Contact Centre Operating System, CCOS |
| Brand owner | Serene AI |
| Product category | **Contact Centre Operating System** |

sereneAI.CCOS is **not** an intranet, portal, dashboard, chatbot, CRM, telephony platform,
workforce management system, QA platform or ticketing tool. It is the **operational layer**
that sits above existing contact centre systems, connects them together, and gives users one
coherent place to work.

## 2. Mission, vision and positioning

**Mission.** Help contact centres eliminate operational fragmentation by unifying people,
processes, knowledge, work, workflows, reporting, integrations and AI in one platform.

**Vision.** Become the operating system for modern contact centres — the central workspace
through which contact centres manage daily operations, knowledge, work, reporting,
integrations and AI-powered assistance. Long-term objective: users spend the majority of
their working day inside sereneAI.CCOS.

**Position.** A commercial Contact Centre Operating System acting as the Operational,
Knowledge, Workflow, Task, Reporting, Search, AI, Integration and Administration hub.

## 3. Core problem statement

Contact centres operate across disconnected systems (telephony, CRM, case management, WFM,
learning, knowledge bases, reporting, portals, collaboration, AI tools, task management,
ticketing, QA). This creates multiple sources of truth, duplicate effort, poor visibility,
inconsistent processes, operational inefficiency, poor employee experience, difficult
reporting, limited AI readiness, weak governance, poor handoffs and reduced confidence in
operational data. **sereneAI.CCOS exists to solve operational fragmentation.**

## 4. Strategic principles (approved)

1. **Integrate First** — connect existing systems (Microsoft ecosystem, Jira, SharePoint,
   Teams, Planner, CRM, telephony, reporting, knowledge repositories) and deliver value fast.
2. **Replace Second** — progressively switch on sereneAI-native modules (e.g. Planner →
   sereneAI Tasks).
3. **Automate Third** — automate only once work, knowledge, processes, roles and permissions
   are properly structured.
4. **Transform Fourth** — AI-driven operational transformation built on trusted operational
   context, never an isolated chatbot.

## 5. Commercial strategy (approved)

- Commercial software product, reusable deployment framework, sellable platform under the
  Serene AI brand; configurable per customer; scalable across contact centre environments.
- A blend of sellable SaaS, reusable deployment framework, and customer-specific isolated
  implementation. Each customer receives their own version of sereneAI.CCOS while benefiting
  from a shared core platform.
- Deployment models: **sereneAI Cloud** (SaaS), **Private Cloud** (dedicated managed),
  **Self-Hosted** (controlled deployment package — versioned, with migrations and upgrade
  tooling; never a bespoke manual install).

### Target market
Contact centres: customer service, financial services, debt collection, utilities, telecoms,
BPOs/outsourcers, internal customer operations teams, regulated operational environments.
Future expansion to broader operational teams; initial focus stays on contact centres.

### Buyer profile
Contact Centre Directors, Operations Directors, Heads of CX, CIOs, CTOs, IT Directors,
Transformation Leads, Digital Operations Leaders, Compliance Leaders, Workforce &
Performance Leaders. The product must be explainable to operational **and** technical buyers.

## 6. User groups (approved)

MVP personas: **Advisors/Agents, Team Leaders, Operations Managers, Executives, Platform
Administrators.** Future role extensions: Learning Teams, Workforce Planning, QA, Compliance,
CX, Business Support, IT Support, Data & MI Teams. All personas operate from the same
platform with role-specific experiences. Full persona definitions: `phase-4/02-user-ecosystem.md`.

## 7. Experience vision (approved)

One operational workspace providing: **My Work** (tasks, assignments, priorities, actions,
follow-ups, escalations, personal workload), **Knowledge** (policies, procedures, guidance,
playbooks, process docs, training, FAQs, standards), **Quick Access** (business systems,
resources, tools, integrated apps, bookmarks, links), **Operational Intelligence**
(performance, risks, alerts, trends, insights, compliance indicators, service health,
bottlenecks), **AI Assistance** (operational support, guidance, recommendations, search and
action assistance, workflow assistance, summaries, decision support).

## 8. Core domains (approved)

Workspace · Work Management · Knowledge · Workflow & Automation · Integrations ·
Reporting & Insights · AI Services · Administration.

## 9. Knowledge strategy (approved)

Knowledge is a strategic organisational asset: trusted source for policies, procedures,
guidance, training, playbooks, process docs, FAQs, business rules, role-based guidance.
Knowledge must be easy to discover/govern/maintain/consume, AI-accessible, permission-aware,
version-controlled and auditable.

Lifecycle (MVP): **Created → Published → Reviewed → Archived.**
Future maturity: Draft → Review → Approved → Published → Scheduled review → Archived.

## 10. Work management strategy (approved)

CCOS is where operational work is coordinated: task assignment/completion, escalations,
operational actions, follow-ups, improvement activities, collaboration, Team Leader and
Agent actions, case-linked and process-linked work. Initially integrate Planner/Jira; later
allow native sereneAI work modules to replace them. Objective: reduce friction, create clear
ownership of work.

## 11. Operational intelligence strategy (approved)

Unified operational picture across customer interactions, case management, complaints,
vulnerability indicators, learning activity, compliance outcomes, operational performance,
workforce activity, task activity, workflow progress, knowledge usage and AI usage.
Objective: complete operational visibility.

## 12. AI strategy (approved)

**AI is not the product. AI is an embedded platform capability.** The platform must deliver
operational value before AI is introduced. Principle: **Operational foundation before AI
transformation.**

### MVP AI scope
Knowledge Q&A, source-backed answers, basic operational guidance, summaries, workflow help,
platform navigation support. **No destructive or autonomous actions.**

### Required AI governance (from MVP)
Prompt/response/source/user/organisation/provider-model logging; permission-aware retrieval;
AI governance controls (enable/disable, provider config, data boundaries).

### RAG (approved flow)
User question → tenant + permission context → knowledge retrieval → relevant sources →
prompt assembly → LLM response → citations + audit log. RAG must be organisation-isolated,
permission-aware, source-backed, auditable, governed, configurable per customer. **AI must
never retrieve data from another organisation.**

### Memory
Approved layers: session memory, user preferences, organisation-approved facts,
task/workflow context, future long-term governed memory. MVP includes only safe foundations
(session context, user preferences). Persistent autonomous AI memory is **not** MVP.

### Future AI
Copilots, workflow assistants, agent framework, proactive risk detection, approved action
execution, cross-system recommendations.

## 13. Data architecture (approved)

Design spine: **Organisation → People → Knowledge → Work → Process → AI.** AI sits on top of
structured operational data, trusted knowledge, defined roles, governed processes and
permission-aware context.

**Core data domains:** Organisation, People, Roles, Permissions, Knowledge, Work, Workflow,
Integrations, Reporting, AI Context, Audit, Configuration, Events, Notifications, Search, Files.

**Database strategy:** PostgreSQL primary operational store; tenant-aware model with
Organisation ID on every tenant-owned record; object storage for files; PostgreSQL full-text
search and pgvector for MVP search/vector needs; audit/event tables for traceability.

## 14. Multi-tenancy and isolation (approved)

Shared core platform, many customer deployments, configuration-driven setup, strong tenant
isolation, upgradeable environments. MVP SaaS: shared database with tenant-isolated
(organisation-scoped) rows. Larger customers may later need dedicated schema/database,
private cloud, or fully self-hosted. Per-customer separability: branding, authentication,
users, permissions, configuration, infrastructure, database, Microsoft/Jira tenants,
content, knowledge, reporting, AI configuration.

## 15. Security model (approved)

**RBAC + ABAC.** Role-based permissions enhanced with organisational attributes
(organisation, department, team, role, location, business unit, employment type, access
level, data sensitivity, integration permissions). Security designed in from the start:
authentication, authorisation, tenant isolation, audit logging, encryption in transit and at
rest, secure API access, least privilege, admin controls, AI usage logging, integration
secret protection.

**Authentication:** MVP — email/password, secure sessions, password reset, MFA-ready design.
V1 — Microsoft Entra ID, SSO, SAML/OIDC, customer IdP support, MFA enforcement.

**Audit logging (required from MVP):** login events, permission/user/task/knowledge/
integration changes, AI prompts/responses/source retrieval, data exports, admin actions,
configuration changes. Audit logs are not editable from the UI.

## 16. Architecture principles and Phase 3 decisions (approved)

Platform must be: modular, configurable, extensible, secure, scalable, upgradeable,
multi-tenant capable, API-first, AI-ready, integration-first, cloud-deployable,
self-hostable, observable, auditable, event-ready, vendor-agnostic where practical.
**Avoid unnecessary complexity. Prefer simplicity.**

### Architecture style
**Modular monolith for MVP — not microservices.** Internally modular so services can be
separated later. The headline Phase 3 decision: *Start with a modular monolith and
PostgreSQL. Avoid microservices, Kubernetes and heavy event infrastructure until the product
has proven value.*

### Backend modules
Identity, Organisation, User & Team, Permission, Task, Knowledge, Integration, AI, Audit,
Notification, Dashboard, Configuration.

### Frontend
Modular web application: shell app + dashboard, task, knowledge, assistant, integration and
admin modules + shared UI components. **No security-critical business logic in the
frontend** — permissions, tenancy and workflow rules are enforced by the backend.

### API
API-first. Initial areas: `/api/v1/auth`, `/orgs`, `/users`, `/teams`, `/tasks`,
`/knowledge`, `/integrations`, `/events`, `/assistant`, `/audit`, `/admin`. APIs are
versioned, tenant-aware, permission-controlled, auditable, rate-limit ready, secure by default.

### Events
MVP: internal event log, database-backed events, simple handlers. Future: queues, workers,
event bus, automation engine, agent-triggered workflows. Canonical event types include
`user.created`, `user.role_updated`, `task.created`, `task.assigned`, `task.completed`,
`knowledge.created`, `knowledge.updated`, `integration.connected`,
`integration.sync_started`, `integration.sync_completed`, `assistant.response_generated`,
`assistant.rag_source_used`.

### Service catalogue (begin as monolith modules)
Identity, Organisation, User, Permission, Task, Knowledge, Dashboard, Integration, Sync,
Audit, Notification, AI Assistant, AI Governance, Reporting.

### Integration architecture
Connector framework is central. Connector structure: authentication, configuration, field
mapping, sync rules, webhook support, error handling, audit logging. Sync models: manual,
scheduled, event/webhook. MVP supports manual + scheduled sync and limited webhook
foundations. Target connectors: CRM, telephony, workforce tools, Planner, SharePoint, Teams,
Outlook, Jira, knowledge bases, BI tools, case management, future contact centre platforms.

### Build vs integrate
**Build:** workspace, search, assistant, knowledge experience, task experience, workflow
engine, automation engine, notification centre, reporting experience, configuration layer,
administration layer.
**Integrate:** identity providers, CRM, telephony, workforce, learning, external knowledge,
existing task tools (Jira, Planner), Teams, SharePoint, Outlook, Power BI, case management.

### Native module strategy
| Existing tool | sereneAI replacement |
|---|---|
| Microsoft Planner | sereneAI Tasks |
| SharePoint Knowledge | sereneAI Knowledge Hub |
| External dashboards | sereneAI Reporting & Insights |
| External workflow tools | sereneAI Workflow Engine |

## 17. MVP technical stack (approved recommendation)

| Layer | Recommendation |
|---|---|
| Frontend | React / Next.js |
| Backend | Node.js / NestJS (or equivalent structured backend) |
| Database | PostgreSQL |
| ORM | Prisma or Drizzle |
| Auth | Built-in auth first, SSO-ready |
| Search | PostgreSQL full-text search initially |
| Vector store | pgvector initially |
| File storage | S3-compatible object storage |
| Queue | Redis/BullMQ only when needed |
| Deployment | Docker containers |
| SaaS hosting | Cloud platform with managed PostgreSQL |
| Self-hosted | Docker Compose initially |
| Observability | Structured logs, health checks, error tracking |
| AI provider | Provider-agnostic abstraction layer |

No Kubernetes, microservices or full event bus for MVP without clear justification.

## 18. MVP definition (approved)

**The MVP solves operational fragmentation. It is not an AI transformation project.**

**Included:** Unified Workspace, Knowledge Access, Quick Links, Tasks & Actions, Search,
Announcements, User Personalisation, Assistant Foundations, Administration, Integration
Foundations, Customer Configuration Foundations, Audit Logging, Tenant Isolation, Basic AI
Governance.

**Excluded:** Native CRM, native telephony, native WFM, native QA, advanced AI agents, full
automation engine, full marketplace, complex workflow orchestration, autonomous AI actions,
enterprise event bus, microservices.

**MVP modules:** Home, My Work, Knowledge Hub, Global Search, sereneAI Assistant,
Announcements, Administration, Integrations Foundation, Configuration Foundation.
Detailed specs: `phase-4/05-module-specifications.md`.

## 19. Release strategy (approved)

- **Release 0 — Foundation:** platform setup, authentication, organisation model, users,
  roles, permissions, core UI shell, basic admin, audit foundation.
- **Release 1 — Operational Hub MVP:** dashboard, tasks, knowledge, quick links,
  announcements, search, basic integrations, user personalisation.
- **Release 2 — AI Enabled Operations:** assistant foundation, RAG over approved knowledge,
  AI source logging, prompt/response audit, AI governance settings, safe summaries and guidance.

Detailed plan with exit criteria: `phase-4/09-mvp-release-plan.md`.

**V1 modules (approved):** Workflow Engine, Reporting, Notification Centre, Native Task
Management, Enhanced Knowledge Management, Role-specific Dashboards, SSO, Improved Connector
Framework, Queue-based Processing, Better AI Governance.

**V2 modules (approved):** Automation Engine, Copilot Framework, Advanced Search, Advanced
Reporting, Marketplace Foundations, AI Governance Console, Connector Marketplace, Agent
Framework, Event Bus, Advanced ABAC, Enterprise Deployment Tooling.

## 20. Configuration and upgrade principles (approved)

**Configuration before customisation.** Customers configure branding, navigation,
dashboards, roles, permissions, workflows, integrations, AI settings, knowledge sources,
feature flags and modules — never modify core platform code.

**One core platform, many customer deployments.** All customers remain upgradeable from a
shared core codebase; configuration must not create upgrade blockers.

---

## 21. Decision Register

### Approved (Phases 0–3)
| # | Decision |
|---|---|
| D-01 | Product renamed to sereneAI.CCOS |
| D-02 | Product category is Contact Centre Operating System |
| D-03 | Sold commercially under Serene AI |
| D-04 | Primary market is contact centres |
| D-05 | Core problem is operational fragmentation |
| D-06 | Platform is a reusable deployment framework |
| D-07 | Supports SaaS, private cloud and self-hosted |
| D-08 | Each customer requires strong isolation |
| D-09 | Integrate existing systems first |
| D-10 | sereneAI-native modules introduced progressively |
| D-11 | AI is an embedded capability, not the product |
| D-12 | Operational foundation before AI transformation |
| D-13 | One shared core platform, many customer deployments |
| D-14 | Customers configure, never change core code |
| D-15 | MVP = workspace, knowledge, tasks, search, announcements, administration, assistant foundations |
| D-16 | MVP architecture is a modular monolith |
| D-17 | PostgreSQL is the primary operational database |
| D-18 | pgvector may be used for MVP vector search |
| D-19 | APIs must be versioned and tenant-aware |
| D-20 | Audit logging required from MVP |
| D-21 | AI governance required from MVP |
| D-22 | Advanced agents are not MVP |
| D-23 | Full automation engine is not MVP |
| D-24 | Native CRM, telephony, WFM and QA excluded from MVP |

### Proposed in Phase 4 (await approval)
| # | Proposed decision | Rationale |
|---|---|---|
| P4-01 | **Build sereneAI Cloud (SaaS) first**; ship the self-hosted Docker Compose package only after the first paying SaaS customer | Fastest route to market proof; one production environment to operate; the Docker-first stack keeps self-hosted viable later without carrying its support burden now |
| P4-02 | **Mandatory MVP integrations: Microsoft Graph family only** — Entra-ready auth design, SharePoint (knowledge source links), Planner (task sync read-first). Jira is a fast-follow (Release 1.x), not MVP-blocking | One auth scheme (Graph/OAuth) yields three connectors; matches assumption that Microsoft ecosystem matters most; halves connector surface for MVP |
| P4-03 | **First AI capability: permission-aware Knowledge Q&A with citations** (Release 2) | Lowest-risk, highest-trust AI; directly exercises the RAG + governance architecture; demo-able to buyers |
| P4-04 | **First native replacement module: sereneAI Tasks** (V1, per approved V1 list) | My Work is the daily-habit surface; native tasks remove the most friction and prove Replace Second |
| P4-05 | **First compliance targets: GDPR (UK/EU) from day one + SOC 2 Type II readiness posture**; ISO 27001 later | Target market is UK/EU-leaning and regulated; SOC 2 evidence collection must start early to be certifiable later |
| P4-06 | **Data residency rule for MVP: store, don't mirror.** CCOS stores what it owns (users, tasks, knowledge, config, audit, AI logs) and stores only *references + minimal display metadata* for integrated records (e.g. Planner task id/title/status) — never bulk-copies of external system data | Bounds compliance exposure, keeps sync simple, answers the "store vs reference" open question |
| P4-07 | **Pricing model recommendation: per-active-user/month, three tiers (Workspace / Workspace+AI / Enterprise)** with deployment model as an Enterprise-tier attribute | Aligns price to seats (contact centres think in seats); AI as a tier protects AI cost exposure. Commercial decision — needs sponsor sign-off |
| P4-08 | **MVP admin model: single "Org Admin" role + read-only "Auditor" role**, with custom role builder deferred to V1 | Minimum viable administration; avoids shipping a half-built RBAC editor |
| P4-09 | **Ideal first customer profile: UK mid-size regulated contact centre (50–500 seats), Microsoft-ecosystem, with an operational transformation sponsor** | Big enough to feel fragmentation pain, small enough to deploy fast; matches integration choices |
| P4-10 | Phase 4 deliverable set accepted as the product definition baseline (this Bible v4.0 + `phase-4/` documents) | Converts strategy into buildable plan |

## 22. Risk Register

Carried risks (Phases 0–3): scope expansion; integration complexity; AI governance; upgrade
management; security and compliance; customer configuration drift; building too many native
modules too early; competing head-on with Microsoft/Jira/ServiceNow; self-hosted support
burden; customer-specific requirements weakening the shared core; AI before knowledge/process
maturity; weak tenant isolation; overbuilt event architecture; premature microservices;
uncontrolled AI actions; poor audit coverage; unclear MVP success metrics.

New risks identified in Phase 4:

| # | Risk | Mitigation |
|---|---|---|
| R4-01 | Microsoft Graph API permission consent friction at customer IT departments delays pilots | Publish a one-page admin-consent guide; design connectors to degrade gracefully (links-only mode) when consent is partial |
| R4-02 | Knowledge Hub launches empty → workspace feels hollow → adoption stalls | Onboarding includes a content-seeding playbook and bulk import (Markdown/Word/SharePoint links) in Release 1 |
| R4-03 | "Reduced tool switching" success metric is hard to measure directly | Use proxy metrics: quick-link click-throughs, in-app dwell time, search success rate (defined in success framework) |
| R4-04 | AI answer quality on thin/immature customer knowledge bases damages trust in the whole platform | Assistant ships behind a per-org feature flag; enablement checklist requires minimum published-knowledge threshold; "I don't have a source for that" is a designed first-class response |
| R4-05 | Shared-row tenancy bug = catastrophic cross-tenant leak | Org scoping enforced in one place (ORM middleware/query layer), Postgres RLS as defence-in-depth, automated cross-tenant leak tests in CI from Release 0 |
| R4-06 | Per-customer branding/navigation config grows into de-facto customisation | Configuration schema is versioned and validated; anything not expressible in the schema is a product gap, not a customer patch |

## 23. Opportunity Register (new in Phase 4)

| # | Opportunity |
|---|---|
| O4-01 | Audit log + AI governance is itself a sales feature for regulated buyers — surface it in demos, not just in compliance docs |
| O4-02 | Knowledge usage analytics (what agents search and fail to find) is an early, cheap differentiator for Ops Managers |
| O4-03 | The connector framework's config schema can become the V2 Connector Marketplace foundation with no rework if designed declaratively now |
| O4-04 | Announcements + read-acknowledgement gives compliance teams evidence trails (e.g. "all agents acknowledged policy change") — small feature, large regulated-buyer value |

## 24. Assumptions Register

Carried from Phases 0–3 (unchanged): contact centres value an operational layer; gradual
adoption preferred; Microsoft ecosystem integration important; Jira matters for some teams;
self-hosted needed by larger/regulated orgs; AI adoption depends on trust/governance/data
readiness; native modules become attractive once the workspace is in daily use; product must
serve operational and technical buyers; modular monolith is the right MVP balance;
PostgreSQL sufficient for first data model; AI must be vendor-agnostic.

Added in Phase 4:
- A 50–500 seat contact centre can be onboarded (org, users, branding, knowledge seed, one
  integration) in under two weeks of elapsed time.
- Agents will tolerate one new tool only if it demonstrably removes visits to at least two
  existing tools.
- Pilot customers will accept email/password auth for the pilot if Entra SSO is on the V1
  roadmap in writing.

## 25. Open Questions

Resolved by Phase 4 proposals (pending sign-off): first deployment model (P4-01), mandatory
MVP integrations (P4-02), first AI capability (P4-03), first native replacement module
(P4-04), first compliance standards (P4-05), store-vs-reference (P4-06), pricing model
(P4-07), minimum viable administration (P4-08), first ideal customer profile (P4-09),
minimum viable connector framework (`phase-4/05-module-specifications.md` §8), MVP reporting
(usage + operational counters only — see scope definition), AI governance level for pilots
(`phase-4/08-non-functional-requirements.md` §AI).

Still open:
- How much configuration latitude before upgrades become unsafe? (Needs a configuration
  compatibility policy — recommend drafting in Release 0.)
- What minimum audit evidence will enterprise customers expect? (Validate P4-05 evidence
  list with first enterprise prospect.)
- Commercial sign-off on pricing tiers and price points (P4-07 sets the model, not numbers).

## 26. Phase log

| Phase | Outcome |
|---|---|
| 0 | Discovery / platform vision workshop |
| 1 | Business strategy & product vision |
| 2 | Capabilities, domain & information architecture, MVP definition, commercial architecture |
| 3 | Solution architecture & technical blueprint (modular monolith + PostgreSQL headline decision) |
| 4 (current) | Product vision model, user ecosystem, journeys, functional requirements, module specs, UX architecture, scope, NFRs, release plan, success framework, backlog — drafted in `phase-4/` |

## 27. Maintenance rules for this Bible

1. The Bible changes only via the Decision Register: proposals enter as **PROPOSED**, move
   to approved with sponsor sign-off, and the affected sections are then updated.
2. Version bumps: minor (4.x) for register updates, major (5.0) when a phase completes.
3. Phase deliverable documents are subordinate to the Bible; on conflict the Bible wins and
   the deliverable must be corrected.
4. Every risk accepted, decision reversed or scope change must be visible in the registers —
   no silent edits.
