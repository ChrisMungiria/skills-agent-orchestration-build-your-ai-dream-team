# Agent team

This team will use GitHub Copilot CLI in a GitHub Codespace to orchestrate the
work of building Mona's Project Pulse dashboard. The agent definitions live in
`.github/agents/`.

## Orchestrator

- **Model:** Claude Opus 4.7
- **Definition:** `.github/agents/orchestrator.agent.md`
- **Responsibility:** Coordinates the Planner, Designer, and Coder; turns the
  plan into phases with explicit file ownership and dependencies; decides what
  can run in parallel; integrates and verifies the result; and reports the
  final outcome. It does not implement the work or perform Git operations.

## Planner

- **Model:** Claude Opus 4.7
- **Definition:** `.github/agents/planner.agent.md`
- **Responsibility:** Researches the repository, documentation, dependencies,
  edge cases, risks, and implicit requirements, then produces the practical
  implementation plan that the Orchestrator can execute. It does not write
  code.

## Designer

- **Model:** Gemini 3.1 Pro
- **Definition:** `.github/agents/designer.agent.md`
- **Responsibility:** Defines the Project Pulse user experience, including
  information hierarchy, accessibility, responsive behavior, visual clarity,
  project cards, status badges, priority treatment, readable spacing, and
  polished styling with `.dashboard` and `.project-card` hooks.

## Coder

- **Model:** GPT-5.5
- **Definition:** `.github/agents/coder.agent.md`
- **Responsibility:** Implements the assigned dashboard files with clear,
  deterministic, testable code and validates the result. For Project Pulse,
  this includes `app/index.html`, `app/styles.css`, `app/project-data.json`,
  and `.vscode/launch.json` when assigned. The launch configuration must use
  strict JSON, serve from `app/`, and open `index.html`.

## Collaboration model

The Orchestrator should ask the Planner for the implementation plan first,
then delegate non-overlapping design and coding work according to dependencies.
The team should verify that the integrated Project Pulse dashboard meets the
brief before producing a final handoff. None of the custom agents stages,
commits, or pushes changes; the learner controls Git operations through
GitHub Copilot CLI.
