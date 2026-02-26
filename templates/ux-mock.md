![template](https://img.shields.io/badge/template-UX_mock-2ECDA7?style=flat-square)

# UX Mock: [Feature Name]

<!--
PURPOSE: Show the user journey and screen layout BEFORE building.
FORMAT: This can be a screenshot, Figma link, HTML file, or even a hand-drawn sketch.
GOAL: One screen + clear user journey. If it takes more than one screen, break into multiple mocks.
-->

## Feature Name
<!-- Brief name for this feature or screen -->


## Screen / View Description
<!-- What is the user looking at? What page/modal/section? 1-2 sentences. -->


## Visual Mock
<!--
OPTIONS:
- Paste screenshot below
- Link to Figma: [View Mock](https://figma.com/file/...)
- Attach HTML file: `ux-mocks/feature-name.html`
- Hand-drawn photo (yes, really - if it's clear)

If pasting screenshot, use:
![screen-mock](./path-to-screenshot.png)
-->


## User Journey (Numbered Steps)
<!--
Describe what the user DOES, step by step. Use numbered list.
Example:
1. User clicks "Deploy Model" button from Models list
2. Modal opens with Quick Deploy vs Advanced options
3. User clicks "Quick Deploy"
4. System shows: "Auto-selecting engine... llamacpp selected"
5. User clicks "Confirm"
6. Modal closes, deployment card appears in Deployments list with "Initializing" status
-->


## Key Interactions
<!--
What can the user click/type/select? What happens?
Example:
- **Quick Deploy button**: Triggers auto-config + immediate deploy
- **Advanced link**: Opens full deployment form with all fields
- **Cancel button**: Closes modal, no action taken
- **Model name (clickable)**: Opens model details page
-->


## Edge Cases
<!--
What happens when things go wrong or are unexpected?
Example:
- No GPU available: Show warning badge "Running on CPU - may be slow"
- Model format not recognized: Disable Quick Deploy, show "Use Advanced mode"
- Deployment already exists: Show "Replace existing deployment?" confirmation
-->


## API Endpoints This Screen Calls
<!--
List the API endpoints this screen interacts with. Format: METHOD /path - purpose
Example:
- GET /api/v1/models/{id} - Fetch model metadata
- POST /api/v1/serving/deploy - Trigger deployment
- GET /api/v1/cluster/resources - Check available hardware
-->


---

<!--
EXAMPLE (delete before filling in):

# UX Mock: Quick Model Deployment

## Feature Name
One-click model deployment with auto-configuration

## Screen / View Description
Modal dialog that appears when user clicks "Quick Deploy" from the Models list page. Shows auto-selected configuration with option to customize.

## Visual Mock
[Figma Link](https://figma.com/file/abc123)

*(In a real scenario, you'd paste a screenshot or link to a prototype here)*

## User Journey (Numbered Steps)
1. User navigates to Models page, sees list of uploaded models
2. User clicks "Quick Deploy" button on model row (e.g., "llama-2-7b.gguf")
3. Modal opens titled "Deploy Model: llama-2-7b"
4. System shows auto-detected config:
   - Engine: llamacpp (auto-selected based on .gguf format)
   - Resources: 4 CPU, 8GB RAM (based on model size)
   - GPU: None (none available currently)
5. User sees green "Deploy Now" button and gray "Customize" link
6. User clicks "Deploy Now"
7. Modal shows spinner: "Starting deployment..."
8. Modal closes after 2 seconds
9. Success toast appears: "Deployment started - check Deployments page"
10. User navigates to Deployments page, sees new deployment with "Initializing" status

## Key Interactions
- **Quick Deploy button** (Models list): Opens quick deploy modal
- **Deploy Now button** (modal): Triggers deployment with auto-config
- **Customize link** (modal): Expands modal to show advanced form fields
- **Cancel button** (modal): Closes modal, no action taken
- **Model name in modal title** (clickable): Opens model details page in new tab

## Edge Cases
- **No GPU available but model is large**: Show warning "No GPU detected - deployment may be slow. Recommended: use GPU node"
- **Unknown model format**: Disable Quick Deploy button, show tooltip "Format not recognized - use Advanced deployment"
- **Deployment name conflict**: Auto-append "-2" to name, show in modal: "Deployment name: llama-2-7b-2"
- **Insufficient resources**: Show error in modal "Not enough resources available. Need 8GB RAM, only 4GB free. Try stopping other deployments."

## API Endpoints This Screen Calls
- GET /api/v1/models/{id} - Fetch model metadata (name, format, size)
- POST /api/v1/serving/auto-config - Get auto-selected engine + resource config
- GET /api/v1/cluster/resources - Check available CPU/GPU/memory
- POST /api/v1/serving/deploy - Trigger deployment with config
- GET /api/v1/serving/deployments - Fetch deployment list for conflicts check

-->
