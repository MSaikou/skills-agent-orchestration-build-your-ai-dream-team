# Agent team

For Mona's Project Pulse dashboard, I will use a small custom agent team defined under `.github/agents/` and orchestrated with GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the repo, identify requirements and edge cases, and produce a phased implementation plan. Definition: `.github/agents/planner.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: shape the dashboard UX/UI, accessibility, information hierarchy, and visual polish for the Project Pulse experience. Definition: `.github/agents/designer.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the assigned code changes, fix logic issues, and validate the behavior in the app. Definition: `.github/agents/coder.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break work into phases, delegate tasks to the specialist agents, coordinate dependencies, and confirm the integrated result is coherent. Definition: `.github/agents/orchestrator.agent.md`.

This setup uses the GitHub Copilot CLI as the coordination layer in the Codespace, with the Orchestrator delegating to the Planner, Designer, and Coder as needed to move the dashboard from planning through implementation and validation.
