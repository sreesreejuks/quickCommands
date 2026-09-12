# Quick Commands

A personal reference of frequently used CLI commands, organized by technology,
so they can be quickly copied and run when needed.

## Structure

Each technology has its own subfolder containing a `commands.md` file:

```
quick-commands/
  git/
    commands.md
  <technology>/
    commands.md
```

Commands within each file are grouped by category (e.g. Cluster Access, Pods,
Logs) with a one-line description above each command.

If a technology's commands outgrow a single file (roughly 15-20+ `##`
categories), split it into multiple category files inside that subfolder
plus a `README.md` index linking to them — see `kubernetes/` for an example.

## Adding a new technology

Create a new subfolder named after the technology (lowercase) with its own
`commands.md`, following the same format as `git/commands.md`.
