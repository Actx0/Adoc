# Commit

Stage all changes and create a meaningful, SSH-signed commit.

## Optional message

If the user typed text after `/commit`, treat it as the commit message hint or full message. Otherwise, draft one from the staged diff.

## Steps

1. Inspect the repo state in parallel via the Shell tool:
   - `git status`
   - `git diff` and `git diff --cached`
   - `git log -5 --oneline` (match this repo's commit style)

2. If there is nothing to commit (clean worktree and empty index), stop and say so. Do not create an empty commit.

3. Stage everything:

```bash
git add --all
```

4. Draft a concise conventional commit message (1 line, optional short body):
   - Prefer `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `style:`
   - Focus on why, not a file list
   - Follow recent `git log` style (e.g. `feat: update api docs`)
   - If the user provided a full message after `/commit`, use it (trim whitespace only)
   - If they provided a short hint, expand it into a proper conventional message
   - Never commit secrets (`.env`, credentials, private keys). Warn and unstage those files if present.

5. Commit with SSH signing using `~/.ssh/vid_ed25519` (do not change global git config). Use a HEREDOC for the message:

```bash
git -c gpg.format=ssh \
  -c user.signingkey="$HOME/.ssh/vid_ed25519" \
  -c commit.gpgsign=true \
  commit -S -m "$(cat <<'EOF'
COMMIT_MESSAGE_HERE

EOF
)"
```

Request `all` permissions for the Shell tool so the SSH key is readable outside the sandbox.

6. If the commit fails (hooks, signing, etc.), fix the issue and create a **new** commit. Do not amend unless the user explicitly asks and amend rules allow it.

7. Run `git status` and report the commit hash and subject.

## Examples

- `/commit` → analyze diff, `git add --all`, sign-commit with a drafted message
- `/commit feat: update api docs` → use that exact message
- `/commit intro copy` → draft something like `docs: expand introduction and why Actx0`

## Notes

- Do not push unless the user also asks to push.
- Do not update git config.
- Do not skip hooks (`--no-verify`) unless the user explicitly asks.
- Signing key is always `~/.ssh/vid_ed25519` for this command.
