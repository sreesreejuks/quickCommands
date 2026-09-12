# GitHub CLI (gh) Quick Commands

## Auth

Check current login status and token scopes:
```bash
gh auth status
```

## Repo Info

Show the current repo's owner/name (useful for scripting `--repo` flags):
```bash
gh repo view --json nameWithOwner -q .nameWithOwner
```

## Repository

Clone a repo:
```bash
gh repo clone <owner>/<repo>
```

Create a new repo (from the current local folder, pushing it up):
```bash
gh repo create <owner>/<repo> --source=. --push
```

Open the current repo in the browser:
```bash
gh repo view --web
```

## Pull Requests

Create a PR from the current branch:
```bash
gh pr create --title "<title>" --body "<body>"
```

List open PRs:
```bash
gh pr list
```

View a PR's details (or the one tied to the current branch if omitted):
```bash
gh pr view <pr-number>
```

Check out a PR's branch locally:
```bash
gh pr checkout <pr-number>
```

Merge a PR:
```bash
gh pr merge <pr-number>
```

## Issues

Create an issue:
```bash
gh issue create --title "<title>" --body "<body>"
```

List open issues:
```bash
gh issue list
```

View an issue's details:
```bash
gh issue view <issue-number>
```

## Workflow Runs

List recent workflow runs:
```bash
gh run list
```

View a specific run's details/logs:
```bash
gh run view <run-id> --log
```

Manually trigger a workflow (must have `workflow_dispatch` configured):
```bash
gh workflow run <workflow-name>
```

## Actions Variables

List repository variables (Settings → Secrets and variables → Actions →
Variables tab):
```bash
gh variable list --repo <owner>/<repo>
```

Set (create or update) a repository variable:
```bash
gh variable set <variable-name> --repo <owner>/<repo> --body "<value>"
```

Note: `vars.<name>` in a workflow only reads from this Variables store —
a value saved as a *secret* with the same name is invisible to it. If a
workflow input silently resolves empty (e.g. `role-to-assume` missing from
the run log's `with:` block), check whether the value was accidentally
added as a secret instead of a variable.

## Actions Secrets

List repository secrets (names only, values are never shown):
```bash
gh secret list --repo <owner>/<repo>
```

Set (create or update) a repository secret:
```bash
gh secret set <secret-name> --repo <owner>/<repo> --body "<value>"
```

Delete a repository secret:
```bash
gh secret delete <secret-name> --repo <owner>/<repo>
```
