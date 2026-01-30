# agent-onboarding

A Codex skill that initializes a repository for AI coding agents by generating:
- `AGENTS.md` (agent operating contract)
- a structured `docs/` set (technical docs for agents)

## How to use
1. Open Codex in the repository you want to initialize.
2. Invoke this skill in your prompt (for example: `$agent-onboarding`).
3. Optionally add 1–3 sentences about the project and any constraints.
4. Answer any clarification questions the skill asks.
5. Review the generated files: `AGENTS.md` and `docs/`.

## What it writes
- `AGENTS.md` at the repo root
- `docs/` (or `docs/agents/` if `docs/` already exists)

## License
Apache-2.0
