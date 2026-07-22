## Summary
Build Mona’s Project Pulse as a static frontend dashboard in this repository using the existing agent team model: Orchestrator coordinates, Planner defines phases, Designer owns UI/UX decisions, and Coder implements files.  
The deliverable set is:

- app/index.html
- app/styles.css
- app/project-data.json
- .vscode/launch.json

The plan below is structured so an Orchestrator can run it safely with clear ownership, explicit dependencies, and CI/Codespace-friendly validation checkpoints.

## Ordered implementation steps
1. Confirm scope, constraints, and acceptance criteria from repository docs and workflows.
2. Define UI/UX direction and content model for Project Pulse before coding.
3. Create canonical project data seed for dashboard rendering.
4. Build semantic HTML structure and client-side rendering pipeline for project cards.
5. Implement polished, responsive styling and accessibility refinements.
6. Add launch configuration to run and preview the dashboard from app/ and open index.html.
7. Run local validation in Codespace and resolve failures.
8. Produce orchestration handoff notes for final validation/reporting.

## File assignments for each step
| Step | Agent responsibility | Files assigned | Dependency details per step |
|---|---|---|---|
| 1. Scope and acceptance check | Orchestrator: collect requirements and split work; Planner: verify criteria from docs/workflows | docs/agent-team.md, .github/project-pulse-brief.md, .github/steps/2-step.md, .github/steps/3-step.md, .github/workflows/2-step.yml, .github/workflows/3-step.yml, scripts/validate-exercise.sh | No upstream dependency; gates all implementation steps. |
| 2. UI/UX + data contract definition | Designer: define card hierarchy, badge language, responsive behavior, accessibility targets; Planner: freeze field contract and rendering expectations | app/index.html (planned structure), app/styles.css (planned selectors/theme), app/project-data.json (schema agreement) | Depends on Step 1. Must finish before Step 4/5 implementation details are finalized. |
| 3. Seed project data | Coder: create JSON data source with top-level projects and required fields; validate JSON parse | app/project-data.json | Depends on Step 2 data contract. Enables Step 4 rendering work. |
| 4. Build dashboard markup + rendering | Coder: implement Project Pulse page, data fetch/load path, render project-card elements and required fields; Designer: review hierarchy/accessibility | app/index.html | Depends on Step 2 and Step 3. Uses finalized data shape from app/project-data.json. |
| 5. Polish styling and responsive UX | Designer: author visual system and accessibility-focused style decisions; Coder: implement approved CSS exactly | app/styles.css | Depends on Step 2 design direction and Step 4 class/markup contract. |
| 6. Add runnable launch configuration | Coder: create strict JSON launch config, serve from app/, open index.html, set deterministic launch name | .vscode/launch.json | Depends on Step 4 existence of app/index.html and agreed port/launch behavior. |
| 7. Validate in Codespace | Coder: run technical checks; Orchestrator: verify acceptance against exercise expectations | app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json, scripts/validate-exercise.sh | Depends on Steps 3–6 complete. Failures loop back to owning step agent. |
| 8. Final orchestration handoff | Orchestrator: summarize agent contributions, validations, known risks, and readiness | docs/final-handoff.md | Depends on Step 7 passing validations. |

## Dependencies between steps
- Step 1 -> Step 2, Step 3, Step 4, Step 5, Step 6, Step 7, Step 8
- Step 2 -> Step 3, Step 4, Step 5
- Step 3 -> Step 4
- Step 4 -> Step 5, Step 6
- Step 5 -> Step 7
- Step 6 -> Step 7
- Step 7 -> Step 8

## Work that can run in parallel
- Step 3 (project data creation) and Step 6 (launch configuration draft) can run in parallel after Step 2 because:
  - They modify different files (app/project-data.json vs .vscode/launch.json).
  - Launch config semantics are mostly independent of styling and content details.
- Late Step 4 adjustments and Step 5 styling can partially overlap only if file ownership is split by phase:
  - Designer reviews and hands off style decisions first.
  - Coder applies CSS after HTML class names are stable.
  - Reason: avoids churn from changing selectors while markup is still moving.

## Work that must run sequentially
- Step 2 before Step 4 and Step 5:
  - UI hierarchy and selector conventions must be defined first to avoid rework.
- Step 3 before Step 4:
  - Rendering logic depends on actual data shape and field names.
- Step 4 before Step 6:
  - Launch target should open an existing app/index.html dashboard.
- Step 5 and Step 6 before Step 7:
  - Validation should test final integrated behavior, not partial outputs.
- Step 7 before Step 8:
  - Handoff must report actual validation outcomes.

## Edge cases to handle
- Missing or malformed app/project-data.json.
- Empty projects array (dashboard should show a clear empty state).
- Unknown status/priority values (fallback badge/class treatment).
- Long project names or recentActivity strings causing layout overflow.
- Network/file load failure for project-data.json when served statically.
- Non-200 response for data fetch in local server context.
- Accessibility concerns:
  - Insufficient color contrast for status/priority badges.
  - Missing heading hierarchy or landmarks.
  - Small-screen card readability and spacing collapse.
- Launch misconfiguration:
  - Opening server root directory instead of index.html.
  - Incorrect cwd not pointing at app/.
  - Invalid JSON format in .vscode/launch.json.

## Validation expectations
Run these in Codespace after implementation:

1. Start Copilot CLI environment (if not already active):  
   `copilot --allow-all --enable-all-github-mcp-tools`
2. JSON syntax checks:  
   `python3 -m json.tool app/project-data.json`  
   `python3 -m json.tool .vscode/launch.json`
3. Exercise-level validation script:  
   `bash scripts/validate-exercise.sh`
4. Manual run/preview check in VS Code:
   - Launch **Run Project Pulse Dashboard** from Run and Debug.
   - Confirm browser opens `http://localhost:%s/index.html` resolved to actual port and shows Project Pulse UI, not a directory listing.
5. Content/UI checks:
   - app/index.html includes Project Pulse title, references styles.css and project-data.json, and renders `project-card` elements.
   - app/styles.css includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
   - app/project-data.json uses top-level `projects` with `name`, `owner`, `status`, `recentActivity`, `priority`.
6. Orchestrator-level validation summary:
   - Confirm Designer and Coder responsibilities were both exercised and integrated.

## Open questions
1. Should app/index.html use runtime fetch from project-data.json or embed a fallback inline dataset for offline robustness?
2. Is a fixed project count expected, or should the UI support arbitrary project list length with pagination/scroll behavior?
3. Are status and priority vocabularies constrained (for example: `On Track`, `At Risk`, `Blocked`), or should they remain free-form?
4. Is keyboard-only interaction required beyond basic semantic navigation (for example, filter chips, sorting controls)?
5. Should .vscode/launch.json include only one deterministic configuration or also a secondary debug variant?
