# Agent team

For Mona's Project Pulse dashboard, the custom agent team is composed of four specialists coordinated through the repository agent definitions.

## Team summary

- Agent: Orchestrator
	- Target model: Claude Opus 4.7 (copilot)
	- Responsibility: Coordinates end-to-end delivery by requesting plans, splitting work into phases, delegating to specialists, managing sequencing vs. parallelism, and verifying integrated outcomes.
	- Definition path: .github/agents/orchestrator.agent.md

- Agent: Planner
	- Target model: Claude Opus 4.7 (copilot)
	- Responsibility: Produces implementation strategy after repository and documentation research, including dependencies, edge cases, validation expectations, and phase-ready file assignments.
	- Definition path: .github/agents/planner.agent.md

- Agent: Coder
	- Target model: GPT-5.5 (copilot)
	- Responsibility: Implements application logic and code changes in assigned files, keeps behavior deterministic and testable, and supports runnable setup for Project Pulse when assigned.
	- Definition path: .github/agents/coder.agent.md

- Agent: Designer
	- Target model: Gemini 3.1 Pro (copilot)
	- Responsibility: Owns UI/UX quality for dashboard presentation, accessibility, information hierarchy, responsive behavior, and visual clarity for Project Pulse styling tasks.
	- Definition path: .github/agents/designer.agent.md

## Orchestration note

This workflow is orchestrated with GitHub Copilot CLI inside a Codespace, where the Orchestrator delegates scoped work to Planner, Coder, and Designer to build Mona's Project Pulse dashboard.
