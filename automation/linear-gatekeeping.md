![automation](https://img.shields.io/badge/automation-issue_tracker_gatekeeping-2ECDA7?style=flat-square) ![status](https://img.shields.io/badge/status-blueprint-blue?style=flat-square)

# Issue Tracker Gatekeeping Automation - Blueprint

## Problem

Interface and contract work frequently starts without testable intent defined upfront. Engineers move tickets to "In Progress" without acceptance criteria, design documentation, or linked UX/API contracts. The result is technically sound builds that miss customer intent and require rework after implementation is complete.

**Cost of late discovery:**
- Wasted engineering time building the wrong thing
- Design rework after implementation
- Customer frustration with missed expectations
- Extended delivery timelines

## Solution: State Transition Enforcement

Enforce required fields and artifacts before allowing issue state transitions. When a ticket is moved to "In Progress" AND it meets trigger criteria (see Scope below), the automation enforces required fields before allowing the transition.

### Required Fields

**Core requirements:**
- Definition of Done (DoD)
- Acceptance Criteria
- Required links: UX mock OR API contract (can be a link to a file, design tool, or inline description)

**Teams should customize based on their needs. Additional examples:**
- Test strategy or test plan link
- Architecture decision record (ADR) reference
- Security/privacy review status
- Performance requirements
- Accessibility requirements

### Bot Behavior

**When fields are missing:**
- Block the state transition
- Post a comment explaining what's needed
- Provide template links or examples
- Optionally notify in team chat (Slack, Teams, Discord)

**When fields are present:**
- Allow transition to "In Progress"
- Auto-create helpful scaffolding (see Auto-Stub section below)

## Scope: What Triggers the Gate

The gatekeeping applies ONLY to tickets with specific labels/tags. This keeps friction targeted and avoids slowing down non-interface development work.

### Example Trigger Labels

**Reference implementation (adapt to your team):**
- `interface` - User-facing changes
- `contract` - API/service contract changes
- `ux-facing` - Frontend/UI work
- `breaking-change` - Backward-incompatible changes
- `architecture` - Significant architectural decisions

**Explicitly excluded from gating:**
- Bug fixes (unless they change interfaces)
- Chores and maintenance work
- Internal-only technical work
- Documentation updates
- Dependency updates

**Customization guidance:**
- Start with 2-3 trigger labels to avoid over-gating
- Review quarterly: which work types cause most rework?
- Consider severity tiers (strict for breaking changes, lighter for enhancements)

## Auto-Stub Behavior

When the bot allows transition to "In Progress", it can optionally auto-generate helpful scaffolding to reduce setup friction.

### Example Scaffolding Actions

1. **Feature branch creation** (if not already automated)
   - Create branch with consistent naming: `feature/{ticket-slug}` or `{team-prefix}/{ticket-id}`
   - Base branch: typically `main`, `develop`, or custom per team

2. **Documentation folder scaffolding**
   - Create directory: `docs/features/{ticket-slug}/` or similar structure
   - Copy templates from canonical location (e.g., `docs/templates/`)
   - Pre-fill ticket metadata: title, description, acceptance criteria
   - Generate stubs: `design-note.md`, `prd-lite.md`, `test-plan.md`, etc.

3. **Draft pull request creation**
   - Create draft PR linking back to issue
   - Pre-fill PR description with ticket context
   - Add checklist derived from acceptance criteria
   - Link to scaffolded documentation

**Important:** Scaffolding should feel like helpful automation, not bureaucratic overhead. If engineers find it burdensome, simplify or make it opt-in.

## Implementation Approach

### Architecture Options

**Option 1: Webhook listener service (recommended for most teams)**
- Lightweight service (Node.js, Python, Go) hosted on your infrastructure
- Listen for issue tracker webhooks (e.g., Linear "issue.update", Jira "issue_updated")
- Validate state transitions and enforce requirements
- Pros: Full control, easy to customize, works offline
- Cons: Requires hosting and maintenance

**Option 2: GitHub Actions (for GitHub-centric teams)**
- Trigger on issue tracker webhook → GitHub Actions workflow
- Use Actions to validate and scaffold
- Pros: No separate hosting, integrates with existing CI/CD
- Cons: Limited to GitHub ecosystem, slower execution

**Option 3: Serverless functions (AWS Lambda, Vercel, Cloudflare Workers)**
- Webhook → serverless function → validation and scaffolding
- Pros: Minimal infrastructure, scales automatically
- Cons: Cold start latency, vendor lock-in

### Reference Implementation: Linear Webhooks

This example uses Linear as the issue tracker, but the pattern adapts to Jira, Shortcut, GitHub Issues, GitLab Issues, or custom trackers.

**Webhook payload structure (Linear example):**
```json
{
  "action": "update",
  "data": {
    "id": "issue-123",
    "title": "Add user profile editing",
    "state": {
      "name": "In Progress"
    },
    "labels": [
      {"name": "interface"}
    ],
    "description": "...",
    "customFields": {
      "definition_of_done": "...",
      "acceptance_criteria": "..."
    }
  },
  "updatedFrom": {
    "state": {
      "name": "Todo"
    }
  }
}
```

**Adapting to other trackers:**
- **Jira:** Use "issue_updated" webhook, check `changelog` for status transition
- **GitHub Issues:** Use "issues" webhook with "edited" or "labeled" action
- **GitLab Issues:** Use "Issue Hook" webhook
- **Shortcut:** Use "story-update" webhook

### Phased Rollout

**Phase 1: Validation + Comment (enforce gate)**
- Listen for issue state change webhooks → "In Progress"
- Check for trigger labels (e.g., `interface`, `contract`)
- Validate required fields and links
- If validation fails:
  - Use API to revert status to previous state
  - Post explanatory comment with template links and examples
  - Optionally notify in team chat channel
- If validation succeeds:
  - Allow transition (no action needed)

**Phase 2: Auto-Scaffolding (reduce friction)**
- On successful validation, trigger scaffolding automation
- Create feature branch (if not already automated)
- Scaffold documentation directory structure
- Copy and customize templates with ticket metadata
- Create draft PR via API with ticket link in description
- Post success comment with links to scaffolded resources

**Phase 3: Completion Gate (close the loop)**
- Bot checks if acceptance criteria are met before allowing transition to "Done"
- Validates that PR exists and is linked to ticket
- Confirms design documentation is present (if required)
- Optional: Check that PR has passing CI, code review approval, etc.

## Success Criteria

1. **Zero gated tickets reach "In Progress" without required fields**
   - Measure: Audit issue transitions weekly
   - Target: 100% compliance after 2-week adoption period

2. **Engineers report net positive experience**
   - Measure: Survey after 1 month
   - Target: >70% report scaffolding saves time

3. **Measurable reduction in rework due to missing requirements**
   - Measure: Track PR rewrite rate (substantive changes after initial implementation)
   - Target: 30% reduction in rework PRs after 2 months

4. **Reduced time from "requirements unclear" to "clarified"**
   - Measure: Time between "In Progress" and first meaningful commit
   - Target: Decrease by 25% (faster clarification at gate)

5. **Consistent documentation presence**
   - Measure: % of completed interface/contract work with design documentation
   - Target: >90% compliance

## Flow Diagram

```
Ticket → State Change to "In Progress"
    │
    ├── Has trigger label? (e.g., interface, contract, ux-facing)
    │       │
    │       ├── NO → Allow transition (no gate)
    │       │
    │       └── YES → Validate required fields
    │               │
    │               ├── Fields MISSING → Block + comment with guidance
    │               │                    Revert status to previous state
    │               │                    Notify in team chat (optional)
    │               │
    │               └── Fields PRESENT → Allow + auto-scaffold (Phase 2)
    │                       │
    │                       ├── Create feature branch (if not automated)
    │                       │
    │                       ├── Scaffold documentation directory
    │                       │   ├── design-note.md (from template)
    │                       │   ├── prd-lite.md (from template)
    │                       │   ├── test-plan.md (from template)
    │                       │   └── acceptance-criteria.md (from ticket)
    │                       │
    │                       └── Create draft PR
    │                           ├── Link to issue
    │                           ├── Pre-filled description
    │                           └── Checklist from acceptance criteria
```

## Open Questions for Your Team

1. **Field configuration:** What exact fields should you require? (Custom fields vs description sections)
2. **Label taxonomy:** What labels trigger the gate? (Start narrow, expand based on data)
3. **Notification channels:** Should the bot post in chat (Slack/Teams/Discord) as well as issue comments?
4. **Owner assignment:** Who builds and maintains this automation? (DevOps, platform team, infrastructure?)
5. **Template location:** Where do you store canonical templates? (In repo, wiki, separate docs repo?)
6. **Completion gate:** Should Phase 3 block "Done" transitions or just warn? (Block for higher quality, warn for flexibility)
7. **Exemption mechanism:** How do engineers request exemption for edge cases? (Override label, manual approval, time-based bypass?)
8. **Rollout strategy:** Pilot with one team first? Gradual label expansion? Hard cutover?

## Technical Implementation Notes

### Issue Tracker API Access

**Linear:**
- Requires Linear API key with issue read/write permissions
- Webhooks deliver to configured endpoint (requires public URL or tunnel for dev)
- [Linear API docs](https://developers.linear.app/docs/graphql/working-with-the-graphql-api)

**Jira:**
- Requires Jira API token (user or app-based)
- Configure webhooks in Jira admin panel
- [Jira webhook docs](https://developer.atlassian.com/server/jira/platform/webhooks/)

**GitHub Issues:**
- Requires GitHub App or Personal Access Token with repo write permissions
- Configure webhooks at repo or org level
- [GitHub webhooks docs](https://docs.github.com/en/webhooks)

### Version Control API Access

**GitHub:**
- Requires GitHub App or PAT with repo write permissions
- Must handle branch creation, file commits, and PR creation
- [GitHub REST API](https://docs.github.com/en/rest)

**GitLab:**
- Requires GitLab API token with API scope
- Similar branch and merge request operations
- [GitLab API docs](https://docs.gitlab.com/ee/api/)

### Template System

**Template variables to support:**
```
{{ticket.id}}          - Issue identifier
{{ticket.title}}       - Issue title
{{ticket.description}} - Full description
{{ticket.url}}         - Link to issue
{{ticket.labels}}      - Comma-separated labels
{{ticket.assignee}}    - Assigned engineer
{{ticket.milestone}}   - Sprint/milestone name
{{ticket.dod}}         - Definition of Done
{{ticket.acceptance}}  - Acceptance criteria
{{date}}               - Current date
{{branch}}             - Feature branch name
```

**Template engine options:**
- Mustache (simple, logic-less)
- Handlebars (extends Mustache with helpers)
- Jinja2 (powerful, Python ecosystem)
- Simple string replacement (`str.replace()` in a loop)

**Recommendation:** Start with simple string replacement. Graduate to template engine if complexity grows.

### Error Handling

**Bot must gracefully handle:**
- API rate limits (both issue tracker and VCS)
- Network failures and timeouts
- Malformed webhook payloads
- Concurrent state changes (race conditions)
- Missing permissions or authentication failures

**Best practices:**
- Retry transient failures with exponential backoff
- Log all actions for debugging and audit
- Post user-friendly error messages in ticket comments
- Alert engineering team for repeated failures
- Implement circuit breakers for cascading failures

### Security Considerations

- Store API tokens in secrets manager (not environment variables or code)
- Validate webhook signatures to prevent spoofing
- Implement rate limiting to prevent abuse
- Restrict bot permissions to minimum required scope
- Audit bot actions for security review
- Rotate API tokens periodically

## Customization Examples

### Example 1: Strict Gate for Breaking Changes

```python
def should_block_transition(ticket):
    """Block breaking changes without architecture review."""
    if "breaking-change" in ticket.labels:
        return not (
            ticket.has_field("architecture_review_link") and
            ticket.has_field("migration_plan") and
            ticket.has_field("backward_compatibility_strategy")
        )
    return False
```

### Example 2: Graduated Requirements by Ticket Size

```python
def get_required_fields(ticket):
    """Require more documentation for larger tickets."""
    if ticket.story_points >= 8:
        return ["dod", "acceptance_criteria", "design_doc", "test_plan"]
    elif ticket.story_points >= 3:
        return ["dod", "acceptance_criteria", "design_doc"]
    else:
        return ["dod", "acceptance_criteria"]
```

### Example 3: Team-Specific Gates

```python
def get_gate_config(ticket):
    """Different teams have different requirements."""
    team = ticket.team

    if team == "platform":
        return {"required": ["dod", "adr_link"], "optional": ["perf_requirements"]}
    elif team == "frontend":
        return {"required": ["dod", "figma_link"], "optional": ["a11y_checklist"]}
    elif team == "api":
        return {"required": ["dod", "api_spec"], "optional": ["breaking_change_flag"]}

    return {"required": ["dod"], "optional": []}
```

## Measuring Impact

### Metrics to Track

**Process metrics:**
- Number of blocked transitions per week
- Time to resolve blocked state (add required fields)
- % of tickets that pass gate on first attempt
- Exemption request frequency

**Quality metrics:**
- PR rework rate (major changes after initial implementation)
- Bug escape rate for gated vs non-gated work
- Time from "In Progress" to first commit (should decrease)
- Documentation completeness at PR time

**Developer experience metrics:**
- Survey: "Gating helps me deliver better work" (Likert scale)
- Survey: "Scaffolding saves me time" (Likert scale)
- Support requests related to gating
- Adoption rate across teams

### Review Cadence

- **Weekly:** Review blocked transitions and exemption requests
- **Monthly:** Review metrics dashboard and adjust thresholds
- **Quarterly:** Survey team and decide on expansions or relaxations
- **Annually:** Comprehensive retrospective on ROI

## Getting Started Checklist

- [ ] Choose issue tracker integration point (Linear, Jira, GitHub, etc.)
- [ ] Define trigger labels for your team
- [ ] Identify required fields (start with DoD + acceptance criteria)
- [ ] Set up webhook listener infrastructure
- [ ] Implement Phase 1 (validation + block)
- [ ] Create template documentation and examples
- [ ] Pilot with one team for 2 weeks
- [ ] Gather feedback and iterate
- [ ] Roll out to full engineering team
- [ ] Implement Phase 2 (auto-scaffolding) if Phase 1 succeeds
- [ ] Monitor metrics and adjust

## Related Blueprints

- **Automated UAT expansion:** Separate initiative for regression testing
- **PRD generation automation:** Upstream tool for requirements documentation
- **Branch scaffolding:** Can be combined with this automation in Phase 2
- **CI/CD improvements:** Downstream integration for Phase 3 completion gates
