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
1. User clicks "Invite Members" button from Team Settings page
2. Modal opens with text area for email addresses
3. User pastes 3 email addresses (comma-separated)
4. User selects "Member" role from dropdown
5. User clicks "Send Invites"
6. Modal shows success message with 3 shareable invite links
-->


## Key Interactions
<!--
What can the user click/type/select? What happens?
Example:
- **Invite Members button**: Opens invite modal
- **Email text area**: Accepts comma or newline-separated emails
- **Role dropdown**: Selects role for all invitees (defaults to "Member")
- **Send Invites button**: Triggers invite creation
- **Copy Link icon**: Copies individual invite link to clipboard
-->


## Edge Cases
<!--
What happens when things go wrong or are unexpected?
Example:
- Invalid email format: Show inline error "user@example is not a valid email"
- User already exists: Show warning badge "user@example.com is already a team member"
- No permission: Disable "Invite Members" button, show tooltip "Admin access required"
-->


## API Endpoints This Screen Calls
<!--
List the API endpoints this screen interacts with. Format: METHOD /path - purpose
Example:
- GET /api/v1/team/members - Fetch existing team members
- POST /api/v1/team/invites - Create invitations
- GET /api/v1/team/roles - Fetch available roles for dropdown
-->


---

<!--
EXAMPLE (delete before filling in):

# UX Mock: Bulk Team Member Invitation

## Feature Name
Bulk team member invitation with shareable links

## Screen / View Description
Modal dialog that appears when user clicks "Invite Members" from the Team Settings page. Allows pasting multiple email addresses and generates shareable invite links.

## Visual Mock
[Figma Link](https://figma.com/file/abc123)

*(In a real scenario, you'd paste a screenshot or link to a prototype here)*

## User Journey (Numbered Steps)
1. User navigates to Team Settings page, sees current team member list
2. User clicks "Invite Members" button in top right
3. Modal opens titled "Invite Team Members"
4. User sees text area labeled "Email addresses (comma or newline-separated)"
5. User pastes: "alice@example.com, bob@example.com, charlie@example.com"
6. User sees role dropdown pre-selected to "Member"
7. User clicks "Send Invites" button
8. Modal shows spinner: "Sending invitations..."
9. Modal updates to show success state with 3 invite links
10. User clicks copy icon next to each link to share via Slack/chat
11. User clicks "Done" to close modal
12. User sees 3 new "Pending" members in team list

## Key Interactions
- **Invite Members button** (Team Settings): Opens invite modal
- **Email text area** (modal): Accepts emails, validates on blur
- **Role dropdown** (modal): Selects role for all invitees (Member, Admin, Viewer)
- **Send Invites button** (modal): Triggers invite creation, validates emails
- **Copy Link icon** (success state): Copies invite URL to clipboard, shows "Copied!" toast
- **Done button** (modal): Closes modal, refreshes team member list

## Edge Cases
- **Invalid email format**: Show inline error below text area "Invalid email: bob@invalid"
- **User already invited**: Show warning badge "alice@example.com already has a pending invite"
- **User already member**: Show info badge "charlie@example.com is already a team member"
- **No admin permission**: Disable "Invite Members" button, show tooltip "Only admins can invite members"
- **Email domain not allowed**: Show error "bob@external.com is not on the approved domain list. Contact support."

## API Endpoints This Screen Calls
- GET /api/v1/team/members - Fetch existing team members and pending invites
- POST /api/v1/team/invites - Create bulk invitations
- GET /api/v1/team/roles - Fetch available roles for dropdown
- GET /api/v1/organization/settings - Check if domain allowlist is enabled

-->
