# Command quality checklist

Use this checklist before adding or keeping a command in this repo.

## Basics

- The command is directly useful in a real terminal workflow.
- The example is short enough to copy and paste quickly.
- The command uses clear placeholder values such as `<namespace>` or `<repo-url>`.
- The purpose of the command is obvious from the description above it.

## Safety

- Destructive commands are clearly labeled or avoided unless necessary.
- The command is explicit about target environment, context, namespace, or repo.
- The example does not hide critical variables or assumptions.

## Maintainability

- The command uses current best-practice flags for the tool version it targets.
- The example is not overly specialized or tied to one temporary environment.
- The command fits the repo's category and naming conventions.

## Review

- Check whether a simpler or more portable version exists.
- If a command is environment-specific, note the platform or shell assumptions.
- Remove stale examples that no longer match current tooling behavior.
