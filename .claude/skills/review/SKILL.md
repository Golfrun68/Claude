---
name: review
description: Review staged changes for bugs, style, and missing tests, then summarize. Use whenever the user asks to "review", "review my changes", "review staged changes", or /review is invoked directly for a local working diff (not a GitHub PR).
---

# Review

Review the currently staged changes for correctness, style, and test coverage, then give the user a concise summary.

## Steps

1. Run `git diff --staged` to see what's staged. If nothing is staged, fall back to `git diff` (unstaged working-tree changes) and tell the user you're reviewing unstaged changes instead.
2. Read each changed file's surrounding context as needed (not just the diff hunks) to judge correctness — a diff line can look fine in isolation and still be wrong given the rest of the function.
3. Check for:
   - Bugs: logic errors, edge cases, off-by-one mistakes, incorrect error handling, broken assumptions.
   - Style: inconsistency with the surrounding code's conventions (naming, formatting, structure).
   - Missing tests: new behavior or bug fixes without corresponding test coverage.
4. Summarize findings grouped by severity (bugs first, then style, then missing tests). For each finding, reference the file and line. If nothing significant is found in a category, say so briefly rather than omitting it silently.

## Notes

- This reviews the local working diff, not a GitHub pull request — for reviewing a PR, use the `review` PR skill or `/code-review` instead.
- Don't make edits during the review unless the user asks you to fix what you find; report first.
