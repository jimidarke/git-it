---
name: git-it
description: One-stop save button for git workflows and version tracking. Auto-detects repo state, suggests .gitignore patterns, generates Conventional Commit messages, determines semantic version bumps, and executes commit+tag with final confirmation. Use when saving work, committing changes, or versioning releases.
allowed-tools: Read, Write, Edit, Grep, Bash(git:*), Bash(test:*), Bash(ls:*), Bash(cat:*)
---

# git-it: One-Stop Git Save

**Think of git like a video game save system.** Every time you save, you create a checkpoint you can return to. This skill makes saving your work just as easy - type *"Save my work"* and it will! 

## Quick Start

Just say any of these:
- **"save my work"** - Save to your current branch - same as **/git-it**
- **"save my work somewhere else"** - Create a new branch first, then save
- **"save my work with a full security scan"** - Scan ALL project files, not just changes
- **"what branches do I have?"** - See all your save locations

**Learning about git:**
- **"where is my work saved?"** - Explains how git stores your work
- **"how is my work being saved?"** - Shows the save system overview
- **"what is git doing?"** - Explains what happens behind the scenes
- **"explain git"** or **"how does git work?"** - Full beginner tutorial
- **"how is my work checked for security?"** - Explains the security scan process

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
- Attempt `git pull --ff-only` (auto-pull if no conflicts)
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

### Phase 10: Security Scan

**See:** `SECURITY.md` for detection patterns and output formats.

After user confirms intent to commit, scan staged content for security issues.

**Default Scan Scope (staged files only):**
- Scans ONLY files being committed (`git diff --cached`)
- Excludes: unchanged committed files, .gitignored files, untracked files not staged
- This is fast and focuses on what's actually changing

**Full Scan Mode** (triggered by "save my work with a full security scan"):
- Scans ALL tracked files in the repository (`git ls-files`)
- Use for: initial project audit, periodic security reviews, before public releases
- Takes longer but catches issues in existing codebase

**Scan Process:**
1. Get list of files (staged only OR all tracked, based on mode)
2. Check filenames against sensitive patterns (*.pem, id_rsa, credentials.*, etc.)
3. Scan file contents for HIGH priority patterns (API keys, private keys, credentials)
4. Scan for LOW priority patterns (debug flags, exposed ports, PII)
5. Check project-local `.security-ignored` for acknowledged patterns
6. Display results based on severity

**Output Levels:**
- **PASS**: `Security scan: ✓ Passed` → continue to Phase 10
- **INFO**: Show notes, continue to Phase 10 automatically
- **WARN**: Show warnings with recommendations, ask `Continue with commit? [y/N]`

**On Warnings:**
1. Display all warnings with file locations and matches
2. Show remediation recommendations
3. Offer to add files to .gitignore (integrates with Phase 6 patterns)
4. Check `.security-ignored` - if pattern acknowledged, show but don't prompt
5. For new warnings: Ask `Continue with commit? [y/N]` (default No)
6. If user confirms, offer to add to `.security-ignored` with optional reason
7. If user declines, return to Phase 9 for reconsideration

**Auto-gitignore `.security-ignored`:**
When creating `.security-ignored`, automatically add it to `.gitignore` if not already present.
This prevents broadcasting acknowledged security patterns to public repositories.

**Never block commit** - user has final decision, but defaults to safe option.

### Phase 11: Execute

After confirmation:
1. `git add -A` → `git commit` → `git tag -a v<version>`
2. Update VERSION.txt (via Write tool)
3. **Push is NEVER automatic.** Ask: "Push to remote? [y/N]"

---

## Branch Workflows

**See:** `EXAMPLES.md` for complete interaction examples.

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

1. Check for conflicts: `git diff main...feature-branch --name-only`
2. If clean → merge, offer to delete merged branch
3. If same files modified → warn about potential conflicts, offer options

---

## Additional Edge Cases

Beyond workflow phases, also handle:
- Tag already exists → suggest next version
- Large binary (>100MB) → warn about git LFS
- Secrets detected (.env) → warn prominently, suggest .gitignore
- Push rejected (behind remote) → suggest `git pull --rebase` then push again
- Security warnings detected → show detailed warnings, allow continue with acknowledgment
- Previously acknowledged warnings → flag but don't prompt (check `.security-ignored`)

---

## Reference Files

| File | Contents |
|------|----------|
| `PATTERNS.md` | .gitignore patterns, commit format, version bump logic |
| `SECURITY.md` | Security scan patterns, detection rules, best practices |
| `EXAMPLES.md` | Usage examples (beginner to advanced) |
| `WELCOME.md` | Welcome banners, educational responses, setup messages |

---

## Limitations

- No interactive rebases or conflict resolution
- Stages all changes (no partial staging)
- Push always opt-in, confirmation required for all writes
- Security scan is best attempt and not a substitute for proper cyber security audit

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
