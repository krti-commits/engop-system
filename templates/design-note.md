![template](https://img.shields.io/badge/template-design_note-2ECDA7?style=flat-square)

# Design Note: [Feature Name]

<!--
PURPOSE: Document the technical approach BEFORE coding starts.
AUDIENCE: Engineers, architects, security reviewers.
GOAL: Answer "how will we build this?" in 1-2 pages. Focus on impacts and decisions.
-->

## Approach Summary
<!--
Describe the technical approach in 1-2 paragraphs.
- What's the high-level architecture?
- What components are involved?
- Why this approach over alternatives?

Example (see bottom for full example):
We'll add a new POST /serving/auto-config endpoint that analyzes model metadata
(format, size) and cluster resources (CPU/GPU availability) to return a recommended
engine and resource allocation. The UI calls this before showing the quick deploy modal.
-->


## API / Interface Impacts

### New Endpoints
<!-- List new endpoints. Format: METHOD /path - purpose -->

### Changed Endpoints
<!-- List modified endpoints. What's changing and why? -->

### Breaking Changes
<!-- Any breaking changes to existing APIs? How will we handle backward compatibility? -->


## Data Model Impacts

### New Entities
<!-- Any new database tables or schemas? -->

### Modified Entities
<!-- Changes to existing tables? New fields, constraints, indexes? -->

### Migration Story
<!--
How will we migrate existing data?
- Alembic migration script needed?
- Default values for new fields?
- Data backfill required?
-->


## Security Implications

### Authorization Touchpoints
<!--
What operations require auth checks?
- Which roles can access new endpoints?
- Any row-level security (e.g., user can only deploy their own models)?
-->

### Audit Fields
<!--
What gets logged?
- User actions to audit log?
- Sensitive data handling?
- PII or confidential classification concerns?
-->

### Other Security Considerations
<!--
- Input validation requirements?
- Rate limiting needed?
- Secrets/credentials handling?
-->


## Rollout Plan

### Feature Flags
<!-- Will this use a feature flag? What's the flag name and default state? -->

### Migrations
<!-- Database migrations? Service restart required? Order of operations? -->

### Backward Compatibility
<!--
How do we ensure old clients still work?
- Graceful degradation?
- Version negotiation?
- Deprecation timeline?
-->

### Testing Strategy
<!--
- Unit tests for core logic?
- Integration tests for API endpoints?
- Manual testing scenarios?
-->


---

<!--
EXAMPLE (delete before filling in):

# Design Note: Quick Model Deployment

## Approach Summary
We'll add a new service method `auto_configure_deployment()` in the Serving service that
analyzes model metadata (file format, size) and current cluster resources (CPU/GPU/memory
availability) to recommend an engine and resource allocation. This logic is exposed via
POST /api/v1/serving/auto-config. The frontend calls this endpoint when the user clicks
"Quick Deploy", displays the recommended config, and allows one-click deployment.

We're avoiding a pure client-side approach because resource availability changes rapidly
and we need server-side validation. We're not using a background job because this needs
to be fast (<500ms response time).

## API / Interface Impacts

### New Endpoints
- POST /api/v1/serving/auto-config - Returns recommended engine + resources for a model
  - Request: `{"model_id": "uuid"}`
  - Response: `{"engine": "llamacpp", "cpu": 4, "memory_gb": 8, "gpu": 0, "confidence": "high"}`
  - Auth: Requires authenticated user (any role)

### Changed Endpoints
- POST /api/v1/serving/deploy - Add optional `use_auto_config: bool` field
  - If true, server re-runs auto-config logic before deploying (in case resources changed)
  - Backward compatible: defaults to false

### Breaking Changes
None. All changes are additive.

## Data Model Impacts

### New Entities
None. Using existing Model and Deployment tables.

### Modified Entities
**Deployment table:**
- Add `auto_configured: boolean` field (default: false) to track which deployments used quick deploy
- Add index on `auto_configured` for analytics queries

### Migration Story
Alembic migration adds new column with default value `false`. No data backfill needed
since this is a new feature. Existing deployments remain marked as manually configured.

## Security Implications

### Authorization Touchpoints
- POST /api/v1/serving/auto-config: Requires authenticated user (any role)
- POST /api/v1/serving/deploy: No change (already requires editor role)
- No row-level security needed - users can auto-config any visible model

### Audit Fields
- Log to audit trail when auto-config is used: `{"action": "deployment.auto_config", "model_id": "...", "user_id": "...", "recommended": {...}}`
- Log actual deployment separately as existing logic does

### Other Security Considerations
- Input validation: Model ID must be valid UUID, must exist, user must have read access
- Rate limiting: Apply existing /api/v1/serving rate limit (10 req/min per user)
- No secrets or credentials involved

## Rollout Plan

### Feature Flags
Feature flag: `quick_deploy_enabled` (default: false)
- When disabled: "Quick Deploy" button hidden in UI, endpoint returns 404
- When enabled: Full feature available

### Migrations
1. Deploy backend with new endpoint (feature flag off)
2. Run Alembic migration to add `auto_configured` column
3. Deploy frontend with Quick Deploy UI (feature flag off)
4. Enable feature flag for internal testing
5. Enable feature flag for all users after 1 week of testing

### Backward Compatibility
Fully backward compatible:
- Old UI works (doesn't call new endpoint)
- New endpoint is additive
- Deployment table change is additive with safe default

### Testing Strategy
**Unit tests:**
- Test `auto_configure_deployment()` logic with various model formats (.gguf, .safetensors, etc.)
- Test resource selection when GPU available vs not available
- Test error handling (model not found, no resources available)

**Integration tests:**
- Test POST /api/v1/serving/auto-config endpoint
- Test POST /api/v1/serving/deploy with `use_auto_config: true`
- Test feature flag behavior (enabled vs disabled)

**Manual testing:**
- Quick deploy flow end-to-end with real model
- Test edge cases: no GPU, no memory, conflicting deployment names
- Test on both AMD64 and ARM64 nodes

-->
