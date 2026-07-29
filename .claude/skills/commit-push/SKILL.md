---
name: commit-push
description: Stage the current changes, create a git commit with a well-written message, and push to the remote. Use when the user says "/commit-push", "commit and push", or asks to commit this and push it up.
---

# Commit and Push

Commit the current working tree changes and push them to the remote in one flow.

## Steps

1. Run in parallel to gather context:
   - `git status` (never with `-uall`)
   - `git diff` (staged and unstaged)
   - `git log --oneline -10` to match this repo's commit message style
   - Check the current branch and whether it tracks a remote branch (`git status -sb` or `git rev-parse --abbrev-ref --symbolic-full-name @{u}`)
2. Review the changes. If anything looks like it could contain secrets (`.env`, credentials, API keys) even in an innocuously-named file, flag it to the user before staging instead of committing it blindly.
3. Stage relevant files by name (avoid `git add -A` / `git add .` when the diff includes anything unexpected or unrelated).
4. Write a concise commit message in [Conventional Commits](https://www.conventionalcommits.org/) format: `<type>(<optional scope>): <description>`, using a type such as `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `chore`, or `test`. Keep the summary line short and imperative; add a 1-2 sentence body focused on *why* if it's not obvious from the summary. Note: this repo's existing history predates this convention and uses plain descriptive messages — going forward, new commits made via this skill should use Conventional Commits regardless. End the message with:
   ```
   Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
   ```
   Pass the message via a HEREDOC so formatting is preserved.
5. Create the commit, then run `git status` to confirm it succeeded. If a pre-commit hook fails, fix the issue, re-stage, and make a **new** commit — never `--amend` after a failed hook, and never use `--no-verify` to skip it.
6. Push to the remote:
   - If the branch already tracks a remote, `git push`.
   - If it doesn't, `git push -u origin <branch>`.
   - Never force-push (`--force`/`--force-with-lease`) unless the user explicitly asks for it in this request, and never force-push to `main`/`master` even then without an explicit warning.
7. Report back the commit hash/summary and confirm the push succeeded (or explain why it didn't).

## Notes

- This skill performs a real push to the remote — a visible, shared-state action. Invoking `/commit-push` is the user's authorization to push for *this* run; it does not carry over to unrelated future pushes.
- If there is nothing staged or changed, say so instead of creating an empty commit.
- If the working tree has unrelated in-progress changes mixed in, ask the user which files belong in this commit rather than guessing.
