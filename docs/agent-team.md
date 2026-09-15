# Agent team

The Mona's Project Pulse dashboard will be built by a coordinated team of four custom agents:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Breaks the work into phases, delegates tasks to the specialists, manages dependencies and file ownership, and verifies the integrated result. | [`.github/agents/orchestrator.agent.md`](../.github/agents/orchestrator.agent.md) |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository, documentation, dependencies, risks, edge cases, and validation needs, then produces an implementation plan. | [`.github/agents/planner.agent.md`](../.github/agents/planner.agent.md) |
| **Coder** | GPT-5.5 (copilot) | Implements the assigned application logic and support configuration with clear, testable behavior and explicit error handling. | [`.github/agents/coder.agent.md`](../.github/agents/coder.agent.md) |
| **Designer** | Gemini 3.1 Pro (copilot) | Shapes the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, styling, and visual clarity. | [`.github/agents/designer.agent.md`](../.github/agents/designer.agent.md) |

I am using **GitHub Copilot CLI in a Codespace** to orchestrate this team: the Orchestrator coordinates the Planner, Coder, and Designer while keeping their file scopes and dependencies clear.
Orchestrator, Planner, Coder, and Designer.
The model assigned to each agent.
The responsibility of each agent.
The .github/agents/ file for each agent.
How the team will work together to build Project Pulse.