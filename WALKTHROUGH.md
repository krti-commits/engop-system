![walkthrough](https://img.shields.io/badge/walkthrough-presenter_notes-2ECDA7?style=flat-square)

# Walkthrough Script: Execution Operating System

**Duration:** ~10 min (adjustable)
**Format:** Screen-share the GitHub repo, scroll through README, click into supporting docs
**Audience:** Engineering team + leadership

---

## Setup

- Open https://github.com/krti-commits/engop-system in browser
- Have the README visible at the top (the banner should be showing)
- Keep this script open in a separate window or on your phone

---

## PART 1: The Problem (2 min)

> **[Show: README banner + "Why: avoidable churn" section]**

"Quick context on what this is and why it exists."

"We've had specific misses where the intent at the interface or contract level was ambiguous. COE tool, simplified interface -- these are real examples. Not a systemic failure, but a pattern worth addressing."

"The result: we build things that are *plausible* -- they look right, they work -- but they're not what the customer actually needed. That's avoidable churn."

"The goal here is simple: make intent testable *before* we build, so rework becomes the exception, not the norm."

---

## PART 2: The Model (2 min)

> **[Show: "Minimal operating model" section with the flow diagram SVG]**

"So here's the operating model. Four layers. Nothing novel -- Intent, Design, Execution, Release."

"The key idea is *contract-first*. That means UX flow and API contract get reviewed together before build starts. Matt made the point that when UI drives API design, you end up with incoherent APIs. This avoids that -- they're designed as a unit."

> **[Show: "This cycle (D150)" section]**

"For D150 -- this cycle -- we're not changing anything. Daniel's right that you don't introduce process mid-cycle. We stick with what we've already adopted: Definition of Done in Linear, milestones, short tech design notes."

"One thing I did tighten from v1: the escalation path. If sign-off is blocked, the DRI escalates to Daniel or Matt. Decision gets recorded in Linear. No 'silence is agreement.'"

---

## PART 3: The D180 Play (2 min)

> **[Show: "D180 automation target" section]**

"Here's where it gets interesting. For D180, we ship *one* automation. Not a new process -- a bot."

"The idea: when someone moves a ticket to 'In Progress' and it's tagged as interface or contract work, the bot checks -- do we have DoD? Acceptance criteria? A mock or API contract link? If not, it blocks the transition and tells you what's missing."

"If the fields *are* there, the bot auto-stubs a design note and scaffolds the feature folder. Automation beats process -- that's straight from Matt's feedback."

> **[Click into: automation/linear-gatekeeping.md]**

"I've written out the full spec for this. Three phases: Phase 1 is validation and blocking. Phase 2 is auto-scaffolding -- branch, folder, draft PR. Phase 3 closes the loop at the 'Done' transition."

> **[Scroll to the flow diagram in the spec]**

"Here's the decision tree. Bug fixes and chores are *not* gated -- this only applies to interface and contract work. Targeted friction, not blanket overhead."

> **[Navigate back to README]**

---

## PART 4: UAT (1 min)

> **[Show: "UAT expansion" section]**

"Separate from the Linear gatekeeping, and equally high-ROI: UAT expansion. Matt called out the top three regression areas -- authnz, installers, model engine. Nick is already building the CD pipeline. We expand from SDK smoke tests into Playwright browser flows."

"These are independent tracks. Both ship. Not either/or."

---

## PART 5: The Example (1 min)

> **[Show: "Example chain" section with the SVG diagram]**

"Here's the chain in practice. Linear ticket with DoD and milestones, to UX mock plus API contract, to build and review, to outcome that matches customer intent."

"The bottom line: if the mock or contract is missing, we build plausible things that aren't the customer thing. This chain prevents that."

---

## PART 6: Show the Supporting Docs (2 min)

> **[Show: "Repo contents" table]**

"This isn't just a deck. The repo has four supporting sections."

> **[Click into: templates/prd-lite.md]**

"First -- usable templates. Here's the PRD-lite. Problem statement, target user, current vs proposed workflow, success criteria, non-goals, risks. There's a filled-in example at the bottom -- 'One-Click Model Deployment' -- so you can see what good looks like."

> **[Navigate back, click into: templates/design-note.md]**

"Same for the design note. Approach, API impacts, data model impacts -- and we define what those mean. Data model impact equals entities plus migration story. Security equals authz touchpoints plus audit fields. No ambiguity."

> **[Navigate back, click into: case-studies/workroom-implementation.md]**

"And here's proof this works. The WorkRoom implementation ran this exact process during D150. Multiple discovery conversations, Daniel's milestones, traceability mapping into a PRD. It caught assumption conflicts, surfaced a dependency we would've hit during integration, and prevented three scope-creep requests."

"This isn't theoretical. We already did it."

> **[Navigate back, click into: decisions/001-execution-operating-system.md]**

"Finally -- every decision that shaped this is logged. Five decisions from our Slack thread. Automation beats process, no mid-cycle changes, Linear gatekeeping for D180, ownership model, UAT as independent track. ADR-style, with context and rationale."

---

## PART 7: Close + Discussion (1 min)

> **[Navigate back to README top]**

"So to recap:"

"D150: we don't change anything. Stay the course with DoD, milestones, design notes."

"D180: we ship the Linear gatekeeping bot. Automation, not process. The spec is written, it needs a DRI and a build plan."

"In parallel: UAT expansion on the top three regression areas."

"The repo is the reference. Templates are ready to use today. The automation spec is ready to build from."

"What questions do you have?"

---

## Likely Questions + Answers

**"Who's building the Linear bot?"**
> "That's one of the open questions. I'm proposing myself as DRI. Happy to discuss."

**"Isn't this adding overhead?"**
> "For non-interface work -- zero overhead, it's not gated. For interface work -- the overhead is filling in fields you should already be thinking about. And the bot auto-stubs the design note, so it's actually saving setup time."

**"What about Matt's point that prompting frameworks are higher ROI?"**
> "That's complementary, not competing. Better prompts help engineers work with AI tools. Contract-first helps ensure they're building the right thing. Both matter."

**"Can we just use this for D150 work right now?"**
> "The templates -- yes, use them today. The automation -- that's D180. But nothing stops you from copying the PRD-lite template for your current work."

**"How does this relate to John's engineering-internal docs repo?"**
> "The templates and feature folders would live there. This repo is the operating system reference. John's repo is where the artifacts land."

---

*Adjust pacing based on engagement. If the room is nodding, move faster through Parts 1-2 and spend more time in Parts 5-6 (the concrete stuff). If there's pushback, lean on the case study and the decision log -- they show this was shaped by the team's own feedback, not handed down.*
