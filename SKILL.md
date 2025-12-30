---
name: git-it
description: One-stop save button for git workflows and version tracking. Auto-detects repo state, suggests .gitignore patterns, generates Conventional Commit messages, determines semantic version bumps, and executes commit+tag with final confirmation. Use when saving work, committing changes, or versioning releases.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(git:*), Bash(test:*), Bash(ls:*), Bash(cat:*)
---

# git-it: One-Stop Git Save

**Think of git like a video game save system.** Every time you save, you create a checkpoint you can return to. This skill makes saving your work as simple as pressing "Save Game."

## Quick Start

Just say any of these:
- **"/git-it"** or **"save my work"** - Save to your current branch
- **"save my work somewhere else"** - Create a new branch first, then save
- **"what branches do I have?"** - See all your save locations

**Learning about git:**
- **"where is my work saved?"** - Explains how git stores your work
- **"how is my work being saved?"** - Shows the save system overview
- **"what is git doing?"** - Explains what happens behind the scenes
- **"explain git"** or **"how does git work?"** - Full beginner tutorial

---

## First-Time Welcome

**See:** `WELCOME.md` for welcome banners, educational responses, and setup messages.

**Auto-display welcome when:** No `.git` folder, no `VERSION.txt`, empty `VERSION.txt`, or user asks educational questions (triggers in Quick Start).

---

## Workflow

### Phase 0: First-Time User Check

```bash
git --version
```

If git not installed → display "Git Installation Instructions" from WELCOME.md and stop.

### Phase 1: Identity Setup (Global Persist)

```bash
git config --global user.name
git config --global user.email
```

- If BOTH empty → display "Both name and email missing" from WELCOME.md, stop
- If only name missing → display "Only name missing" from WELCOME.md, stop
- If only email missing → display "Only email missing" from WELCOME.md, stop

**Do not proceed until both are configured.** This prevents commits with empty author info.

### Phase 2: Repository Check

```bash
git rev-parse --is-inside-work-tree 2>/dev/null
```

If NOT a git repo:
1. Display "Prompt to initialize" from WELCOME.md
2. If user confirms: `git init`
3. Display "After initialization" from WELCOME.md

### Phase 3: Branch & Remote Awareness

```bash
git branch --show-current              # Current branch
git branch --list                      # All branches
git remote -v                          # Check for remotes
git fetch origin 2>/dev/null           # Sync remote state (if remote exists)
git status -sb                         # Shows ahead/behind count
```

If remote exists and branch is behind:
```bash
git pull origin <branch> --ff-only   # Auto-pull if fast-forward (no conflicts)
```
- If fast-forward succeeds → continue silently, show "Synced with remote"
- If conflicts would occur → warn user, ask how to proceed

**Never auto-push.** Always ask: "Push to remote? [y/N]"

If multiple local branches exist → remind user which branch they're on and list others.
See "Branch Reminders" section below for message formats.

### Phase 4: Check for In-Progress Operations

```bash
test -f .git/MERGE_HEAD      # Merge in progress
test -d .git/rebase-merge    # Rebase in progress
```

If detected → prompt user to complete or abort before proceeding.

### Phase 5: Analyze Changes

```bash
git status --porcelain      # File status (?? M A D R)
git diff --numstat          # Lines added/removed per file
```

If no changes → show "Nothing to save" with current branch/version info.

### Phase 6: Smart .gitignore

**See:** `PATTERNS.md` → ".gitignore Patterns"

Scan untracked files for known patterns (node_modules, __pycache__, .env, etc.)
- Only suggest patterns matching actual files present
- Check existing .gitignore for duplicates
- Ask before adding: "Add these to .gitignore? [Y/n]"
- Special warning for secrets (.env files)

### Phase 7: Generate Commit Message

**See:** `PATTERNS.md` → "Commit Message Patterns"

Format: `<type>(<scope>): <description>`
- Detect type from file patterns (feat, fix, docs, test, etc.)
- Detect scope from most-changed directory
- Imperative mood, <50 chars, no period

### Phase 8: Determine Version Bump

**See:** `PATTERNS.md` → "Version Bump Logic"

Read VERSION.txt for current version (default: 0.0.0 → 0.1.0)
- MAJOR: Breaking changes, deleted public files
- MINOR: New features, `feat:` commits
- PATCH: Fixes, docs, tests, <20 lines

### Phase 9: User Confirmation

Display summary showing: commit message, version bump, tag, branch, files changed, lines +/-.
Options: [Y] Save  [n] Cancel  [e] Edit message  [v] Edit version

Wait for user input before proceeding.

### Phase 10: Execute

After confirmation:
```bash
git add -A                                    # Stage all
git commit -m "<message>"                     # Commit
git tag -a v<version> -m "<message>"          # Tag
```

Then update VERSION.txt (via Write tool).

**Push is NEVER automatic.** Always ask: "Push to remote? [y/N]"
If user confirms: `git push origin HEAD && git push origin v<version>`

---

## Branch Workflows

### "Save My Work Somewhere Else"

1. Prompt for branch name (e.g., feature-login, experiment)
2. `git checkout -b [branch-name]`
3. Proceed with normal save workflow

### Branch Reminders

When multiple branches exist:
- After saves: remind about other branches, show switch/merge commands
- Session start: "Welcome back! You're on branch X. Don't forget Y, Z..."
- Proactively show "Conflict avoidance tip" from WELCOME.md

### Simple Merge

```bash
git diff main...feature-branch --name-only   # Check for conflicts
git checkout main && git merge feature-branch
git branch -d feature-branch                  # Optional cleanup
```

If same files modified → warn about potential conflicts, offer options.
If clean → proceed with merge, offer to delete merged branch.

---

## Additional Edge Cases

Beyond workflow phases, also handle:
- Tag already exists → suggest next version
- Large binary (>100MB) → warn about git LFS
- Secrets detected (.env) → warn prominently, suggest .gitignore
- Push rejected (behind remote) → suggest `git pull --rebase` then push again

---

## Reference Files

| File | Contents |
|------|----------|
| `PATTERNS.md` | .gitignore patterns, commit format, version bump logic |
| `EXAMPLES.md` | 10 usage examples (beginner to advanced) |
| `WELCOME.md` | Welcome banners, educational responses, setup messages |

---

## Limitations

- No interactive rebases or conflict resolution
- Stages all changes (no partial staging)
- Push always opt-in, confirmation required for all writes

---

## Quick Reference

| Command | Action |
|---------|--------|
| `save my work` | Save to current branch |
| `save my work somewhere else` | New branch + save |
| `switch to [branch]` | Change branch |
| `merge X into Y` | Simple merge |
| `where is my work saved?` | Educational response |

**Requirements:** Git 2.0+, Bash (Linux/Mac/WSL)
