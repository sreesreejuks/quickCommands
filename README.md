# Quick Commands

A personal reference of frequently used CLI commands, organized by technology,
so they can be quickly copied and run when needed.

## How to use this repo

- Start with the technology folder you need.
- Open the folder's `README.md` index first when it exists; it points to the
  relevant command pages.
- Use the `commands.md` files as the actual command library and keep entries
  grouped by category.
- Prefer small, practical examples that are easy to copy and adapt to your
  environment.

## Structure

Each technology has its own subfolder with a consistent landing page and a
command file:

```
quick-commands/
  README.md
  checklist.md
  git/
    README.md
    commands.md
  <technology>/
    README.md
    commands.md
```

For smaller collections, a single `commands.md` file is enough. When a
technology grows beyond roughly 15-20 categories, split it into multiple
category files within that folder and keep a `README.md` index linking to them
— see `kubernetes/` for an example.

## Platform notes

- Most commands assume a Unix-like shell (`bash`/`zsh`) unless the file or
  command description specifically says otherwise.
- Windows users may need Git Bash, WSL, or PowerShell equivalents for commands
  that rely on Unix piping, shell expansion, or common Linux tooling.
- Kubernetes, Helm, and Docker commands are usually intended to be run against
  a specific cluster or context; double-check the active namespace or context
  before any destructive action.
- CI/CD examples should be treated as templates and adapted to the runner's
  environment and shell.

## Quality checklist

Before adding or keeping a command in this repo, use the checklist in
[checklist.md](checklist.md). It helps keep examples practical, safe, and easy
to maintain.

## Common gotchas

- Git: check the remote and current branch before pushing or rebasing; a repo
  can look healthy while pointing at the wrong remote or branch.
- Helm: namespace mismatches are a common source of confusion; verify the
  release namespace before uninstalling or upgrading.
- GitHub CLI: values stored as repository secrets are not visible to
  `vars.<name>` in workflows, even if they share the same name.
- Docker: container names and filters are easy to confuse; use `docker ps -a`
  and `--filter` to verify targets before cleanup.
- Kubernetes: incorrect context or namespace can lead to commands hitting the
  wrong cluster; confirm both before running apply, delete, or debug commands.

## Maintenance

Keep this repo healthy by:

- verifying command syntax whenever a tool changes or a command appears stale
- removing or updating examples that no longer match current flags or default
  behavior
- preferring commands that are easy to copy into a real shell and safe to
  adapt
- keeping category names consistent so commands remain easy to scan and find

## Adding a new technology

Create a new subfolder named after the technology (lowercase) with a
`README.md` index and a `commands.md` file, following the same structure as the
existing folders.

When adding commands, keep to the same pattern used elsewhere in the repo:
short category heading, one-line explanation, then a fenced `bash` block.
