---
name: git-commit-message
description: "Analyze uncommitted changes via git status/diff and suggest English commit messages. Use when user asks for commit message recommendations, commit summaries, or wants suggestions based on uncommitted changes."
license: Complete terms in LICENSE.txt
---

# Git Commit Message

## Overview
Suggest English commit messages based on uncommitted changes in the current repo.

## Workflow
1. Gather changes
   - `git status -sb`
   - `git diff --stat`
   - `git diff`
   - If staged changes exist, also check `git diff --cached` and prioritize staged changes for the message.
   - If no changes exist, inform the user and confirm the scope of work.

2. Identify core intent
   - Determine the primary change type based on file weight and content.
   - If different areas are mixed, suggest separate commits or provide multiple message options.

3. Compose message
   - Default to Conventional Commits format: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `build`, `ci`.
   - Use imperative/present tense, keep under ~72 characters.
   - Include scope when clearly identifiable.

4. Clarify if needed
   - Ask about style (Conventional vs no prefix) or files to include when ambiguous.

## Output
- Provide 3–5 candidate commit messages in English.
- Add a one-line rationale only when necessary.

## Heuristics
- docs, README, comments → `docs`
- dependency/update, tooling → `chore(deps)` or `build`
- formatting/lint only → `chore` or `style`
- tests only → `test`
- no behavior change → `refactor`
