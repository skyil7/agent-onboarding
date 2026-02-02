---
name: agent-onboarding
description: Initialize a repository by analyzing its structure and generating `AGENTS.md` and structured `docs/`, enabling AI coding agents (Claude Code, Codex, Cursor, etc.) to operate safely, deterministically, and with minimal human supervision.
license: Apache-2.0
metadata:
  author: Gio Paik <giopaik0@gmail.com>
  version: "1.0"
---
## Purpose
Initialize a repository so that AI coding agents (Claude Code, Codex, Cursor, etc.)
can operate safely, deterministically, and with minimal human supervision.

This skill generates:
- AGENTS.md (agent operational contract)
- docs/ directory with structured technical documentation

## Inputs
- Repository to initialize.
- Optional: short human note describing the project and instructions (1–3 sentences)

## Outputs
- AGENTS.md at repository root
- docs/ directory with standardized markdown files

## Core Principles
- Do NOT guess. If information is missing, explicitly mark TODOs.
- Prefer correctness and safety over completeness.
- Optimize for agent execution, not human prose.
- Minimize exploration cost and failure modes.
- If the given repository follows a monorepo pattern, create a subdirectory under docs/ for each project so the documentation doesn’t get mixed together.
  - Example) `docs/frontend/00_overview.md` `docs/frontend/adr` `docs/frontend/plan`

---

## Execution Steps

### Step 1: Repository Inspection

- Detect:
  - Primary language(s)
  - Framework(s)
  - Package manager
  - Test framework
  - Deployment indicators (Dockerfile, CI configs, Vercel, etc.)
- Identify:
  - Entry points
  - Core business logic directories
  - Infrastructure-related files (DB, queues, storage)

If detection is ambiguous:
- Record ambiguity explicitly in docs/00_overview.md
- Do NOT assume defaults

---

### Step 2: Confirm Ambiguities & Decisions With the Human

Request the user to provide the following information:

- Whether Git-related commands are allowed: use freely vs use only with explicit approval
- Language preferences:
  - Language for comments, documents and commit messages
  - Communication language: same as the user vs a specific language
- Any other items discovered in Step 1 that are ambiguous or seem to require questions

If the human does not answer:
- Do NOT guess.
- Pause and ask again as needed; if you must proceed, mark missing items as “Unknown / TODO” in docs (without embedding the unanswered questions), and keep AGENTS.md conservative (minimal privileges, no destructive ops).

---

### Step 3: Generate AGENTS.md
First, copy [AGENTS.md.example](`assets/AGENTS.md.example`), then edit it based on the information gathered so far to create `AGENTS.md`.

AGENTS.md MUST include:
# Repository Guidelines
## Project Structure & Module Organization

## Documentation

## Coding Style & Naming Conventions

## Commit Message & PR Guidelines
### Commit Message Format
### Atomic Commits

## Agent Operation & Safety Rules

## Language Policy

## User Interaction

Hard rules:
- Keep AGENTS.md concise (target: 50–120 lines)
- Link to docs/ instead of duplicating explanations

---

### Step 4: Generate docs/ Structure

**If the `docs/` directory already exists, generate all new files inside `docs/agents/` instead of `docs/`. Otherwise, use `docs/` as the target directory.**

Create the following files if applicable:

- docs/00_overview.md
- docs/10_architecture.md
- docs/20_dev_env.md
- docs/30_commands.md
- docs/40_testing.md
- docs/50_deploy.md
- docs/60_observability.md
- docs/70_security.md
- docs/80_style_guide.md
- docs/90_troubleshooting.md

Rules:
- Omit files that are clearly irrelevant
- Never fabricate cloud providers, CI tools, or databases
- Prefer explicit “Unknown / TODO” over assumptions

---

### Step 5: ADR Detection (Optional but Recommended)
If architectural decisions are inferred:
- Create docs/adr/
- Record assumptions as provisional ADRs
- Mark them as “UNVERIFIED”

---

### Step 6: Validation Checklist
Before finalizing:
- All commands copy-paste runnable OR marked TODO
- No secrets included
- No speculative tech choices
- AGENTS.md links resolve correctly

---

## Failure Handling
If the repository is:
- Empty
- Too small to infer structure
- Highly ambiguous

Then:
- Generate minimal AGENTS.md
- Generate docs/00_overview.md that clearly separates detected facts vs “Unknown / TODO” items (but do not include a question list; ask any needed questions during execution).

If, at any point, you encounter ambiguities or actions that appear potentially risky:

Then:
- always pause and request clarification from the user before proceeding.

---

## Non-Goals
- Do not optimize for marketing or onboarding humans
- Do not refactor code
- Do not introduce new dependencies
- Do not modify runtime behavior

---

## Success Criteria
A coding agent can:
- Identify where to make changes
- Execute tests confidently
- Avoid destructive or unsafe operations
- Complete scoped tasks without human correction
