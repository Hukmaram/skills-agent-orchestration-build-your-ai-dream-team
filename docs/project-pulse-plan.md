# Project Pulse implementation plan

## Dashboard goal and concise implementation summary

Goal: create a lightweight static dashboard that gives Mona’s team a fast, contributor-friendly view of active projects, project owners, status, recent activity, risk/priority, and a short summary. The dashboard should feel polished and readable without requiring a framework or backend.

Implementation summary: build a small static app with a semantic HTML shell, a static JSON data source, and CSS styling that emphasizes project cards, badges, spacing, and hierarchy. The app should be previewable from VS Code via a launch configuration that serves the `app/` folder and opens `index.html` directly, so learners see the dashboard instead of a directory listing.

## File assignments

- `app/index.html`
  - Primary owner: Coder
  - Design review: Designer
  - Purpose: markup for the dashboard shell, project-card list, and any minimal JS that reads `project-data.json` and renders project data.
  - Must include the Project Pulse title and semantic sections for project cards, status, owner, recent activity, and priority.

- `app/styles.css`
  - Primary owner: Designer
  - Coder alignment: ensure CSS hooks match the HTML classes used by the Coder.
  - Purpose: visual design system for the dashboard: spacing, typography, badges, card layout, border radius, shadows, and responsive behavior.
  - Explicit CSS hooks should include `.dashboard`, `.project-card`, and design tokens for status/priority states.

- `app/project-data.json`
  - Primary owner: Coder
  - Design review: Designer
  - Purpose: top-level `projects` array with each object containing `name`, `owner`, `status`, `recentActivity`, and `priority`.
  - Used as the canonical data contract for the dashboard and must remain valid JSON.

- `.vscode/launch.json`
  - Primary owner: Coder
  - Design review: not required
  - Purpose: create the VS Code launch configuration named `Run Project Pulse Dashboard` that serves the `app/` directory and opens `index.html`.
  - Must be strict JSON and should not serve a directory listing.

## Agent responsibilities and integration

### Designer
Responsibilities:
- Establish the visual hierarchy and information architecture.
- Define the dashboard mood: clean, contributor-friendly, readable, polished.
- Guide spacing, color contrast, badges, card shapes, and responsive behavior.
- Ensure the dashboard clearly differentiates project status and risk/priority.

Files owned:
- `app/styles.css` (primary owner)
- Design review of `app/index.html` structure and semantic layout

Integration:
- Designer defines the visual system and class names before the Coder finalizes HTML structure.
- Coder implements the design contract in markup and JS while staying within the agreed DOM classes.
- The final result should feel intentional and consistent rather than a generic CSS pass over a raw HTML file.

### Coder
Responsibilities:
- Build the functional static app.
- Create or update `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`.
- Ensure the dashboard renders from JSON and works as a static app without a server-side dependency.
- Validate the generated app and launch configuration before handoff.

Files owned:
- `app/index.html` (primary owner)
- `app/project-data.json` (primary owner)
- `.vscode/launch.json` (primary owner)
- Minor CSS adjustments only when required to match the final HTML structure

Integration:
- Coder must align HTML structure to Designer’s CSS contract.
- Coder should render project cards based on the JSON schema and keep the same field names and statuses used in the design.
- Launch config should only be finalized after `index.html` and related files are in their final expected form.

## Dependencies between tasks and agents

Sequential dependencies:
1. Repository brief and requirements must be reviewed first.
2. Designer’s visual direction should be agreed before final HTML and CSS are locked.
3. Data schema for `app/project-data.json` must be defined before final card rendering logic is implemented.
4. HTML structure should be finalized before the launch configuration is configured, because the launch file depends on the final folder and file names.
5. Validation must happen after integration, once HTML, CSS, JSON, and launch config all exist together.

Cross-agent dependencies:
- Designer and Coder must agree on CSS hooks and semantic markup names.
- Coder depends on the JSON field definitions for rendering, while Designer depends on the same data fields for labeling and badges.
- Launch preview depends on the final app structure and file paths.

## Parallel work decisions

What can run in parallel:
- Designer can draft the visual direction and CSS class strategy while Coder defines the JSON schema and app structure.
- Coder can build the `project-data.json` content and draft the launch file in parallel with the Designer’s CSS pass, as long as the field names and page structure are still aligned to the brief.

What must run sequentially:
- Final HTML and CSS integration must be sequential after the design contract is agreed.
- Final launch configuration should be added only after the app structure is stable.
- Validation should be a final sequential gate, because it depends on the full app being assembled.

## Proposed execution phases

Phase 1: Requirements and contract
- Confirm the dashboard goal, required fields, and expected preview behavior.
- Establish the data contract: `projects` array with `name`, `owner`, `status`, `recentActivity`, `priority`.

Phase 2: Design and data creation
- Designer creates the visual hierarchy and CSS class naming.
- Coder creates `app/project-data.json` following the agreed fields.

Phase 3: UI implementation
- Coder builds `app/index.html` with sections and project-card markup.
- Designer reviews the structure and adjusts CSS if needed.

Phase 4: Preview support
- Coder creates `.vscode/launch.json` so the app opens from the `app/` directory and displays `index.html`.

Phase 5: Validation
- Check that the page renders in preview.
- Verify JSON parses and UI cards match the data.
- Confirm the launch configuration opens the dashboard, not a directory listing.

## Validation expectations

Dashboard validation:
- Open the app using the VS Code launch configuration and confirm the page is the Project Pulse dashboard, not a folder index.
- Verify that each project card shows: name, owner, status, recent activity, and priority.
- Check that cards have readable spacing, status badges, and consistent visual hierarchy.
- Confirm the layout is responsive enough for a narrow browser width without breaking readability.

Implementation-specific validation:
- `app/index.html` references `styles.css` and loads the dashboard data source.
- `app/project-data.json` parses as valid JSON and has a top-level `projects` array.
- `app/styles.css` includes the core design hooks such as `.dashboard` and `.project-card` and uses rounded corners, shadows, and spacing.
- `.vscode/launch.json` includes the launch name `Run Project Pulse Dashboard` and targets `index.html` from the `app/` workspace folder.

## Edge cases, risks, and open questions

Edge cases:
- If the HTML is opened directly as a file instead of being served from the `app/` folder, `fetch()`-based JSON loading may fail due to browser restrictions. This is why the launch config should serve the app properly.
- Empty or missing project fields should be handled gracefully in the UI to avoid broken card layouts.
- Status text should be normalized to a simple set of values so badges and styling stay predictable.

Risks:
- CSS and HTML can drift apart if class names are inconsistent.
- Invalid JSON would break the app silently if the data source is not validated.
- Launch configuration mismatch could cause a directory listing instead of the dashboard.

Open questions:
- Should project summaries be rendered as static text entries in the JSON or generated from the status/priority values in the UI?
- Should priority and status use fixed values (e.g., `Low`, `Medium`, `High`, `At Risk`) or more semantic labels from the team?
- Should the dashboard be fully static with inline rendering, or should it fetch a local JSON file via a served app environment?

## Final handoff expectation

The Orchestrator should treat this as a coordinated integration task:
- Designer owns the visual design and CSS contract.
- Coder owns the static app implementation and preview support.
- The final result is a coherent Project Pulse dashboard that loads from `project-data.json`, is styled by `styles.css`, and is previewable via the VS Code launch configuration.

This plan gives the Orchestrator a clear sequence, file ownership, dependencies, validation plan, and parallelization decisions without overloading any single agent.
