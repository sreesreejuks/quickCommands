# Git Quick Commands

Use this section for repository setup, daily workflow, and repo-audit tasks.

## Setup

Initialize a new git repo in the current folder:
```bash
git init
```

Clone an existing repo:
```bash
git clone <repo-url>
```

## Basic Workflow

Check what's changed/staged:
```bash
git status
```

Stage changes for commit:
```bash
git add .
```

Commit staged changes:
```bash
git commit -m "<message>"
```

Push commits to remote:
```bash
git push
```

Pull latest changes from remote:
```bash
git pull
```

## Config

Enable long path support (Windows fix for "Filename too long" errors):
```bash
git config --global core.longpaths true
```

Check current value of a config setting:
```bash
git config --global core.longpaths
```

## Remotes

Fetch changes from remote without merging:
```bash
git fetch
```

Show remote(s) and their URLs:
```bash
git remote -v
```

Add a remote:
```bash
git remote add <name> <url>
```

Change a remote's URL:
```bash
git remote set-url <name> <new-url>
```

Remove a remote:
```bash
git remote remove <name>
```

## Branching

List branches:
```bash
git branch
```

Create and switch to a new branch:
```bash
git checkout -b <branch-name>
```

Switch to an existing branch:
```bash
git checkout <branch-name>
```

## Auditing a Cloned Repo

When cloning an existing repo to bootstrap a new microservice, search for
leftover references to the old name before assuming a rename is complete —
easy to catch `artifactId`/`repoURL` but miss things like a `<name>` tag,
Helm template `define` names, or a README title:
```bash
grep -rn "<old-service-name>" .
```
Check systematically across: `README.md`, `Chart.yaml` (both the outer
chart and any subchart), `_helpers.tpl` (Helm template `define` names),
`pom.xml` (`artifactId` *and* `name` — easy to rename one and miss the
other), CI workflow files (`.github/workflows/*.yaml`), and any ArgoCD
Application manifests (`repoURL`, `path`).
