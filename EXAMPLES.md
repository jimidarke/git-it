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

---

## Example 11: Security Scan - Clean Pass

```
$ /git-it

Checking environment... OK
Branch: main

Analyzing changes...
  Added:    src/api/users.py (+89 lines)
  Modified: src/app.py (+12, -3)
  Added:    tests/test_users.py (+45 lines)

=== SAVE POINT SUMMARY ===

Message:  feat(api): add user management endpoints
Version:  1.3.0 -> 1.4.0 (MINOR)
Tag:      v1.4.0
Branch:   main

Files (3 total):
  New: src/api/users.py, tests/test_users.py
  Modified: src/app.py

Lines: +146, -3

Save it? [Y/n] Y

Security scan: ✓ Passed

Saving...
  Committed: feat(api): add user management endpoints
  Tagged: v1.4.0

Push to remote? [y/N]
```

---

## Example 12: Security Scan - Warnings Found

```
$ /git-it

Checking environment... OK
Branch: feature-aws-integration

Analyzing changes...
  Added:    src/aws/client.py (+156 lines)
  Added:    config/aws.py (+23 lines)
  Modified: src/app.py (+8, -2)

=== SAVE POINT SUMMARY ===

Message:  feat(aws): add S3 client integration
Version:  1.4.0 -> 1.5.0 (MINOR)
Tag:      v1.5.0
Branch:   feature-aws-integration

Save it? [Y/n] Y

Security scan: ⚠️ 2 warnings

  ⚠️ API Key: AWS access key detected
     File: config/aws.py:15
     Match: AKIAIOSFODNN7EXAMPLE

  ⚠️ URI Credentials: SFTP password in URL
     File: src/aws/client.py:42
     Match: sftp://deploy:s3cr3t@backup.example.com

Recommendations:
  1. Move secrets to environment variables
  2. Add sensitive files to .gitignore
  3. If already committed, rotate credentials

Add config/aws.py to .gitignore? [Y/n] n

Continue with commit? [y/N] y

Add to .security-ignored? (optional reason): Test API key for development

Added to .security-ignored
Added .security-ignored to .gitignore (keeping acknowledgments private)

Saving...
  Committed: feat(aws): add S3 client integration
  Tagged: v1.5.0

Push to remote? [y/N]
```

---

## Example 13: Security Scan - User Declines

```
$ /git-it

...

Security scan: ⚠️ 1 warning

  ⚠️ Private Key: RSA private key detected
     File: deploy/prod.pem:1
     Match: -----BEGIN RSA PRIVATE KEY-----

Continue with commit? [y/N] n

Commit cancelled.

Tip: Add sensitive files to .gitignore:
  echo "deploy/prod.pem" >> .gitignore
```

---

## Example 14: Security Scan - Informative Notes

```
$ /git-it

Checking environment... OK
Branch: main

Analyzing changes...
  Modified: src/server.py (+15, -8)
  Modified: config/settings.py (+4, -1)

=== SAVE POINT SUMMARY ===

Message:  feat(server): add HTTP server configuration
Version:  1.5.0 -> 1.6.0 (MINOR)
Tag:      v1.6.0
Branch:   main

Save it? [Y/n] Y

Security scan: ✓ Passed

Notes:
  ℹ️ Debug flag enabled - config/settings.py:12
     DEBUG = True

  ℹ️ Server binds to all interfaces - src/server.py:45
     app.run(host='0.0.0.0', port=8080)

Saving...
  Committed: feat(server): add HTTP server configuration
  Tagged: v1.6.0

Push to remote? [y/N]
```

---

## Example 15: Security Scan - Acknowledged Warning

```
$ /git-it

Checking environment... OK
Branch: main

=== SAVE POINT SUMMARY ===

Message:  chore: update deployment config
Version:  1.6.0 -> 1.6.1 (PATCH)

Save it? [Y/n] Y

Security scan: ⚠️ 1 warning (acknowledged)

  ⚠️ Private Key: SSH key file staged
     File: deploy/server.pem
     Status: Previously acknowledged (test key for CI)

Saving...
  Committed: chore: update deployment config
  Tagged: v1.6.1

Push to remote? [y/N]
```

---

## Example 16: Security Scan - PII Detection

```
$ /git-it

Checking environment... OK
Branch: feature-customer-export

Analyzing changes...
  Added:    scripts/export_customers.py (+89 lines)
  Added:    data/sample_customers.csv (+50 lines)

=== SAVE POINT SUMMARY ===

Message:  feat(scripts): add customer data export
Version:  1.6.1 -> 1.7.0 (MINOR)

Save it? [Y/n] Y

Security scan: ✓ Passed

Notes:
  ℹ️ Possible phone number - data/sample_customers.csv:12
     Pattern: (555) 123-4567

  ℹ️ Possible personal email - data/sample_customers.csv:15
     john.smith@gmail.com

  ℹ️ Credit card pattern (test) - scripts/export_customers.py:34
     4111-1111-1111-1111 (known test number, skipped)

Saving...
  Committed: feat(scripts): add customer data export
  Tagged: v1.7.0
```

---

## Example 17: Full Security Scan (All Files)

```
$ save my work with a full security scan

Checking environment... OK
Branch: main

Analyzing changes...
  Modified: src/config.py (+5, -2)

=== SAVE POINT SUMMARY ===

Message:  fix(config): update timeout value
Version:  1.7.0 -> 1.7.1 (PATCH)

Save it? [Y/n] Y

Running FULL security scan (all 47 tracked files)...

Security scan: ⚠️ 1 warning, 2 notes

  ⚠️ API Key: Possible hardcoded token (existing file)
     File: src/legacy/old_client.py:89
     Match: api_key = "sk_test_..."

Notes:
  ℹ️ Debug flag enabled - src/config.py:12
  ℹ️ Server binds to 0.0.0.0 - docker-compose.yml:15

The warning above is in an EXISTING file (not part of this commit).
This was found because you requested a full scan.

Continue with commit? [y/N] y

Add src/legacy/old_client.py to .security-ignored? [y/N] y
Reason (optional): Legacy test key, scheduled for removal

Added to .security-ignored

Saving...
  Committed: fix(config): update timeout value
  Tagged: v1.7.1
```

---

## Example 18: Version Sync Across Multiple Files

```
$ /git-it

Checking environment... OK
Branch: main

Analyzing changes...
  Modified: src/api/client.py (+45, -12)
  Modified: tests/test_client.py (+20, -5)

Scanning version references...
  Found: VERSION.txt (1.7.1)
  Found: package.json (1.7.1)
  Found: pyproject.toml (1.7.1)
  Found: src/__init__.py (1.7.1)

All versions in sync ✓

=== SAVE POINT SUMMARY ===

Message:  feat(api): add retry logic to client

          Co-Authored-By: Claude <noreply@anthropic.com>

Version:  1.7.1 → 1.8.0 (MINOR)
Files:    VERSION.txt, package.json, pyproject.toml, src/__init__.py
Tag:      v1.8.0
Branch:   main

Changes (2 files):
  Modified: src/api/client.py (+45, -12)
  Modified: tests/test_client.py (+20, -5)

Lines: +65, -17

Save it? [Y/n] Y

Security scan: ✓ Passed

Updating versions...
  ✓ VERSION.txt: 1.7.1 → 1.8.0
  ✓ package.json: 1.7.1 → 1.8.0
  ✓ pyproject.toml: 1.7.1 → 1.8.0
  ✓ src/__init__.py: 1.7.1 → 1.8.0

Saving...
  Committed: feat(api): add retry logic to client
  Tagged: v1.8.0

Push to remote? [y/N]
```

---

## Example 19: Version Mismatch Detection

```
$ /git-it

Checking environment... OK
Branch: main

Analyzing changes...
  Added: src/utils/helpers.py (+67 lines)

Scanning version references...
  Found: VERSION.txt (1.8.0)
  Found: package.json (1.7.0)
  Found: pyproject.toml (1.8.0)

⚠️ Version mismatch detected:
  VERSION.txt:    1.8.0
  package.json:   1.7.0  ← out of sync
  pyproject.toml: 1.8.0

Sync all to 1.9.0? [Y/n] Y

=== SAVE POINT SUMMARY ===

Message:  feat(utils): add helper functions

          Co-Authored-By: Claude <noreply@anthropic.com>

Version:  1.8.0 → 1.9.0 (MINOR)
Files:    VERSION.txt, package.json, pyproject.toml
Tag:      v1.9.0
Branch:   main

Changes (1 file):
  New: src/utils/helpers.py (+67 lines)

Lines: +67

Save it? [Y/n] Y

Security scan: ✓ Passed

Updating versions...
  ✓ VERSION.txt: 1.8.0 → 1.9.0
  ✓ package.json: 1.7.0 → 1.9.0 (was out of sync)
  ✓ pyproject.toml: 1.8.0 → 1.9.0

Saving...
  Committed: feat(utils): add helper functions
  Tagged: v1.9.0

Push to remote? [y/N]
```
