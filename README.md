<p align="center">
  <img src="assets/banner.svg" alt="Execution Operating System" width="900"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-v2_draft-2ECDA7?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/cycle-D150_scope-2ECDA7?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMiIgaGVpZ2h0PSIxMiI+PGNpcmNsZSBjeD0iNiIgY3k9IjYiIHI9IjUiIGZpbGw9IiMyRUNEQTciLz48L3N2Zz4=" alt="D150"/>
  <img src="https://img.shields.io/badge/target-D180_automation-00B98D?style=flat-square" alt="D180"/>
</p>

---

## Why: avoidable churn

We have concrete misses where interface/contract intent was ambiguous (COE tool, simplified interface). Not pervasive, but avoidable.

> **Result** &mdash; plausible builds that were not the customer outcome
>
> **Goal** &mdash; make intent testable earlier so rework is avoidable

---

## Minimal operating model (contract-first)

> Contract-first = UX flow + API contract reviewed together; UI must not force incoherent APIs.

<p align="center">
  <img src="assets/flow-model.svg" alt="Intent → Design → Execution → Release" width="800"/>
</p>

---

## This cycle (D150): lock what is in motion

<table>
<tr>
<td width="40">

![check](https://img.shields.io/badge/✓-2ECDA7?style=flat-square&labelColor=2ECDA7)

</td>
<td>

**Definition of Done** in Linear + milestones + short tech design note

</td>
</tr>
<tr>
<td>

![check](https://img.shields.io/badge/✓-2ECDA7?style=flat-square&labelColor=2ECDA7)

</td>
<td>

**No new process rollout mid-cycle**

</td>
</tr>
<tr>
<td>

![check](https://img.shields.io/badge/✓-2ECDA7?style=flat-square&labelColor=2ECDA7)

</td>
<td>

If sign-off is blocked past date X, **DRI escalates to Daniel or Matt**; decision + rationale recorded in Linear

</td>
</tr>
</table>

---

## D180 automation target: Linear gatekeeping

<table>
<tr>
<td width="40">

![bot](https://img.shields.io/badge/🤖-1a1a2e?style=flat-square&labelColor=1a1a2e)

</td>
<td>

Block **'In Progress'** unless DoD fields + mock/contract link are present for interface/contract work

</td>
</tr>
<tr>
<td>

![bot](https://img.shields.io/badge/🤖-1a1a2e?style=flat-square&labelColor=1a1a2e)

</td>
<td>

Bot can **auto-stub the design note** to reduce friction

</td>
</tr>
<tr>
<td>

![bot](https://img.shields.io/badge/🤖-1a1a2e?style=flat-square&labelColor=1a1a2e)

</td>
<td>

Enforces intent before build &mdash; **automation beats process**

</td>
</tr>
</table>

---

## UAT expansion (highest ROI)

| | |
|---|---|
| **Focus areas** | authnz, installers, model engine |
| **Approach** | Build on existing CD pipeline + SDK smoke tests; expand with Playwright browser flows + regression harness |
| **Outcome** | Fewer regressions without slowing delivery |

---

## Example chain: make intent testable

<p align="center">
  <img src="assets/example-chain.svg" alt="Linear ticket → UX mock + API contract → Build & review → Outcome" width="820"/>
</p>

<p align="center">
  <em>If mock/contract is missing, we build plausible outcomes that are not the customer outcome.</em>
</p>

---

## Repo contents

| | Resource | Description |
|:--|:---------|:------------|
| **[`templates/`](templates/)** | Fill-in-the-blank artifacts | [PRD-lite](templates/prd-lite.md) · [UX mock](templates/ux-mock.md) · [Design note](templates/design-note.md) |
| **[`case-studies/`](case-studies/)** | Process in action | [WorkRoom Implementation](case-studies/workroom-implementation.md) |
| **[`automation/`](automation/)** | D180 build specs | [Linear gatekeeping](automation/linear-gatekeeping.md) |
| **[`decisions/`](decisions/)** | Decision log | [Execution operating system decisions](decisions/001-execution-operating-system.md) |

---

<details>
<summary><strong>Appendix: templates (tiny + specific)</strong></summary>

<br/>

| Artifact | Contents | Template |
|:---------|:---------|:---------|
| **PRD-lite** | problem, user, workflow, success criteria, non-goals, risks | [Use template &rarr;](templates/prd-lite.md) |
| **UX mock** | one screen + user journey description | [Use template &rarr;](templates/ux-mock.md) |
| **Design note** | approach, API/interface impacts, data model impacts, security implications, rollout plan | [Use template &rarr;](templates/design-note.md) |

**What good looks like:**
- Data model impact = entities + migration story
- Security = authz touchpoints + audit fields

</details>

<details>
<summary><strong>Appendix: learning loop</strong></summary>

<br/>

- **Hot wash:** same-day capture of surprises and pain points
- **Retro/COE:** metrics-based root cause by layer
- **Output:** 3-5 corrective actions with DRIs and dates

</details>

<details>
<summary><strong>Appendix: ownership model</strong></summary>

<br/>

| Role | Responsibility |
|:-----|:---------------|
| **Customer surrogate** | Owns intent (Probst, Preston, field engineers) |
| **Project lead** | Owns the PRD artifact |
| **Engineering** | Negotiates feasibility and nonfunctional requirements |

Explicit decision + escalation + log.

</details>

---

<p align="center">
  <sub>krti@kamiwaza.ai · Feb 2026</sub>
</p>
