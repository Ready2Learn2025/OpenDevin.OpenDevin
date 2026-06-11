# Phase 4 — Non-Functional Requirements (MVP)

> NFRs are testable commitments, not aspirations. Each has a verification method. Targets
> are for MVP/pilot scale (≤5 organisations, ≤500 users/org); V1 revisits them with data.

## 1. Security

| ID | Requirement | Verification |
|---|---|---|
| NFR-S1 | All traffic TLS 1.2+; HSTS enabled | config scan |
| NFR-S2 | Data encrypted at rest (DB volumes, object storage, backups) | infra review |
| NFR-S3 | Passwords argon2id/bcrypt; secrets (connector tokens, API keys) encrypted with KMS-managed keys, never logged, never returned by APIs | code review + secret-leak tests |
| NFR-S4 | Every API endpoint requires authentication + declared permission; default-deny | endpoint inventory test in CI |
| NFR-S5 | RBAC∩ABAC evaluated in one shared server-side engine; no permission logic in frontend | architecture review |
| NFR-S6 | OWASP Top 10 controls: parameterised queries, output encoding, CSRF protection, security headers, dependency scanning in CI | SAST/dep-scan in CI + pre-pilot pentest |
| NFR-S7 | Rate limiting on auth and API endpoints | load test |
| NFR-S8 | Session: httpOnly, Secure, SameSite; idle timeout configurable per org (default 12h); revocation on deactivation immediate | integration tests |

## 2. Tenant isolation

| ID | Requirement | Verification |
|---|---|---|
| NFR-T1 | Every tenant-owned row carries org_id; enforced by a single query-layer mechanism (ORM middleware), not per-handler discipline | code review |
| NFR-T2 | Postgres Row-Level Security enabled as defence-in-depth on tenant tables | migration review |
| NFR-T3 | Automated cross-tenant leak test suite (every list/get/search/AI endpoint probed across two seeded orgs) runs in CI; any failure blocks merge | CI gate from Release 0 |
| NFR-T4 | AI retrieval (FTS + pgvector) carries org_id as a hard filter in the query itself | dedicated tests |
| NFR-T5 | Org export contains only that org's data | export test |

## 3. Performance & capacity (pilot targets)

| ID | Requirement |
|---|---|
| NFR-P1 | Home render ≤2s P95; module navigation ≤1s P95 |
| NFR-P2 | Search ≤1s P95 at 10k articles / 100k tasks per org |
| NFR-P3 | Assistant first token ≤5s P95, full answer ≤15s P95 (provider-dependent; measured and reported per provider) |
| NFR-P4 | API ≤300ms P95 for standard CRUD |
| NFR-P5 | 500 concurrent users/org without degradation (shift start spike: 80% logins within 15 min) |
| NFR-P6 | Scheduled syncs complete within their window and never block interactive traffic |

Verification: k6/Gatling load profile simulating shift start, run before pilot go-live.

## 4. Availability & operability

| ID | Requirement |
|---|---|
| NFR-A1 | SaaS target 99.5% monthly availability for MVP pilots (commercial SLA set at V1) |
| NFR-A2 | RPO ≤24h (daily backups minimum; point-in-time recovery preferred via managed Postgres); RTO ≤4h; restore tested before first pilot |
| NFR-A3 | Health endpoints (`/healthz` liveness, `/readyz` dependencies); structured JSON logs with request + org correlation IDs; error tracking (e.g. Sentry-class) |
| NFR-A4 | Zero-data-loss deploys: migrations forward-compatible, deploys without scheduled downtime |
| NFR-A5 | Provider outage (LLM) degrades assistant only; platform unaffected (graceful AI degradation) |

## 5. Auditability & compliance

| ID | Requirement |
|---|---|
| NFR-C1 | Audit events append-only; no UPDATE/DELETE grants on audit tables to the application role; retention ≥ 12 months online (configurable per org, archival beyond) |
| NFR-C2 | Audit coverage = the FR-13.1 list, verified by an audit-coverage test (action performed → event asserted) |
| NFR-C3 | GDPR: data export per user, erasure workflow (anonymise PII, preserve audit integrity), documented processor/sub-processor list, EU/UK data residency for pilot hosting |
| NFR-C4 | SOC 2 readiness posture from MVP: access control policy, change management via PR + CI, logging/monitoring evidence, vendor list — evidence collected from Release 0 (decision P4-05) |

## 6. AI governance (Release 2 gate)

| ID | Requirement |
|---|---|
| NFR-AI1 | Assistant disabled by default per org; enablement requires checklist: ≥ minimum published-knowledge threshold, admin acknowledgement of AI terms, provider/model selected |
| NFR-AI2 | 100% of AI interactions logged (prompt, response, sources, user, org, provider, model, latency, token counts) |
| NFR-AI3 | Citations present on every synthesised answer or explicit no-source response — enforced in the answer pipeline, not the prompt alone |
| NFR-AI4 | No tool/action invocation capability deployed in MVP assistant — the capability surface is structurally absent, not policy-suppressed |
| NFR-AI5 | Provider-agnostic gateway: switching provider/model is org-level configuration; no provider SDK types leak beyond the gateway |
| NFR-AI6 | Customer knowledge is not used to train models; provider data-retention settings documented per supported provider |

## 7. Upgradeability & deployment

| ID | Requirement |
|---|---|
| NFR-U1 | Single versioned core: every customer org runs the same release; org-level differences are configuration only |
| NFR-U2 | All schema changes via ordered, reversible-where-possible migrations; config schemas versioned with automated config migration |
| NFR-U3 | Docker images + compose definition produced from Release 0 (even though SaaS-first per P4-01), keeping the self-hosted path honest |
| NFR-U4 | Seed/demo dataset script for demos, tests and pilots |

## 8. Quality engineering

| ID | Requirement |
|---|---|
| NFR-Q1 | CI: lint, typecheck, unit + integration tests, tenant-leak suite, dependency scan — green before merge |
| NFR-Q2 | Integration tests run against real Postgres (not mocks) for tenancy/permission paths |
| NFR-Q3 | Critical journeys (J1–J8) covered by automated end-to-end tests before each release exit |
| NFR-Q4 | Accessibility checks (axe-class) in CI for shell + Home + My Work + Knowledge |
