---
name: commit
description: Stage the current working-tree changes and create a git commit with a clear, conventional-commit-style message derived from the diff. Use whenever the user asks to "commit", "commit this", "save my changes", or /commit is invoked directly, and there is no more specific skill already handling the commit for them.
---

# Commit

Turn the current uncommitted changes into a single, well-described git commit.

## Steps

1. Run `git status` to see what's changed (tracked modifications and untracked files) and `git diff` (plus `git diff --staged` if anything is already staged) to understand the actual content of the changes. Never guess at a commit message from file names alone — read the diff.
2. Decide what belongs in this commit. Default to everything currently changed, unless the user has asked for something narrower (e.g. "just commit the config change"). Stage the relevant files individually by path — avoid `git add -A` or `git add .`, since those can sweep in files the user didn't mean to include (secrets, local config, build output).
3. Check `git log --oneline -10` to match this repo's existing commit message style if it has history. If the repo has no commits yet, default to Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `chore:`, etc.) with a short imperative summary line, e.g. `feat: add user authentication endpoint`.
4. Write the commit message to focus on *why* the change was made, not a mechanical restatement of the diff. One line is often enough; add a short body only if the "why" needs more than the summary line to explain.
5. Create the commit. Do not amend an existing commit or use `--no-verify` unless the user explicitly asks for that.
6. After committing, run `git status` to confirm a clean tree (or show what's still outstanding) and report the commit hash and summary back to the user.

## Notes

- If nothing is staged or changed, say so instead of creating an empty commit.
- If a pre-commit hook fails, fix the underlying issue and create a new commit — don't bypass the hook.
- Don't push automatically; committing and pushing are separate actions unless the user asks for both.
