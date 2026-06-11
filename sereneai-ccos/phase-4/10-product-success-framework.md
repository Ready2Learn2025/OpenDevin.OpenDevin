# Phase 4 — Product Success Framework

> Metrics across adoption, engagement, operational performance, AI usage, commercial and
> customer success. Every metric names its data source — if the platform can't measure it,
> it isn't a metric. Instrumentation events ship with the features that generate them.

## 1. Adoption

| Metric | Definition | Target (pilot day 60) | Source |
|---|---|---|---|
| Activation rate | invited users who complete first login | ≥90% | auth events |
| WAU/seat | weekly active users ÷ licensed seats | ≥80% | session events |
| DAU/WAU stickiness | daily ÷ weekly actives | ≥0.6 | session events |
| Admin setup completion | onboarding checklist items done | 100% in ≤2 weeks | checklist state |
| Integration adoption | orgs with ≥1 healthy connector | 100% of pilots | connector status |

## 2. Engagement

| Metric | Definition | Target | Source |
|---|---|---|---|
| Home-first behaviour | sessions where Home is first meaningful screen and user acts within it | ≥70% | navigation events |
| Knowledge search success | search → result opened in same session | ≥85% | search + usage events |
| Zero-result rate | searches returning nothing | ≤10%, trending down | search events |
| Quick-link click-through | external launches via CCOS per user per day | ≥5 (proxy for reduced tool switching, risk R4-03) | link events |
| Announcement reach | required-ack announcements acknowledged within 48h | ≥95% | receipts |
| Personalisation uptake | users who customised Home or pinned links | ≥40% | layout events |

## 3. Operational performance (customer-facing value evidence)

| Metric | Definition | Target | Source |
|---|---|---|---|
| Task completion rate | tasks completed ÷ created (rolling 30d) | ≥85% | task events |
| Task cycle time | created → done median | baseline → 20% improvement | task history |
| Work visibility | team-leader-assigned work flowing through CCOS vs side channels | ≥90% (survey + event triangulation) | task events + pilot survey |
| Knowledge freshness | published articles past review date | ≤5% | review dates |
| Knowledge gap closure | flagged-outdated articles resolved within 14d | ≥80% | flag tasks |
| Onboarding speed (customer's agents) | new-agent time-to-productive (customer-defined) | improvement vs customer baseline | pilot survey |

## 4. AI usage & trust (Release 2+)

| Metric | Definition | Target | Source |
|---|---|---|---|
| Assistant adoption | WAU using assistant ≥1×/week | ≥50% | assistant events |
| Answer usefulness | 👍 rate on rated answers | ≥70% | feedback |
| Citation integrity | answers with citations or explicit no-source | 100% (hard invariant) | AI audit |
| No-source rate | questions with no adequate source | tracked → feeds knowledge gap pipeline | AI audit |
| Deflection | assistant sessions ending without subsequent escalation/task within 30 min | ≥60% | event correlation |
| Governance completeness | AI interactions fully logged | 100% (hard invariant) | audit reconciliation |

## 5. Commercial success

| Metric | Definition | Target (first 12 months) |
|---|---|---|
| Design partners signed | pilots on Release 1+ | 2–3 |
| Pilot → paid conversion | pilots converting to contracts | ≥50% |
| Time-to-live | contract → org live | ≤4 weeks |
| Logo retention | customers renewing | 100% (small base — every logo matters) |
| Expansion readiness | customers with ≥2 modules beyond Home/Tasks in active use, or AI tier enabled | ≥50% |
| Core integrity | customers on customised core code | **0 — hard invariant** |

## 6. Customer success / health

| Metric | Definition | Cadence |
|---|---|---|
| Sponsor health score | structured check-in rating with exec sponsor | monthly |
| Support burden | tickets per org per month, trending down after week 4 | weekly |
| Connector health | % time connectors Healthy | continuous |
| NPS (operational users) | agents + team leaders | day 60 and day 90 |

## 7. Anti-metrics (things we refuse to optimise)

- **Time-in-app for its own sake** — long sessions may mean friction. We pair dwell time with task completion.
- **AI answer volume** — more answers ≠ better; usefulness and citation integrity govern.
- **Feature count shipped** — release exit criteria and adoption govern, not throughput.

## 8. Review cadence and kill/persevere gates

- Weekly: instrumentation dashboard reviewed by product team.
- Pilot day 30 / 60 / 90: formal review against this framework with sponsor.
- **Persevere gate (pilot day 90):** WAU/seat ≥70%, knowledge search success ≥80%, sponsor health green → proceed to second pilot and commercial push. Below threshold → diagnose (adoption vs value vs content problem) before adding any new scope — per Bible risk register, unclear MVP success metrics are a named programme risk; this framework is the mitigation.
