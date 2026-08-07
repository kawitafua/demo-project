# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository state

This repository currently contains **no application source code**. It is an Obsidian vault used to plan a project called "my-coffee-store" (a coffee shop app/site, based on the repo name). There is no `package.json`, build tool, linter, or test runner configured — do not assume any particular language/framework until source code actually appears.

If asked to build/lint/test, first check whether source code has been added since this file was written; if the repo is still just the `.docs/` vault described below, say so instead of guessing at commands.

## Structure

- `.docs/` — the actual documentation vault (an Obsidian vault: it has its own `.obsidian/` config). Numbered top-level folders encode the project workflow, each pipeline stage feeding the next:
  - `00-archived/` — superseded docs; never delete docs from the project, move them here instead
  - `01-requirements/` — `01-spec` (source-of-truth requirements) → `02-plan` (roadmap/milestones) → `03-task` (actionable task breakdown)
  - `02-design/` — `01-prototypes` (UI/UX mockups, wireframes) and `02-technical` (architecture, DB schema, API design)
  - `03-testing/` — `01-test-plan` (test cases) → `02-test-result` (pass/fail, bugs found)
  - `04-retrospectives/` — lessons learned per phase/sprint, informed by test results and the log
  - `05-log/` — chronological changelog / decision log
- Each folder has an `index.md` describing its purpose and linking to related folders via Obsidian `[[wikilink]]` syntax — follow this convention (relative wikilinks, not plain markdown links) when adding new docs inside `.docs/`.
- Documentation content inside `.docs/` is written in **Thai**; match that when editing or adding docs there unless told otherwise.
- Top-level `.obsidian/` and the dated note (e.g. `2026-08-01.md`) are a separate, personal Obsidian vault rooted at the repo root — distinct from the `.docs/` project vault.
- `.claude/` is gitignored (see `.gitignore`), so anything placed there (settings, worktrees) is local-only and won't be committed.

## Working in this repo

- When the user starts adding real source code, re-derive build/lint/test commands from whatever tooling files show up (`package.json`, etc.) rather than relying on this file.
- When filling in the documentation vault, respect the existing pipeline: requirements → design → testing → retrospectives → log, and place new content in the matching numbered folder rather than inventing new top-level structure.
