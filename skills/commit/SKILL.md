---
name: commit
description: Draft a Git commit message from Git changes using the repository's commit history, falling back to Conventional Commits 1.0.0 when no consistent pattern exists. Use when asked to write a commit message or commit changes; propose the message before staging or committing, then commit only as a separate, explicitly approved second step.
---

# Draft and apply a commit message

Keep message generation and committing separate. Never treat a request to generate a message as permission to stage files or create a commit.

## Step 1: Propose

1. Check `git status --short`. If changes are staged, review `git diff --cached --stat` and `git diff --cached` and draft for the staged changes only. If nothing is staged, review `git diff --stat` and `git diff` and draft for the unstaged tracked changes; the user can stage them in step 2. State which changes the proposal covers and call out excluded unstaged or untracked files. If there are no reviewable changes, ask the user what to include instead of inventing a message.
2. Inspect recent commit history (`git log -10 --format=%B`) to identify a consistent pattern in subjects, scopes, bodies, and trailers; check more history if the sample is inconclusive. Use commit history, **not repository guidance documents or configuration**, to determine the team convention. If no reliable pattern exists, use [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/). Follow [message guidance](rules/message-and-apply.md) for format and scope.
3. Output the proposed message in a copyable code block, with an optional body/footer only when it adds meaningful context. State whether it follows a team convention or the Conventional Commits fallback. Do **not** run `git add` or `git commit` in this step. Invite the user to stage the intended changes if needed, then apply the message themselves or explicitly request step 2.

## Step 2: Apply (only on explicit request)

If the user asks you to commit after reviewing the proposal, recheck `git status --short` and `git diff --cached` before committing. Compare the staged content with the changes reviewed in step 1, whether they were staged then or staged by the user afterward. If nothing is staged, ask the user to stage the intended changes first. If staged content differs from the reviewed changes (including extra or missing changes), regenerate the proposal and ask for approval again. Commit **only the already staged changes** with the approved message (e.g. `git commit -m 'subject'`, with another `-m` for a body); do not stage other files, amend, or push. Report the resulting commit ID and subject, or the error if the commit fails.
