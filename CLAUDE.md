# CLAUDE.md

Guidance for working in this repo.

## Purpose

This is a personal library of copy-pasteable CLI commands, organized by
technology. It is reference material, not application code — there is
nothing to build, test, or lint.

## Conventions

- One subfolder per technology (lowercase name), each containing a
  `commands.md`.
- Commands are grouped by category using `##` headings (e.g. Cluster Access,
  Pods, Logs, Exec/Debug) — pick categories that fit the technology.
- Each command gets a one-line description directly above it, followed by a
  fenced code block (` ```bash ` unless the tool is something else).
- Always use general placeholder values (e.g. `<namespace>`, `<pod-name>`,
  `<cluster-name>`) instead of real example values in commands — including
  values that are typically fixed/well-known defaults for the tool (e.g.
  ArgoCD's `argocd` namespace) — with the one-line description explaining
  what each placeholder represents and, where useful, noting the typical
  default. This keeps commands legible without needing to remember what a
  specific real value was for.
- When the user shares new commands for an existing technology, append them
  to the matching category in that technology's `commands.md`, creating a
  new category heading if none fits.
- When the user introduces a new technology, create a new subfolder +
  `commands.md` following the same structure as `git/commands.md`.
- If a technology's `commands.md` grows past ~15-20 `##` categories, split it
  into multiple category files inside that subfolder plus a `README.md`
  index linking to them — see `kubernetes/` for an example. When adding
  commands to an already-split technology, append to the matching category
  file (or create a new one + link it from the index) instead of recreating
  a single `commands.md`.
