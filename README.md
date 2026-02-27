<p align="center">
  <img src="assets/banner.svg" alt="Execution Operating System" width="900"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-v1-2ECDA7?style=flat-square" alt="Status"/>
  <img src="https://img.shields.io/badge/type-framework-2ECDA7?style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMiIgaGVpZ2h0PSIxMiI+PGNpcmNsZSBjeD0iNiIgY3k9IjYiIHI9IjUiIGZpbGw9IiMyRUNEQTciLz48L3N2Zz4=" alt="Framework"/>
  <img src="https://img.shields.io/badge/approach-contract_first-00B98D?style=flat-square" alt="Contract First"/>
</p>

---

## Why: avoidable churn

Teams often build features where the interface or contract intent was ambiguous. Common examples:

- We built a dashboard that exposed raw data, but users needed actionable insights
- We created a powerful API with 20 endpoints, but the UI workflow only needed 3 simple operations
- We implemented feature X, but the customer actually needed feature Y because the underlying intent wasn't explicit

Not pervasive, but **completely avoidable**.

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

## Current cycle: lock what is in motion

<table>
<tr>
<td width="40">

![check](https://img.shields.io/badge/✓-2ECDA7?style=flat-square&labelColor=2ECDA7)

</td>
<td>

**Definition of Done** in tickets + milestones + short tech design note

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

If sign-off is blocked past agreed date, **DRI escalates to project lead**; decision + rationale recorded in ticket system

</td>
</tr>
</table>

---

## Automation target: ticket system gatekeeping

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
| **Focus areas** | authentication/authorization, installers, core engines |
| **Approach** | Build on existing CD pipeline + SDK smoke tests; expand with browser automation flows + regression harness |
| **Outcome** | Fewer regressions without slowing delivery |

---

## Example chain: make intent testable

<p align="center">
  <img src="assets/example-chain.svg" alt="Ticket → UX mock + API contract → Build & review → Outcome" width="820"/>
</p>

<p align="center">
  <em>If mock/contract is missing, we build plausible outcomes that are not the customer outcome.</em>
</p>

---

## Repo contents

| | Resource | Description |
|:--|:---------|:------------|
| **[`templates/`](templates/)** | Fill-in-the-blank artifacts | [PRD-lite](templates/prd-lite.md) · [UX mock](templates/ux-mock.md) · [Design note](templates/design-note.md) |
| **[`case-studies/`](case-studies/)** | Process in action | Real-world implementation examples |
| **[`automation/`](automation/)** | Automation specs | Ticket system gatekeeping and workflow automation |
| **[`decisions/`](decisions/)** | Decision log | Execution operating system decisions and rationale |

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
| **Customer surrogate** | Owns intent (product owners, field engineers, customer champions) |
| **Project lead** | Owns the PRD artifact and cross-functional coordination |
| **Engineering team** | Negotiates feasibility and nonfunctional requirements |

Explicit decision + escalation + log.

</details>

---
