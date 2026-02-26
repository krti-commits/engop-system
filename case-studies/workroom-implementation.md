# Case Study: WorkRoom Implementation

![case-study](https://img.shields.io/badge/case_study-WorkRoom-2ECDA7?style=flat-square)

**Applying the contract-first operating model to a real D150 feature**

---

## The Feature

WorkRoom is a feature in Kamiwaza's enterprise AI platform that touches both UI and backend API contracts. It's representative of the kind of D150 work that benefits most from the contract-first approach: complex enough to have multiple interpretations, but constrained enough to deliver in a reasonable timeframe.

The feature itself isn't what matters here. What matters is *how it was built*.

## How the Process Worked

### Step 1: Intent (Discovery Conversations)

The team didn't start with a ticket. They started with conversations.

Krti ran multiple discovery sessions with stakeholders to understand what WorkRoom actually needed to do. Not one big kickoff meeting. Not a requirements document dropped over the wall. Multiple iterative conversations that progressively clarified intent.

**Key insight**: The first conversation revealed what people *thought* they wanted. The second and third conversations revealed what they *actually* needed.

### Step 2: Design (Value Streams → Milestones)

Daniel took the output from discovery and translated it into value streams. Then he decomposed those value streams into Linear milestones.

This wasn't a Gantt chart. It was a structured breakdown of *what value gets delivered when*. The milestones existed before any code was written.

**Key insight**: Having milestones in Linear before coding starts changes the engineering conversation. Engineers can see the shape of the work and identify sequencing issues early.

### Step 3: Traceability (Milestones → Requirements → PRD)

Krti mapped the milestones to specific requirements, then to acceptance criteria, then produced a PRD.

This is the "contract" - testable intent that engineers can build against. The PRD wasn't a 50-page spec. It was lightweight, but it was explicit about:
- What success looks like (acceptance criteria)
- What the feature does NOT do (non-goals)
- How the pieces fit together (traceability)

**Key insight**: The PRD forced clarity on edge cases and non-goals that would have otherwise surfaced during code review as "wait, should this also handle X?"

### Step 4: Execution (Engineers Build to Contract)

Engineers received a PRD with clear acceptance criteria, not an ambiguous ticket that said "implement WorkRoom."

They built to the contract. Code review confirmed alignment with acceptance criteria. No surprises. No "I thought we were building Y but you built Z."

## What the Process Caught

Real examples from the WorkRoom implementation:

1. **Assumption surfacing**: Early conversations revealed different stakeholders had conflicting mental models of how users would interact with the feature. Catching this before coding saved at least one complete rework cycle.

2. **Dependency discovery**: Milestone decomposition exposed that a backend API needed to exist before the UI could be meaningful. The old process would have discovered this during integration. The contract-first process discovered it during planning.

3. **Scope control**: The PRD's explicit non-goals prevented three separate "while we're here, could we also..." requests during implementation.

## Before vs After

### Before (Old Way)
1. Ticket: "Implement WorkRoom"
2. Engineer interprets based on limited context
3. Engineer builds plausible implementation
4. Code review reveals misalignment with stakeholder intent
5. Rework cycle
6. Repeat until convergence

**Result**: Working feature, but 2-3 rework cycles and accumulated team frustration.

### After (Contract-First)
1. Discovery conversations surface intent and assumptions
2. Value streams decomposed into milestones
3. PRD defines acceptance criteria and non-goals
4. Engineer builds to contract
5. Code review confirms alignment with acceptance criteria
6. Stakeholder validation against PRD

**Result**: Working feature, first iteration aligned, team confidence high.

## Lessons Learned

### What worked
- **Multiple small conversations beat one big meeting**: Discovery is iterative, not one-shot.
- **Milestones in Linear before code**: Having the structure visible changes how engineers approach the work.
- **Lightweight PRDs work**: The PRD doesn't have to be heavy. It just has to be explicit.
- **Non-goals prevent scope creep**: Saying what you're NOT building is as important as saying what you are.

### What this means for D180 automation
This is exactly what the D180 process should enforce:

1. Discovery conversations must happen (Intent phase)
2. Milestones must exist in Linear before D150 tickets are created
3. PRD (or equivalent contract) must exist with acceptance criteria
4. Engineers build to contract, not to interpretation

### The real shift
The WorkRoom implementation proved that contract-first isn't about process overhead. It's about *front-loading clarity*. The time spent in discovery and planning doesn't add time to the project. It prevents rework cycles that would have taken longer.

**Bottom line**: This is the operating model. WorkRoom is proof it works.

---

*This case study documents a real implementation at Kamiwaza. The contract-first process reduced rework cycles and increased team alignment. It's a template for how D150 work should be approached.*
