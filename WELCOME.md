# Git-It Welcome & Education Reference

This file contains welcome messages and educational content displayed to first-time users or when they ask learning questions. Referenced by SKILL.md.

---

## First-Time Welcome Banner

**Display when:** No `.git` folder, no `VERSION.txt`, or `VERSION.txt` is empty.

```
╔══════════════════════════════════════════════════════════════════╗
║                    WELCOME TO GIT-IT!                            ║
║              Your Video Game Save System for Code                ║
╚══════════════════════════════════════════════════════════════════╝

Hi there! It looks like this might be your first time saving work
here. Let me give you a quick tour of how this works.

🎮 THINK OF GIT LIKE A VIDEO GAME SAVE SYSTEM

  ┌─────────────────────────────────────────────────────────────┐
  │  YOUR CODE          GIT CONCEPT         WHAT IT MEANS       │
  ├─────────────────────────────────────────────────────────────┤
  │  Save Point    -->  Commit         -->  Snapshot of work    │
  │  Save Slot     -->  Branch         -->  Parallel timeline   │
  │  Bookmark      -->  Tag            -->  Named save (v1.0)   │
  │  Save File     -->  Repository     -->  Your project folder │
  └─────────────────────────────────────────────────────────────┘

📁 WHERE IS YOUR WORK SAVED?

  Your work is saved RIGHT HERE in this folder, inside a hidden
  ".git" directory. Git doesn't upload anything unless you ask.

  .git/           <-- Your save history lives here
  ├── objects/    <-- All your snapshots (compressed)
  ├── refs/       <-- Bookmarks to important saves
  └── HEAD        <-- Points to your current location

  Think of it like a time machine built into your project folder.

🔄 HOW SAVING WORKS

  1. You make changes to files (edit, add, delete)
  2. Say "/git-it" or "save my work"
  3. I analyze what changed and suggest a save message
  4. You confirm, and SNAP - a save point is created
  5. You can ALWAYS go back to any previous save

🌳 BRANCHES: PARALLEL TIMELINES

  Imagine you want to try something risky without breaking
  your working code. Branches let you:

  main ─────●─────●─────●───────────●  (stable code)
                   \                 /
                    ●─────●─────●──┘   (experiment branch)
                    (try stuff)        (merge if it works!)

  • "save my work" = save to current branch
  • "save my work somewhere else" = create new branch first
  • "merge X into Y" = combine branches when ready

🏷️ VERSIONS: YOUR RELEASE BOOKMARKS

  Every save gets a version number (like 1.2.3):

  • MAJOR (1.x.x) = Big changes, might break things
  • MINOR (x.1.x) = New features, safe upgrade
  • PATCH (x.x.1) = Bug fixes, always safe

  Example: v1.0.0 → v1.0.1 (bug fix) → v1.1.0 (new feature)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Ready to create your first save point? Let's check your setup...
```

---

## Educational Responses

### "where is my work saved?" / "where does git save?"

```
YOUR SAVE LOCATIONS
━━━━━━━━━━━━━━━━━━━

📁 Project folder: [current directory path]
📦 Git storage:    [current directory path]/.git/

Everything is LOCAL to your computer until you "push" to a remote
like GitHub. Your .git folder contains:

  • Every save point (commit) you've ever made
  • All branches (parallel timelines)
  • Your version tags (v1.0.0, etc.)

Current status:
  Branch:    [current branch]
  Version:   [current version]
  Last save: "[last commit message]" ([time ago])
  Total saves: [commit count] commits

Your work is safe! To share it online: "push to GitHub"
```

---

### "how is my work being saved?" / "how does saving work?"

```
HOW GIT-IT SAVES YOUR WORK
━━━━━━━━━━━━━━━━━━━━━━━━━━

When you say "/git-it" or "save my work", here's what happens:

  1. DETECT    I scan for changed files
               ↓
  2. ANALYZE   I figure out what kind of changes (feature? fix?)
               ↓
  3. SUGGEST   I write a save message describing changes
               ↓
  4. CONFIRM   You review and approve (nothing happens without you!)
               ↓
  5. SNAPSHOT  Git creates a permanent save point
               ↓
  6. TAG       I add a version number (v1.2.3)
               ↓
  7. DONE!     Your work is safely preserved

Every save point:
  ✓ Is permanent (can't be accidentally deleted)
  ✓ Is complete (captures ALL your files)
  ✓ Is retrievable (you can go back anytime)
  ✓ Is identified (unique ID + your message)

Want to see your save history? Ask "show my save history"
```

---

### "what is git doing?" / "explain git" / "how does git work?"

Display the full **First-Time Welcome Banner** from above.

---

### "show my save history" / "what saves do I have?"

Run: `git log --oneline -10`

Format output as:
```
YOUR SAVE HISTORY (last 10)
━━━━━━━━━━━━━━━━━━━━━━━━━━━

  [tag]  [hash]  [message]                        ([time ago])
  v1.2.3  abc1234  feat(auth): add OAuth login      (2 hours ago)
  v1.2.2  def5678  fix(api): handle null response   (yesterday)
  v1.2.1  ghi9012  docs: update README              (3 days ago)
  ...

Total: [count] save points across [branch count] branches

To restore any save: "go back to [hash]"
To see full details: "show commit [hash]"
```

---

## Git Installation Instructions

**Display when:** `git --version` fails.

```
Git is not installed on your system.

Install it from: https://git-scm.com/downloads

  Windows:  Download installer from website
            Or run: winget install Git.Git

  Mac:      Run: brew install git
            Or run: xcode-select --install

  Linux:    Ubuntu/Debian: sudo apt install git
            Fedora: sudo dnf install git
            Arch: sudo pacman -S git

After installing, say "/git-it" again to continue!
```

---

## Identity Setup Messages

### Both name and email missing

```
Welcome to git! Let's set up your identity first.
This is a one-time setup that applies to all your projects.

Your name and email appear in your save history (commits).
Tip: Use the same email as your GitHub/GitLab account if you have one.

Please run these two commands:

  git config --global user.name "Your Full Name"
  git config --global user.email "your@email.com"

Then say "/git-it" again to continue!
```

### Only name missing

```
Almost there! Your git email is set, but your name is missing.

Please run:
  git config --global user.name "Your Full Name"

Then say "/git-it" again!
```

### Only email missing

```
Almost there! Your git name is set, but your email is missing.

Please run:
  git config --global user.email "your@email.com"

Then say "/git-it" again!
```

---

## Repository Initialization

### Prompt to initialize

```
This folder isn't a git project yet.

Think of it like starting a new save file - once initialized,
you can save your progress anytime.

Initialize git here? [Y/n]
```

### After initialization

```
Git initialized! You now have an empty save history.
Your first save will be marked as version 0.1.0.
```

---

## Branch Education

### Conflict avoidance tip (show when multiple branches exist)

```
TIP FOR MULTIPLE BRANCHES

To avoid conflicts when merging:
  - Work on DIFFERENT FILES in each branch
  - Example: Edit "login.py" in feature-login
             Edit "dashboard.py" in main

When you edit the SAME file in multiple branches, git may need
your help deciding which changes to keep (a "merge conflict").

Keep it simple: One file, one branch at a time.
```

### Branch reminder (show periodically when multiple branches exist)

```
Reminder: You have [N] other branches. Don't forget about them!
Use "git branch" to see all, or ask me to help merge when ready.
```
