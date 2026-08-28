# Mona's Project Pulse Implementation Plan

## Summary

Build a lightweight static dashboard that helps Mona's contributors scan active
projects, ownership, current status, recent activity, and priority or risk.
The dashboard will be implemented as three static app outputs:
`app/index.html`, `app/styles.css`, and `app/project-data.json`, with a
VS Code preview configuration in `.vscode/launch.json`.

The Orchestrator asks Planner for this plan first, assigns explicit
non-overlapping scopes to Designer and Coder, then integrates and verifies the
result. No agent stages, commits, or pushes changes; those git operations
remain with the learner.

## Ordered phases

1. **Plan and establish contracts.** Planner uses
   `.github/project-pulse-brief.md` and the repository's agent instructions to
   define the dashboard structure, data contract, preview behavior, file
   ownership, dependencies, risks, and validation criteria.
2. **Define the experience.** Designer specifies the information hierarchy,
   accessible markup expectations, card layout, status and priority treatment,
   spacing, responsive behavior, and visual direction. Designer works only in
   the assigned design scope and hands off concrete guidance before
   implementation.
3. **Implement the static outputs.** Coder creates the three app files and
   `.vscode/launch.json` when assigned. The HTML loads the JSON data and CSS,
   renders visible project cards, and exposes the required fields. The preview
   config serves `app/` and opens `index.html`.
4. **Integrate and verify.** Orchestrator reviews the generated files together,
   checks that the data, markup, styling, and launch configuration agree, and
   directs Coder to resolve any implementation issue within the assigned
   scope. The learner can then preview the dashboard and complete any required
   git workflow separately.

## File assignments

| File | Owner | Scope |
| --- | --- | --- |
| `.github/project-pulse-brief.md` | Planner (read-only) | Source requirements and repository-specific constraints |
| `app/index.html` | Coder | Semantic dashboard shell, exact `Project Pulse` title, stylesheet/data references, data loading, and project-card rendering |
| `app/styles.css` | Designer for direction; Coder for implementation | Polished visual system, `.dashboard` and `.project-card` hooks, badges, priority treatment, spacing, responsive layout, `border-radius`, and `box-shadow` |
| `app/project-data.json` | Coder | Strict JSON with a top-level `projects` array; each project has `name`, `owner`, `status`, `recentActivity`, and `priority` |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named `Run Project Pulse Dashboard`, with cwd `${workspaceFolder}/app`, `python3 -m http.server 5500`, and an `index.html` browser target |

Designer owns the usability and visual decisions, but does not edit the
Coder-owned HTML or data contract. Coder owns the implementation files and
must keep the code deterministic, clear, and testable.

## Designer responsibilities

- Establish a contributor-friendly hierarchy: dashboard title and context
  first, then a scannable collection of project cards.
- Define accessible semantic structure, headings, readable contrast, keyboard
  behavior, and text alternatives where visual status or priority cues are
  used.
- Specify visible status badges and an unambiguous priority or risk treatment
  that does not rely on color alone.
- Define `.dashboard` grid behavior, `.project-card` composition, spacing,
  typography, rounded corners, shadows, and responsive behavior for narrow
  screens.
- Ensure the first viewport reads as a polished Project Pulse frontend rather
  than a plain page or raw data dump.
- Report design decisions and any assumptions to the Orchestrator without
  changing files outside the assigned design scope.

## Coder responsibilities

- Implement `app/index.html`, `app/styles.css`, and
  `app/project-data.json` according to the brief and Designer handoff.
- Reference `styles.css` and `project-data.json` from the HTML, render
  multiple visible elements with the `project-card` class, and show each
  project's status, `recentActivity`, and priority.
- Keep the data shape deterministic: `projects` must be a top-level array,
  and every project object must include `name`, `owner`, `status`,
  `recentActivity`, and `priority`.
- Create `.vscode/launch.json` only within the assigned runnable-app scope:
  strict JSON with no comments, cwd `${workspaceFolder}/app`, command
  `python3 -m http.server 5500`, launch name `Run Project Pulse Dashboard`,
  and `http://localhost:%s/index.html` as the server-ready browser target.
- Validate syntax and the integrated behavior, report files touched and
  remaining risks, and do not stage, commit, or push.

## Dependencies

- The brief and Planner's data/preview contracts must be established before
  implementation begins.
- Designer's layout and accessibility decisions must precede Coder's final
  HTML/CSS implementation; otherwise visual and structural changes may
  conflict.
- `app/project-data.json` defines the fields consumed by `app/index.html`, so
  the data contract must be agreed before rendering logic is finalized.
- `app/styles.css` supplies the selectors and visual treatment used by
  `app/index.html`; both files must be integrated before visual verification.
- `.vscode/launch.json` depends on `app/index.html` existing at the configured
  cwd and must target the file rather than the directory root.

## Explicit parallel-vs-sequential decisions

**Parallel:** After Planner completes, Designer can develop the visual,
accessibility, and responsive specification while Coder prepares the
data-contract structure and launch configuration, because those scopes do not
overlap. Coder may also create the initial JSON fixture and preview config in
parallel with Designer's design work.

**Sequential:** Planner must run first. Designer's final guidance must be
available before Coder finalizes the HTML/CSS presentation. JSON shape must be
settled before HTML data rendering is integrated. The Orchestrator's
integration review and validation must follow all implementation work, because
it checks cross-file behavior and launch consistency.

## Edge cases and risks

- Empty, missing, or malformed project data should produce an explicit,
  understandable state rather than a silent blank dashboard.
- Missing fields or unexpected status/priority values should not break card
  rendering; the implementation should use clear fallback text or a visible
  data error consistent with the static-app scope.
- Status and priority colors may be indistinguishable for color-vision
  differences; retain text labels and sufficient contrast.
- Long project names, owners, activity text, and priority labels must wrap
  without overflowing cards on mobile widths.
- A browser opened at the server root can show a directory listing; the launch
  configuration must use cwd `${workspaceFolder}/app` and explicitly open
  `index.html`.
- `.vscode/launch.json` is strict JSON, so comments or trailing syntax that
  JSON parsers reject are not acceptable.
- Relative asset/data references must work when `index.html` is served from
  the configured `app/` directory.

## Validation expectations

The integrated result should satisfy the Step 3 workflow checks:

- `app/index.html`, `app/styles.css`, `app/project-data.json`, and
  `.vscode/launch.json` exist.
- HTML contains `Project Pulse`, references `styles.css` and
  `project-data.json`, includes `project-card` markup, and renders
  `status`, `recentActivity`, and `priority`.
- CSS contains `.dashboard`, `.project-card`, `border-radius`, and
  `box-shadow`.
- Project data parses as JSON, has a top-level `projects` key, and provides
  `name`, `owner`, `status`, `recentActivity`, and `priority` fields for each
  project.
- Launch config parses as JSON, contains the launch name
  `Run Project Pulse Dashboard`, uses `python3 -m http.server 5500`, serves
  from `${workspaceFolder}/app`, and opens `index.html` through
  `http://localhost:%s/index.html`.

Where applicable to the template-level exercise, also run
`bash scripts/validate-exercise.sh`; it checks the repository workflow and
learner-file expectations in addition to the Project Pulse-specific
requirements. Coder should perform focused syntax and file checks before
handoff, and Orchestrator should perform the final cross-file review.

## Open questions

- How many sample projects should the initial fixture contain, and which
  realistic Mona-team project names should represent the contributor view?
- Should the static dashboard use inline JavaScript in `index.html` to load
  `project-data.json`, or should the implementation use another repository-
  approved static rendering pattern?
- What exact status and priority vocabulary should be standardized for badges
  (for example, active/blocked and high/medium/low)?
- Is a browser-debug launch type already preferred elsewhere in the repository,
  or should the deterministic Python HTTP-server configuration be the sole
  preview path?
