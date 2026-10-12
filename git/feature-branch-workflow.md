# Git Feature Branch Workflow

Step-by-step commands for starting a feature branch, depending on where you are.
Replace `<...>` placeholders: `<new-branch>` is the feature branch name
(e.g. `feat/my-feature`), `<old-branch>` is the branch you were on, `<remote>`
is usually `origin`, and `<base-branch>` is usually `main`.

## Concepts

- `git fetch <remote>` only downloads the remote's changes and updates the
  `<remote>/*` tracking refs (e.g. `origin/main`). It never changes your local
  branches or files. `git pull` = fetch + merge into the current branch.
- `HEAD` means "the branch I'm on right now", so `git push -u <remote> HEAD`
  pushes the current branch to a same-named remote branch.
- `-u` sets the upstream so later `git push` / `git pull` need no arguments.
- A branch created from `<remote>/<base-branch>` gets that as its upstream, so
  a plain `git push` refuses until you push once with `-u <remote> HEAD`.

## 1. On main, with uncommitted edits or new files

See what you've changed, so you know what will move to the new branch:
```bash
git status
```

Create the feature branch from where you are now and switch to it. Uncommitted edits and new files come along; main is untouched:
```bash
git switch -c <new-branch>
```

Stage the files and commit on the new branch. `<path>` is the file or folder to stage; `<message>` is the commit subject:
```bash
git add <path>
git commit -m "<message>"
```

Publish the branch (HEAD = current branch) and set the upstream:
```bash
git push -u <remote> HEAD
```

If local main is behind the remote, refresh the remote's state first (changes no files):
```bash
git fetch <remote>
```

Then create the branch from the latest remote main instead; your edits come along. If git refuses because a file you edited also changed upstream, use the stash route in 2b:
```bash
git switch -c <new-branch> <remote>/<base-branch>
```

## 2. On another branch, starting a new feature

### 2a. Everything is committed (clean tree)

Confirm nothing is uncommitted:
```bash
git status
```

Refresh the remote's state:
```bash
git fetch <remote>
```

New branch from the latest remote main, not from the old branch:
```bash
git switch -c <new-branch> <remote>/<base-branch>
```

### 2b. Uncommitted edits that belong to the OLD branch

Park the edits, including new untracked files. `<label>` is a note to identify the stash:
```bash
git stash push -u -m "<label>"
```

Refresh the remote and create the new branch from the latest remote main:
```bash
git fetch <remote>
git switch -c <new-branch> <remote>/<base-branch>
```

Later, go back to the old branch and restore the parked edits:
```bash
git switch <old-branch>
git stash pop
```

### 2c. Uncommitted edits that belong to the NEW feature

Create the new branch from the latest remote main; the edits come along. If git refuses, stash, switch, then run `git stash pop` on the new branch:
```bash
git fetch <remote>
git switch -c <new-branch> <remote>/<base-branch>
```

The commit and push steps are the same as in section 1.

## Mistake to avoid

Running `git switch -c <new-branch>` with no start point while on another
feature branch starts the new branch from that branch, so its commits end up
in your new PR. Always give the start point (`<remote>/<base-branch>`) in that
case.
