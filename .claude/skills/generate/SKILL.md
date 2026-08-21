---
name: generate
description: Create a new SKILL.md for this repo from a plain-language description of what it should do. Use whenever the user asks to "generate a skill", "create a new skill", "add a /command for X", or /generate is invoked directly.
---

# Generate

Turn a description of a desired workflow into a new skill under `.claude/skills/<name>/SKILL.md`, matching the format already used by this repo's `commit` and `review` skills.

## Steps

1. **Capture intent.** If the user's description is already specific (what it does, when it should trigger, what steps it follows), work from that directly. If it's vague ("a skill for X"), ask focused questions before writing anything:
   - What should invoking this skill actually accomplish?
   - When should it trigger — what phrases or situations, and what should the `/command` be called?
   - What are the concrete steps it should follow, in order?
   - Is there an existing skill in this repo it should look/feel like, or that it might conflict with?

2. **Pick the skill name and directory.** Use a short kebab-case name (e.g. `deploy-check`, not `DeployCheck` or `deploy_check_skill`). Create `.claude/skills/<name>/SKILL.md`. Check `.claude/skills/` first — don't collide with or duplicate an existing skill; if the request overlaps heavily with one, say so and ask whether to extend the existing skill instead of creating a new one.

3. **Write the frontmatter.** This is the primary triggering mechanism, so:
   - `name:` the kebab-case skill name.
   - `description:` one or two sentences covering *both* what the skill does *and* when to use it (explicit trigger phrases, `/command` invocation, situations where it applies). Be a little assertive about triggering conditions rather than vague — Claude tends to under-trigger skills with soft descriptions.

4. **Write the body**, following the existing repo pattern (see `.claude/skills/commit/SKILL.md` and `.claude/skills/review/SKILL.md` for reference):
   - `# Title` — one line restating the skill's purpose.
   - `## Steps` — a numbered list of concrete actions in the order Claude should take them. Write in imperative form. Explain *why* a step matters when it's non-obvious, rather than issuing bare commands.
   - `## Notes` (optional) — edge cases, things it should explicitly *not* do, or how it relates to other skills.
   Keep it tight — this repo's existing skills are ~20 lines; don't pad with generic advice the model already knows.

5. **Show the user the drafted SKILL.md** before treating the task as done, and ask if the trigger phrasing, steps, or scope need adjusting.

6. Do not commit automatically — creating the file satisfies the request; committing is a separate action unless the user asks for both (in which case use the `commit` skill's conventions).

## Notes

- This is a lightweight version of skill creation meant for this repo's simple skills. If the user wants iterative testing, evals, or description-optimization benchmarking for a skill, point them at the `skill-creator` skill instead — don't try to replicate its eval/benchmark machinery here.
- If asked to *modify* an existing skill rather than create a new one, edit its `SKILL.md` in place — don't create a duplicate.
