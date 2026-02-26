# Engineering Execution Operating System - Decision Log

![decisions](https://img.shields.io/badge/decisions-log-2ECDA7?style=flat-square)

## Overview

This log captures key decisions about how the Kamiwaza engineering team executes work, manages product requirements, and enforces quality gates. These decisions emerged from a Slack discussion in February 2026 involving Krti (initiative lead), Daniel (engineering leadership), Matt (CTO-level), and John (engineering leadership).

## Decisions

### Decision 001: Automation Beats Process

**Status:** Accepted
**Date:** February 2026
**Participants:** Matt (proposed), John, Daniel, Krti

#### Context

The v1 of the execution operating system deck included templates, checklists, and process documentation layers. The team debated whether to enforce behavior through documentation or tooling.

#### Decision

Tooling and automation take precedence over documentation and process. If you want to enforce behavior, build it into the system (CI auto-reviewers, Linear bots, automated checks) rather than writing process documents.

#### Consequences

- Process documentation should be minimal
- Enforcement mechanisms should be automated wherever possible
- Investment in tooling infrastructure is prioritized
- Manual checklists are considered a last resort

#### Rationale

Process design is a failure to automate. Teams that rely on documentation and checklists experience drift, inconsistent adoption, and enforcement burden. Automated systems provide consistent, non-negotiable enforcement.

---

### Decision 002: No New Process Mid-Cycle

**Status:** Accepted
**Date:** February 2026
**Participants:** Daniel (proposed), team consensus

#### Context

Team is mid-cycle on the D150 release. Risk of disruption from introducing new process changes during active development.

#### Decision

Stick with currently adopted practices through D150:
- Definition of Done fields in Linear
- Milestone tracking
- Short tech design notes

Do not introduce new process changes until D180 cycle begins.

#### Consequences

- Stability during D150 execution
- New automation initiatives target D180 or later
- Existing practices remain in place without modification

---

### Decision 003: Linear Gatekeeping Automation for D180

**Status:** Proposed
**Date:** February 2026
**Participants:** Krti (proposed), pending final alignment

#### Context

Multiple friction points identified in the development workflow. Team needed to select one automation to ship for D180 cycle.

#### Decision

Build Linear automation that enforces requirements before state transitions:
- Block 'In Progress' state unless Definition of Done fields are populated
- Require mock/contract link for interface/contract work
- Bot auto-generates stub design note when ticket enters 'In Progress'

#### Alternatives Considered

1. Automated PRD generation tool
2. Comprehensive UAT expansion (determined to be independent initiative)
3. Branch scaffolding automation from tickets

#### Consequences

- Reduced incomplete work entering development
- Consistent design documentation for architectural changes
- Enforcement at workflow level, not code review time
- Engineers blocked from starting work without proper setup

---

### Decision 004: Customer Surrogate Owns Intent, Project Lead Owns PRD

**Status:** Accepted
**Date:** February 2026
**Participants:** Daniel, John (converged)

#### Context

Debate about responsibility for requirements definition. Daniel advocated for engineers to own PRDs. John raised concern that engineers build internally-coherent solutions that miss customer needs.

#### Decision

Responsibility split:
- **Customer surrogate** (Probst, Preston, field engineers, sales) owns the intent and customer signal
- **Project lead** owns the PRD artifact and is responsible for negotiating feasibility with engineering
- Engineers wear "product hat" when writing PRD, "engineering hat" when consuming it

#### Consequences

- Customer signal remains grounded in real needs
- Project leads act as translators between customer intent and engineering feasibility
- Engineers participate in product definition but don't exclusively own it
- Clear accountability for customer alignment

#### Rationale

Engineers naturally build internally-coherent systems. Without customer surrogate ownership of intent, solutions risk being technically sound but misaligned with customer problems. Project leads bridge this gap by negotiating feasible solutions that address real customer needs.

---

### Decision 005: UAT Expansion is Independent Initiative

**Status:** Accepted
**Date:** February 2026
**Participants:** Matt (identified), John (endorsed)

#### Context

Matt identified authentication/authorization, installers, and model engine as top regression areas. Need for expanded automated user acceptance testing.

#### Decision

Expanding automated UAT is a separate, parallel initiative:
- Extend beyond SDK smoke tests to Playwright browser flows
- Nick is already working on automated CD infrastructure
- Not either/or with Linear gatekeeping automation
- High ROI independent of other process improvements

#### Consequences

- Multiple automation initiatives proceed in parallel
- UAT expansion does not compete with Linear gatekeeping for D180
- Focus on highest-regression areas: authnz, installers, model engine
- Integration with Nick's CD work

#### Rationale

UAT expansion addresses different problems than workflow enforcement. Both initiatives provide value independently and should proceed in parallel rather than competing for prioritization.

---

## Revision History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | February 2026 | Initial decision log created |
