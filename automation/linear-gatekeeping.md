![automation](https://img.shields.io/badge/automation-Linear_gatekeeping-2ECDA7?style=flat-square) ![status](https://img.shields.io/badge/status-proposed-yellow?style=flat-square)

# D180 Automation: Linear Gatekeeping

## Problem

Interface and contract work frequently starts without testable intent defined upfront. Engineers move tickets to "In Progress" without Definition of Done, acceptance criteria, or linked UX/API contracts. The result is plausible builds that miss customer intent and require rework after implementation is complete.

## Solution: State Transition Enforcement

When a ticket is moved to "In Progress" AND it is tagged as interface/contract work, the Linear bot enforces required fields before allowing the transition:

**Required fields:**
- Definition of Done (DoD)
- Acceptance Criteria

**Required links:**
- UX mock OR API contract (can be a link to a file, Figma design, or inline description)

**Bot behavior:**
- **Fields missing:** Block the state transition, post a comment explaining what's needed, provide template links
- **Fields present:** Allow transition and auto-create:
  - Feature branch from the ticket (already exists today via LinearBot)
  - Scaffolded folder: `docs-internal/features/{ticket-slug}/` with stub files (prd-lite.md, design-note.md)
  - Draft pull request linking back to the Linear ticket

## Scope: What Triggers the Gate

The gatekeeping applies ONLY to tickets with specific labels:
- `interface`
- `contract`
- `ux-facing`

**Explicitly excluded from gating:**
- Bug fixes
- Chores
- Internal-only technical work

This keeps friction targeted and avoids slowing down non-UX development work.

## Auto-Stub Behavior

When the bot scaffolds the feature folder, it performs the following actions:

1. Copies templates from `docs-internal/templates/` directory
2. Pre-fills ticket title, description, and linked milestones in template files
3. Creates `design-note.md` with the ticket's acceptance criteria as section headers
4. Generates `prd-lite.md` with ticket context pre-populated
5. Commits scaffolding to the feature branch created by LinearBot

## Implementation Approach

**Architecture:**
- Linear webhooks → lightweight service (Node.js or Python)
- Can be implemented as a GitHub Action triggered by Linear webhook, or as a standalone bot service
- Service validates ticket state transitions and enforces contract-first requirements

**Phased rollout:**

**Phase 1: Validation + Comment (block + explain)**
- Listen for Linear "issue.update" webhooks when status changes to "In Progress"
- Check for required labels, fields, and links
- If validation fails: use Linear API to revert status and post explanatory comment
- Comment includes template links and examples of properly configured tickets

**Phase 2: Auto-Scaffolding (branch + folder + draft PR)**
- On successful validation, trigger scaffolding automation
- Create `docs-internal/features/{ticket-slug}/` directory structure
- Copy and customize templates with ticket metadata
- Create draft PR via GitHub API with Linear ticket link in description

**Phase 3: Close the Loop (completion gate)**
- Bot checks if acceptance criteria are met before allowing transition to "Done"
- Validates that PR exists and is linked to ticket
- Confirms design documentation is present in feature folder

## Success Criteria

1. Zero interface/contract tickets reach "In Progress" without DoD + mock/contract
2. Engineers report the scaffolding saves setup time (net positive, not friction)
3. Measurable reduction in "plausible but wrong" builds for interface work
4. Decreased rework cycles due to missing requirements discovered after implementation
5. Feature documentation consistently present for all interface/contract work

## Flow Diagram

```
Ticket → "In Progress"
    │
    ├── Has interface/contract label?
    │       │
    │       ├── NO → Allow transition (no gate)
    │       │
    │       └── YES → Check required fields
    │               │
    │               ├── Fields MISSING → Block + comment with template links
    │               │                    Revert status to previous state
    │               │
    │               └── Fields PRESENT → Allow + auto-scaffold
    │                       │
    │                       ├── Create feature branch (via existing LinearBot)
    │                       ├── Scaffold docs-internal/features/{slug}/ directory
    │                       │   ├── prd-lite.md (from template)
    │                       │   ├── design-note.md (from template)
    │                       │   └── acceptance-criteria.md (from ticket)
    │                       │
    │                       └── Create draft PR
    │                           ├── Link to Linear ticket
    │                           ├── Pre-filled description
    │                           └── Checklist from acceptance criteria
```

## Open Questions

1. **Linear configuration:** What exact field names/custom fields should we require? (DoD might be a custom field or part of description)
2. **Label taxonomy:** Should we standardize on `interface`, `contract`, `ux-facing` or use different labels?
3. **Notification channel:** Should the bot post in Slack as well as Linear comments?
4. **DRI assignment:** Who owns building and maintaining this automation?
5. **Template location:** Where do we store the canonical templates? (docs-internal/templates/?)
6. **Completion gate:** Should Phase 3 block "Done" transitions or just warn?
7. **Exemption mechanism:** How do engineers request exemption for edge cases?

## Technical Notes

**Linear API access:**
- Requires Linear API key with issue read/write permissions
- Webhooks deliver to configured endpoint (requires public URL or ngrok for dev)

**GitHub API access:**
- Requires GitHub App or Personal Access Token with repo write permissions
- Must handle branch creation, file commits, and PR creation

**Template system:**
- Templates should support variable substitution: `{{ticket.title}}`, `{{ticket.description}}`, etc.
- Consider using Mustache, Handlebars, or simple string replacement

**Error handling:**
- Bot must gracefully handle API failures (Linear or GitHub)
- Should retry transient failures
- Must log all actions for debugging

## References

- Matt's philosophy: "process design is a failure to automate" and "we build things to do it for us"
- John's suggestion: "Bot creates branches from tickets, blocks state transitions without required fields, scaffolds feature folders"
- Existing LinearBot behavior: Creates branches automatically (e.g., `feature/Context-Service-D150-MVP`)
