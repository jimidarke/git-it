# Git-It Examples

Real-world usage examples showing git-it in action. Referenced by SKILL.md.

---

## Example 1: Complete Beginner (First Time Ever)

```
$ /git-it

Checking for git... Found git 2.39.0

Checking your identity...
  Name: (not set)
  Email: (not set)

Welcome to git! Let's set up your identity first.
This is a one-time setup that applies to all your projects.

Your name and email appear in your save history (commits).
Tip: Use the same email as your GitHub/GitLab account if you have one.

Please run these two commands:

  git config --global user.name "Your Full Name"
  git config --global user.email "your@email.com"

Then say "/git-it" again to continue!
```

---

## Example 2: First Project Save

```
$ /git-it

Checking environment... OK
  Name: Jimi Hendrix
  Email: jimi@example.com

This folder isn't a git project yet.
Initialize git here? [Y/n] Y

Git initialized! You now have an empty save history.

Analyzing 8 files...

Suggest adding to .gitignore:
  - __pycache__/  (found 2 directories)
  - .env          (contains secrets - NEVER commit this!)

Add these? [Y/n] Y

=== SAVE POINT SUMMARY ===

Message:  chore: initial commit
Version:  0.1.0 (new project)
Tag:      v0.1.0

Files (6 total):
  New: main.py, utils.py, config.py, requirements.txt, README.md

Lines: +245

Save it? [Y/n]
```

---

## Example 3: Quick Bug Fix

```
$ /git-it

Checking environment... OK
Branch: main (you have 1 other branch: feature-dark-mode)

Analyzing changes...
  Modified: src/utils.py (+3, -1)

Suggested:
  fix(utils): correct string encoding

Version: 1.2.3 -> 1.2.4 (PATCH)
  Reason: Small fix, <20 lines

=== SAVE POINT SUMMARY ===

Message:  fix(utils): correct string encoding
Version:  1.2.3 -> 1.2.4 (PATCH)
Tag:      v1.2.4
Branch:   main

Save it? [Y/n]
```

---

## Example 4: Save Work Somewhere Else (New Branch)

```
$ save my work somewhere else

Creating a new branch (parallel save file).
Changes here won't affect 'main' until you merge.

What should we call this branch?
Examples: feature-login, experiment, bugfix-header

> feature-notifications

Created and switched to: feature-notifications

Now analyzing your changes...
  Added:    src/notifications.py (+89 lines)
  Modified: src/app.py (+5, -1)

=== SAVE POINT SUMMARY ===

Message:  feat(notifications): add push notification system
Version:  1.2.4 -> 1.3.0 (MINOR)
Tag:      v1.3.0
Branch:   feature-notifications (NEW)

Save it? [Y/n]

---

Saved! Your work is preserved on branch 'feature-notifications'.

Remember: Your other branches:
  - main (your primary branch)
  - feature-dark-mode

When ready to combine, say "merge feature-notifications into main"
```

---

## Example 5: Branch Reminder on Return

```
$ /git-it

Welcome back! You're on branch: feature-notifications

Other branches you might have forgotten:
  - main (last save: 2 days ago)
  - feature-dark-mode (last save: 5 days ago)

Analyzing changes...
```

---

## Example 6: Simple Merge

```
$ merge feature-notifications into main

Checking branches...
  From: feature-notifications (3 commits ahead)
  Into: main

Checking for conflicts...
  feature-notifications changed: src/notifications.py, src/app.py
  main changed: src/utils.py, README.md

  No overlapping files - clean merge expected!

Merge feature-notifications into main? [Y/n] Y

Switched to main...
Merging feature-notifications...

Merged successfully!

main now includes all changes from feature-notifications.

Delete the feature-notifications branch? [y/N] y

Branch deleted. Work preserved in main.

Current status:
  Branch: main
  Version: v1.3.0
  Branches remaining: main, feature-dark-mode
```

---

## Example 7: Breaking Change Detection

```
$ /git-it

Analyzing changes...
  Deleted:  src/v1_api.py
  Modified: src/api.py (+45, -120)
  Modified: tests/test_api.py (+30, -50)

Suggested:
  refactor(api)!: remove deprecated v1 endpoints

  BREAKING CHANGE: v1 API removed

Version: 1.3.0 -> 2.0.0 (MAJOR)
  Reason: Breaking change detected

=== SAVE POINT SUMMARY ===

Message:  refactor(api)!: remove deprecated v1 endpoints
Version:  1.3.0 -> 2.0.0 (MAJOR)
Tag:      v2.0.0
Branch:   main

Save it? [Y/n]
```

---

## Example 8: Nothing to Save

```
$ /git-it

Checking environment... OK
Branch: main

Nothing to save! Your work matches your last save point.

Your current status:
  Branch: main
  Last save: "feat: add login page" (2 hours ago)
  Version: v1.2.0
```

---

## Example 9: Secrets Warning

```
$ /git-it

Analyzing 12 files...

WARNING: Found .env file with potential secrets!

Never commit secrets to git - they become part of permanent history.
Recommendation: Add ".env" to .gitignore

Add .env to .gitignore? [Y/n] Y

Added to .gitignore.

Continuing analysis...
```

---

## Example 10: Educational Query

```
$ where is my work saved?

YOUR SAVE LOCATIONS
━━━━━━━━━━━━━━━━━━━

📁 Project folder: /home/jimi/my-project
📦 Git storage:    /home/jimi/my-project/.git/

Everything is LOCAL to your computer until you "push" to a remote
like GitHub. Your .git folder contains:

  • Every save point (commit) you've ever made
  • All branches (parallel timelines)
  • Your version tags (v1.0.0, etc.)

Current status:
  Branch:    main
  Version:   v1.2.3
  Last save: "feat: add login" (2 hours ago)
  Total saves: 47 commits

Your work is safe! To share it online: "push to GitHub"
```
