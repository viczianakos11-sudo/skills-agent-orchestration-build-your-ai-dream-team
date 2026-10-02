# Agent team for Mona's Project Pulse dashboard

I am using a custom four-agent team, orchestrated through GitHub Copilot CLI in a Codespace, to build Mona's Project Pulse dashboard.

- Planner — Model: Claude Opus 4.7 (copilot). Responsible for researching the repo, identifying edge cases, and turning the work into a practical implementation plan with file assignments and dependencies. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsible for coordinating the Planner, Coder, and Designer, dividing work into phases, delegating file-scoped tasks, and verifying that the integrated result hangs together. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsible for implementing the dashboard logic, fixing bugs, and creating any required runnable app support files within the scope assigned by the Orchestrator. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsible for the dashboard UX, accessibility, layout, responsive styling, and visual polish so the project feels like a polished Project Pulse frontend. Definition: `.github/agents/designer.agent.md`.

These custom agent definitions live in the repository under `.github/agents/`, and they are coordinated through GitHub Copilot CLI from the Codespace environment so the team can work in parallel while staying scoped to the dashboard build.