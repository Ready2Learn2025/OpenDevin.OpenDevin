# Phase 4 — User Ecosystem (Personas)

> Baseline: [PROJECT_BIBLE.md](../PROJECT_BIBLE.md) §6. MVP personas are Agent, Team Leader,
> Operations Manager, Executive, Administrator. The remaining personas are **future-aware**:
> defined now so data, permissions and UX don't design them out, but not MVP build targets.

## MVP personas

### 1. Agent (Advisor)

| | |
|---|---|
| Responsibilities | Handle customer interactions; follow procedures; complete assigned tasks and follow-ups; stay current on policy changes |
| Goals | Resolve customers quickly and correctly; know exactly what to do next; avoid being caught out by a policy change |
| Pain points | 6–10 open systems; stale or duplicated knowledge; verbal/Teams-message task assignment with no record; missed announcements |
| Daily activities | Log in at shift start → check Home (announcements, tasks) → handle interactions, searching knowledge mid-call → complete/raise tasks → acknowledge comms |
| Permissions | Read published knowledge (scope-filtered); manage own tasks; create follow-ups/escalations to own team; use assistant; personalise own quick links and dashboard |
| Key screens | Home, My Work, Knowledge article view, Global Search, Assistant panel, Announcements |

### 2. Team Leader

| | |
|---|---|
| Responsibilities | Run a team of 8–15 agents; assign and chase work; cascade communications; first-line escalation; coach against knowledge |
| Goals | Know team workload state at a glance; assign with accountability; evidence that comms landed |
| Pain points | Work assigned via chat/email is invisible and unauditable; no view of who has read what; chasing status manually |
| Daily activities | Review team task board → assign/reassign → publish/forward announcements → handle escalations → check knowledge gaps raised by agents |
| Permissions | All Agent permissions + create/assign/reassign tasks within team; view team task status; create team-scoped announcements; view team read-acknowledgements |
| Key screens | Home (team variant), My Work — Team view, Announcements composer, Knowledge, Search |

### 3. Operations Manager

| | |
|---|---|
| Responsibilities | Multi-team operational performance; process adherence; escalation owner; knowledge owner for operational procedures |
| Goals | One operational picture; find bottlenecks; keep knowledge current; evidence process compliance |
| Pain points | Reporting stitched manually from many tools; no visibility of knowledge usage or task flow across teams; inconsistent processes between teams |
| Daily activities | Review operational dashboard → drill into stuck/overdue work → publish org/department announcements → review knowledge needing review → manage escalations |
| Permissions | Team Leader permissions across assigned department(s) + publish department knowledge; manage knowledge lifecycle (publish/review/archive); view usage analytics |
| Key screens | Home (ops variant), My Work — Department view, Knowledge Hub management, Announcements, Admin (limited: teams/users in own department) |

### 4. Executive Leadership

| | |
|---|---|
| Responsibilities | Strategic oversight; investment decisions; risk and compliance accountability |
| Goals | Trustworthy top-level operational picture; confidence the AI/data foundation is governed |
| Pain points | Conflicting numbers from different systems; no line of sight into operational risk; AI initiatives without governance |
| Daily activities | Weekly review of adoption/operational summary; consume key announcements; occasional drill-down |
| Permissions | Read-only org-wide dashboards and announcements; no admin rights by default |
| Key screens | Home (executive variant), Announcements, (V1) Reports |

### 5. Platform Administrator

| | |
|---|---|
| Responsibilities | Organisation setup; users/teams/roles; branding and navigation configuration; integrations; AI settings; audit review |
| Goals | Onboard the org fast; least-privilege by default; prove control to auditors |
| Pain points | Tools with admin afterthoughts; opaque integration failures; no audit story |
| Daily activities | User lifecycle management; connector health checks; configuration changes; audit log queries |
| Permissions | Full admin within own organisation: users, teams, roles, branding, navigation, modules/feature flags, integrations, AI governance settings, audit log (read-only) |
| Key screens | Administration (all sections), Integrations, Audit log viewer |

MVP also includes a read-only **Auditor** role (proposed decision P4-08): audit log and
configuration read access, nothing else.

## Future personas (design-aware, not MVP)

| Persona | Core need the platform must not design out | Earliest target |
|---|---|---|
| QA Analyst | Sample interactions/tasks against procedures; raise coaching tasks; QA-scoped dashboards | V1–V2 |
| Learning & Development | Author/curate training knowledge; track consumption; assign learning tasks | V1 |
| Workforce Planning | Schedule-aware views; workload vs capacity signals from task data | V2 |
| Data Analyst | Export/query operational data; usage analytics; (V2) reporting builder | V1 (exports), V2 (builder) |
| Compliance Officer | Read-acknowledgement evidence; knowledge version history; AI audit trail | Partially MVP via Auditor role |

Design implications already honoured: role model is data-driven (new roles = configuration,
not code); knowledge metadata includes audience/department attributes for ABAC; task and
knowledge events are logged from MVP so future analytics personas have history.

## Permission matrix (MVP summary)

| Capability | Agent | Team Leader | Ops Manager | Executive | Admin | Auditor |
|---|---|---|---|---|---|---|
| Read published knowledge (scoped) | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| Create/edit knowledge drafts | — | — | ✅ | — | ✅ | — |
| Publish/review/archive knowledge | — | — | ✅ | — | ✅ | — |
| Manage own tasks | ✅ | ✅ | ✅ | — | ✅ | — |
| Assign tasks (team scope) | — | ✅ | ✅ (dept) | — | ✅ | — |
| Create announcements | — | ✅ (team) | ✅ (dept/org) | — | ✅ (org) | — |
| Use assistant | ✅ | ✅ | ✅ | ✅ | ✅ | — |
| Manage users/teams/roles | — | — | limited (own dept) | — | ✅ | — |
| Configure branding/navigation/modules | — | — | — | — | ✅ | — |
| Manage integrations & AI settings | — | — | — | — | ✅ | — |
| Read audit log | — | — | — | — | ✅ | ✅ |

All checks are enforced server-side with org scope (RBAC) plus attribute filters
(department/team/audience — ABAC) per Bible §15.
