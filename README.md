# Execution Operating System

**Intent → Contract → Delivery**

D150 scope | D180 automation

---

## Why: avoidable churn

We have concrete misses where interface/contract intent was ambiguous (COE tool, simplified interface). Not pervasive, but avoidable.

**Result:** plausible builds that were not the customer outcome.

**Goal:** make intent testable earlier so rework is avoidable.

---

## Minimal operating model (contract-first)

Contract-first = UX flow + API contract reviewed together; UI must not force incoherent APIs.

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│              │     │              │     │              │     │              │
│  1. Intent   │────▶│  2. Design   │────▶│ 3. Execution │────▶│  4. Release  │
│              │     │              │     │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
```

---

## This cycle (D150): lock what is in motion

- **Definition of Done** in Linear + milestones + short tech design note
- **No new process rollout mid-cycle**
- If sign-off is blocked past date X, DRI escalates to Daniel or Matt; decision + rationale recorded in Linear

---

## D180 automation target: Linear gatekeeping

- Block **'In Progress'** unless DoD fields + mock/contract link are present for interface/contract work
- Bot can auto-stub the design note to reduce friction
- Enforces intent before build; **automation beats process**

---

## UAT expansion (highest ROI)

- **Focus areas:** authnz, installers, model engine
- **Approach:** build on existing CD pipeline + SDK smoke tests; expand with Playwright browser flows + regression harness
- **Outcome:** fewer regressions without slowing delivery

---

## Example chain: make intent testable

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────────┐
│ Linear ticket│     │  UX mock +   │     │    Build     │     │     Outcome      │
│ (DoD +       │────▶│ API contract │────▶│   & review   │────▶│ (customer intent) │
│  milestones) │     │              │     │              │     │                  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────────┘
```

> If mock/contract is missing, we build plausible outcomes that are not the customer outcome.

---

## Appendix: templates (tiny + specific)

| Artifact | Contents |
|----------|----------|
| **PRD-lite** | problem, user, workflow, success criteria, non-goals, risks |
| **UX mock** | one screen + user journey description |
| **Design note** | approach, API/interface impacts, data model impacts, security implications, rollout plan |

**What good looks like:**
- Data model impact = entities + migration story
- Security = authz touchpoints + audit fields

---

## Appendix: learning loop

- **Hot wash:** same-day capture of surprises and pain points
- **Retro/COE:** metrics-based root cause by layer
- **Output:** 3-5 corrective actions with DRIs and dates

---

## Ownership model (reference)

- **Customer surrogate** owns intent (Probst, Preston, field engineers)
- **Project lead** owns the PRD artifact
- **Engineering** negotiates feasibility and nonfunctional requirements
- Explicit decision + escalation + log

---

*krti@kamiwaza.ai | Feb 2026*
