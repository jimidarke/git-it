# Git-It Patterns Reference

All patterns for .gitignore, commit messages, and VERSION.txt format. Referenced by SKILL.md.

---

## .gitignore Patterns

Scan untracked files for these patterns:

| Pattern | Description | Language/Tool |
|---------|-------------|---------------|
| `__pycache__/` | Bytecode cache | Python |
| `*.pyc` | Compiled files | Python |
| `*.pyo` | Optimized bytecode | Python |
| `.env` | Environment secrets | All (NEVER commit!) |
| `.env.local` | Local env overrides | All |
| `node_modules/` | Dependencies | Node.js |
| `package-lock.json` | Lock file (optional) | Node.js |
| `.DS_Store` | Finder metadata | macOS |
| `Thumbs.db` | Thumbnail cache | Windows |
| `*.log` | Log files | All |
| `*.log.*` | Rotated logs | All |
| `venv/` | Virtual environment | Python |
| `.venv/` | Virtual environment | Python |
| `env/` | Virtual environment | Python |
| `.cache/` | Cache directory | All |
| `.tmp/` | Temporary files | All |
| `tmp/` | Temporary files | All |
| `dist/` | Build output | All |
| `build/` | Build output | All |
| `out/` | Build output | All |
| `*.egg-info/` | Package metadata | Python |
| `.idea/` | IDE settings | JetBrains |
| `.vscode/` | IDE settings | VS Code |
| `*.swp` | Swap files | Vim |
| `*.swo` | Swap files | Vim |
| `*~` | Backup files | Various editors |
| `coverage/` | Test coverage | JavaScript |
| `.nyc_output/` | NYC coverage | JavaScript |
| `htmlcov/` | Coverage HTML | Python |
| `.coverage` | Coverage data | Python |
| `*.cover` | Coverage files | Python |
| `target/` | Build output | Rust/Java |
| `Cargo.lock` | Lock file (libs only) | Rust |
| `vendor/` | Dependencies | Go/PHP |
| `.gradle/` | Build cache | Gradle |
| `*.class` | Compiled classes | Java |
| `*.jar` | Archives (usually) | Java |

### Rules
- Only suggest patterns matching actual untracked files
- Check existing .gitignore to avoid duplicates
- Never add catch-all patterns unprompted
- Always ask before adding: "Add these to .gitignore? [Y/n]"

### Secrets Warning
```
WARNING: Found .env file with potential secrets!

Never commit secrets to git - they become part of permanent history.
Recommendation: Add ".env" to .gitignore

Add .env to .gitignore? [Y/n]
```

---

## Commit Message Patterns

### Format
```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

### Type Detection

Analyze changed files to determine commit type:

| File Pattern | Type | Priority |
|--------------|------|----------|
| BREAKING in diff/message | triggers MAJOR | 1 |
| New source files (not tests) | `feat` | 2 |
| `test_*`, `*.test.*`, `*.spec.*` | `test` | 3 |
| `spec/`, `__tests__/`, `tests/` | `test` | 3 |
| `README*`, `*.md`, `docs/` | `docs` | 4 |
| `CHANGELOG*`, `LICENSE*` | `docs` | 4 |
| `.github/`, `.gitlab-ci*` | `ci` | 5 |
| `Jenkinsfile`, `azure-pipelines*` | `ci` | 5 |
| `*.yml` (CI context) | `ci` | 5 |
| `package.json`, `package-lock.json` | `build` | 6 |
| `requirements.txt`, `setup.py` | `build` | 6 |
| `Cargo.toml`, `go.mod`, `pom.xml` | `build` | 6 |
| Bug fix indicators in diff | `fix` | 7 |
| Style/formatting only | `style` | 8 |
| Code reorganization | `refactor` | 9 |
| Default | `chore` | 10 |

### Scope Detection

```bash
# Find most-changed directory
git diff --stat | awk -F'|' '{print $1}' | xargs -I{} dirname {} 2>/dev/null | sort | uniq -c | sort -rn
```

| Scenario | Scope |
|----------|-------|
| Single directory changed | Use directory name |
| Single file changed | Use filename (no extension) |
| Multiple directories | Common parent or omit |
| Root files only | Omit scope |

### Description Guidelines

- Imperative mood: "add", "fix", "update" (not "added", "fixed")
- Under 50 characters
- No period at end
- Specific but concise
- Lowercase first letter

### Examples
```
feat(auth): add OAuth2 login flow
fix(parser): handle null input gracefully
docs: update README with examples
chore: initial commit
test(api): add integration tests for users endpoint
build: upgrade dependencies to latest versions
ci: add GitHub Actions workflow
refactor(utils): simplify date formatting logic
style: fix indentation in config files
perf(query): optimize database lookups
```

---

## Conventional Commits Types

| Type | Description | Version Impact |
|------|-------------|----------------|
| `feat` | New feature | MINOR |
| `fix` | Bug fix | PATCH |
| `docs` | Documentation only | PATCH |
| `style` | Formatting, no logic change | PATCH |
| `refactor` | Code change, no feature/fix | PATCH |
| `perf` | Performance improvement | PATCH |
| `test` | Adding/fixing tests | PATCH |
| `build` | Build system, dependencies | PATCH |
| `ci` | CI configuration | PATCH |
| `chore` | Maintenance tasks | PATCH |

### Breaking Changes

Indicate with `!` after type or BREAKING CHANGE footer:

```
feat(api)!: change authentication endpoint

BREAKING CHANGE: /auth/login now requires API key header
```

Breaking changes always trigger MAJOR version bump.

---

## Version Bump Logic

| Bump | Indicators |
|------|-----------|
| **MAJOR** | BREAKING keyword, deleted public files, changed signatures, major restructure |
| **MINOR** | New files (not tests), new exports/functions, `feat:` type |
| **PATCH** | Bug fixes, docs, tests, style, <20 lines changed, `fix:`/`chore:`/`docs:` |

### Detection Commands

```bash
# Check line count for PATCH threshold
git diff --numstat | awk '{sum+=$1+$2} END {print sum}'

# Check for new files (MINOR indicator)
git status --porcelain | awk '/^\?\?|^A / && !/test/' | wc -l

# Check for deleted files (potential MAJOR)
git status --porcelain | awk '/^D /' | wc -l
```

### Version Calculation

```
Current: X.Y.Z
MAJOR:   (X+1).0.0
MINOR:   X.(Y+1).0
PATCH:   X.Y.(Z+1)
New:     0.1.0
```

---

## VERSION.txt Format

The skill creates and maintains VERSION.txt in project root:

```
1.2.3

## Changelog

### 1.2.3 (2025-12-29)
Fix null pointer in parser.

Commits:
- abc1234 fix(parser): handle null input gracefully

### 1.2.2 (2025-12-28)
Update documentation.

Commits:
- def5678 docs: update README with examples
```

### Format Rules
- Line 1: Current version (X.Y.Z) - no prefix
- Line 2: Blank
- Line 3: `## Changelog` header
- Entries in reverse chronological order
- Each entry format:
  ```
  ### X.Y.Z (YYYY-MM-DD)
  Brief description of changes.

  Commits:
  - <hash> <commit message>
  ```

### Tag Format
- Always prefix with `v`: `v1.2.3`
- Annotated tags with commit message: `git tag -a v1.2.3 -m "commit message"`
