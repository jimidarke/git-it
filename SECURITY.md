# Git-It Security Scanning Reference

Security patterns, detection rules, and best practices. Referenced by SKILL.md Phase 9.5.

---

## Scan Timing

Security scan runs AFTER user confirmation (Phase 9) and BEFORE commit execution (Phase 10).

### Default Mode (Staged Files Only)

Scans ONLY files being committed - fast and focused on what's changing.

```bash
git diff --cached --name-only    # Get staged files
git diff --cached                # Get staged content for pattern matching
```

**Automatically excluded:**
- Unchanged committed files (already in repo history)
- Files in .gitignore (never tracked)
- Untracked files not staged for commit

### Full Scan Mode

Triggered by: **"save my work with a full security scan"**

Scans ALL tracked files in the repository - comprehensive but slower.

```bash
git ls-files                     # Get all tracked files
```

**Use full scan for:**
- Initial security audit of existing projects
- Periodic security reviews
- Before making a repository public
- After cloning/forking an unfamiliar codebase

---

## Output Levels

| Level | Icon | Trigger | Behavior |
|-------|------|---------|----------|
| **PASS** | `✓` | No issues | Quick confirmation, proceed |
| **INFO** | `ℹ️` | Low-priority items | Note displayed, auto-proceed |
| **WARN** | `⚠️` | High-priority items | Warn, ask to continue `[y/N]` |

### Output Formats

**No Issues Found:**
```
Security scan: ✓ Passed
```

**Informative Only:**
```
Security scan: ✓ Passed

Notes:
  ℹ️ Debug flag enabled - src/config.py:12
  ℹ️ Server binds to 0.0.0.0 - src/server.py:45
```

**Warnings Found:**
```
Security scan: ⚠️ 2 warnings

  ⚠️ API Key: AWS access key detected
     File: src/config.py:23
     Match: AKIAIOSFODNN7...

  ⚠️ Private Key: SSH key file staged
     File: keys/id_rsa

Recommendations:
  1. Move secrets to environment variables
  2. Add sensitive files to .gitignore
  3. If already committed, rotate credentials

Add to .gitignore? [Y/n] _
Continue with commit? [y/N] _
```

---

## "Acknowledge Once" Behavior

When a HIGH priority warning is found:
1. First occurrence: Prompt user `Continue with commit? [y/N]`
2. If user proceeds: Log to project-local `.security-ignored` file
3. Future scans: Still FLAG the warning, but never prompt again
4. User removes entry: Prompts restored for that pattern

See "Project-Local Ignore File" section for format.

---

## HIGH PRIORITY Patterns (Warning)

### API Keys & Tokens

| Type | Pattern | Example |
|------|---------|---------|
| AWS Access Key | `AKIA[0-9A-Z]{16}` | AKIAIOSFODNN7EXAMPLE |
| AWS Secret Key | `[A-Za-z0-9/+=]{40}` (near AWS context) | wJalrXUtnFEMI/K7MDENG... |
| GitHub Token | `gh[pousr]_[A-Za-z0-9]{36}` | ghp_xxxxxxxxxxxx... |
| GitHub Fine-Grained | `github_pat_[A-Za-z0-9]{22}_[A-Za-z0-9]{59}` | github_pat_xxx... |
| GitLab Token | `glpat-[A-Za-z0-9\-]{20,}` | glpat-xxxxxxxxxxxx |
| Slack Token | `xox[baprs]-[0-9A-Za-z\-]+` | xoxb-123456789... |
| Slack Webhook | `https://hooks\.slack\.com/services/T[A-Z0-9]+/B[A-Z0-9]+/[A-Za-z0-9]+` | |
| Stripe Live Key | `sk_live_[A-Za-z0-9]{24,}` | sk_live_xxxx... |
| Stripe Test Key | `sk_test_[A-Za-z0-9]{24,}` | sk_test_xxxx... |
| Google API Key | `AIza[A-Za-z0-9_\-]{35}` | AIzaSyxxxxxxxxx... |
| Twilio API Key | `SK[a-f0-9]{32}` | SKxxxxxxxx... |
| SendGrid Key | `SG\.[A-Za-z0-9_\-]{22}\.[A-Za-z0-9_\-]{43}` | SG.xxx.xxx |
| NPM Token | `npm_[A-Za-z0-9]{36}` | npm_xxxxxxxxxxxx... |
| PyPI Token | `pypi-[A-Za-z0-9_\-]{100,}` | pypi-xxxxxxx... |
| Generic API Key | `(?i)(api[_\-]?key\|api[_\-]?secret).*[=:].*['"]?[A-Za-z0-9_\-]{20,}['"]?` | api_key = "xxx" |
| Generic Secret | `(?i)(secret\|password\|passwd\|pwd).*[=:].*['"]?[^\s'"]{8,}['"]?` | password = "xxx" |

### Private Keys & Certificates

| Type | Detection |
|------|-----------|
| RSA Private Key | `-----BEGIN RSA PRIVATE KEY-----` |
| EC Private Key | `-----BEGIN EC PRIVATE KEY-----` |
| DSA Private Key | `-----BEGIN DSA PRIVATE KEY-----` |
| OpenSSH Private Key | `-----BEGIN OPENSSH PRIVATE KEY-----` |
| Generic Private Key | `-----BEGIN PRIVATE KEY-----` |
| Encrypted Private Key | `-----BEGIN ENCRYPTED PRIVATE KEY-----` |
| PGP Private Key | `-----BEGIN PGP PRIVATE KEY BLOCK-----` |
| SSH Key Files | `id_rsa`, `id_dsa`, `id_ecdsa`, `id_ed25519` (filenames) |
| Certificate Files | `*.pem`, `*.key`, `*.p12`, `*.pfx` (extensions) |

### Credential Files (by filename)

| Filename Pattern | Description |
|------------------|-------------|
| `.htpasswd` | Apache password file |
| `credentials.json` | Generic credentials |
| `credentials.yaml`, `credentials.yml` | Generic credentials |
| `secrets.json` | Secrets file |
| `secrets.yaml`, `secrets.yml` | Secrets file |
| `.netrc` | FTP/HTTP credentials |
| `.npmrc` (with `_auth` or `_authToken`) | NPM auth tokens |
| `shadow` | Unix password shadows |

### Database Connection Strings

| Pattern | Example |
|---------|---------|
| `mongodb://[^:]+:[^@]+@` | mongodb://user:pass@host |
| `postgres://[^:]+:[^@]+@` | postgres://user:pass@host |
| `postgresql://[^:]+:[^@]+@` | postgresql://user:pass@host |
| `mysql://[^:]+:[^@]+@` | mysql://user:pass@host |
| `redis://:[^@]+@` | redis://:password@host |
| `amqp://[^:]+:[^@]+@` | amqp://user:pass@host |

### URI Strings with Embedded Credentials

| Pattern | Example |
|---------|---------|
| `sftp://[^:]+:[^@]+@` | sftp://user:pass@server.com |
| `ftp://[^:]+:[^@]+@` | ftp://user:pass@files.example.com |
| `https?://[^:]+:[^@]+@` | https://admin:secret@api.example.com |
| `ssh://[^:]+:[^@]+@` | ssh://deploy:key@server.com |
| `s3://[^:]+:[^@]+@` | s3://key:secret@bucket |

Detects credentials embedded in any URI scheme via `user:password@` pattern.

### Tokens

| Type | Pattern |
|------|---------|
| JWT | `eyJ[A-Za-z0-9_\-]*\.eyJ[A-Za-z0-9_\-]*\.[A-Za-z0-9_\-]*` |
| Bearer Token | `Bearer\s+[A-Za-z0-9_\-\.]+` (in string literals) |
| Basic Auth | `Basic\s+[A-Za-z0-9+/=]+` (in string literals) |

---

## LOW PRIORITY Patterns (Informative)

### Network Exposure

| Pattern | Description |
|---------|-------------|
| `0\.0\.0\.0:` | Binding to all network interfaces |
| `host\s*=\s*['"]0\.0\.0\.0['"]` | Flask/Python all-interface binding |
| `\.listen\s*\(\s*\d+` | Server listening on port |
| `EXPOSE\s+\d+` | Docker port exposure (in Dockerfile) |
| `bind_address:\s*0\.0\.0\.0` | Config all-interface binding |

### Debug/Development Flags

| Pattern | Description |
|---------|-------------|
| `DEBUG\s*=\s*[Tt]rue` | Python debug mode |
| `debug\s*:\s*true` | YAML debug mode |
| `"debug"\s*:\s*true` | JSON debug mode |
| `NODE_ENV.*development` | Node.js dev mode |
| `FLASK_DEBUG\s*=\s*1` | Flask debug mode |
| `DJANGO_DEBUG\s*=\s*[Tt]rue` | Django debug mode |

### PII - Personal Identifiable Information (US & Canada)

#### Credit Card Numbers (Luhn validated)

| Card Type | Pattern | Digits |
|-----------|---------|--------|
| Visa | `4[0-9]{12}(?:[0-9]{3})?` | 13, 16, or 19 |
| Mastercard | `5[1-5][0-9]{14}` | 16 |
| American Express | `3[47][0-9]{13}` | 15 |
| Discover | `6(?:011\|5[0-9]{2})[0-9]{12}` | 16 |

**Requirements:**
- Must pass Luhn checksum validation
- Allow spaces/dashes between groups: `4111-1111-1111-1111`

**Excluded test numbers:**
- `4111111111111111` (Visa test)
- `5500000000000004` (Mastercard test)
- `378282246310005` (Amex test)
- `6011111111111117` (Discover test)

#### US Social Security Numbers

| Pattern | Format |
|---------|--------|
| `[0-9]{3}-[0-9]{2}-[0-9]{4}` | 123-45-6789 |
| `[0-9]{3}\s[0-9]{2}\s[0-9]{4}` | 123 45 6789 |

**Excluded invalid ranges:** Area numbers 000, 666, 900-999

#### Canadian Social Insurance Numbers (SIN)

| Pattern | Format |
|---------|--------|
| `[0-9]{3}-[0-9]{3}-[0-9]{3}` | 123-456-789 |
| `[0-9]{3}\s[0-9]{3}\s[0-9]{3}` | 123 456 789 |

**Requirements:**
- Must pass Luhn checksum validation
- Exclude first digits 0 and 8 (invalid)

#### Phone Numbers (US & Canada)

| Pattern | Format |
|---------|--------|
| `\+?1?[-.\s]?\(?[0-9]{3}\)?[-.\s]?[0-9]{3}[-.\s]?[0-9]{4}` | +1 (555) 123-4567 |

**Context-aware:** Flag when near keywords `phone`, `tel`, `mobile`, `contact`, `cell`

**Excluded:** `555-*` area codes (fictional numbers)

#### Email Addresses (Personal)

| Pattern |
|---------|
| `[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}` |

**Excluded prefixes (generic/functional):**
`info@`, `test@`, `web@`, `admin@`, `accounting@`, `support@`, `noreply@`, `no-reply@`, `contact@`, `sales@`, `help@`, `webmaster@`, `postmaster@`, `hostmaster@`, `abuse@`, `security@`, `billing@`, `hello@`, `team@`, `dev@`

**Excluded domains:**
`@example.com`, `@test.com`, `@localhost`, `@127.0.0.1`

Only flag emails with personal (non-generic) local parts.

#### Physical Addresses (Heuristic)

**US Pattern:** Number + Street name + suffix
- Suffixes: Ave, St, Rd, Blvd, Dr, Ln, Way, Ct, Pl, Ter, Cir

**Canada Pattern:** Number + Street + postal code
- Postal code: `[A-Z][0-9][A-Z]\s?[0-9][A-Z][0-9]`

**Context-aware:** Flag when near keywords `address`, `location`, `ship`, `deliver`

---

## Exclusions (False Positive Reduction)

### Auto-Excluded Files & Paths

- `*.example`, `*.sample`, `*.template` - Template files
- `test/`, `tests/`, `__tests__/`, `spec/` - Test directories
- `fixtures/`, `mock*/`, `fake*/`, `stub*/` - Test data
- `docs/`, `*.md` - Documentation (code fences)
- `package-lock.json`, `yarn.lock`, `Cargo.lock` - Lock files

### Known Test Data

| Type | Values |
|------|--------|
| Credit Cards | `4111111111111111`, `5500000000000004`, `378282246310005` |
| SSN | `123-45-6789`, `000-00-0000` |
| Phone | `555-*` area codes |
| Email | `*@example.com`, `test@*`, `user@localhost` |

### Context Exclusions

- Patterns inside comments (`//`, `#`, `/* */`)
- Variables named `example*`, `test*`, `mock*`, `fake*`, `sample*`
- Inside markdown code fences in `.md` files

---

## Luhn Algorithm

Used to validate credit card numbers and Canadian SINs.

**Algorithm:**
1. From rightmost digit, double every second digit
2. If doubling results in >9, subtract 9
3. Sum all digits
4. Valid if sum % 10 == 0

**Example validation for 4111111111111111:**
```
Original: 4 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
Doubled:  8 1 2 1 2 1 2 1 2 1 2 1 2 1 2 1
Sum: 8+1+2+1+2+1+2+1+2+1+2+1+2+1+2+1 = 30
30 % 10 = 0 ✓ Valid
```

---

## Project-Local `.security-ignored` File

When user acknowledges a warning, create/append to `.security-ignored` in project root.

### File Format

```
# Security Ignored Patterns
# Generated by git-it security scan
# Remove entries to restore prompts
# Format: <file_pattern>|<pattern_type>|<reason>

config/api.py|AWS_KEY|Test credentials for local dev
keys/deploy.pem|PRIVATE_KEY|Deploy key managed separately
*.env.local|ENV_FILE|Local environment only
```

### Pattern Types

| Type | Description |
|------|-------------|
| `AWS_KEY` | AWS access key or secret |
| `GITHUB_TOKEN` | GitHub token |
| `PRIVATE_KEY` | SSH/RSA/EC private key |
| `CREDENTIAL_FILE` | Credentials file |
| `DB_URL` | Database connection string |
| `URI_CREDS` | URI with embedded credentials |
| `API_TOKEN` | Generic API token |
| `JWT` | JSON Web Token |
| `ENV_FILE` | Environment file |

### Glob Support

File patterns support glob syntax:
- `*.env` - All .env files
- `config/*.json` - JSON files in config/
- `keys/*` - All files in keys/

### Behavior

1. On warning, check `.security-ignored` for matching entry
2. If match found: Display warning but skip prompt
3. If no match: Prompt `[y/N]` and offer to add entry
4. Entry removed: Prompts restored

### Auto-Gitignore

When `.security-ignored` is first created:
1. Check if `.gitignore` exists (create if not)
2. Check if `.security-ignored` is already listed
3. If not listed, append `.security-ignored` to `.gitignore`

**Rationale:** Acknowledged security patterns should never be broadcast to public repositories.
The `.security-ignored` file documents what you've chosen to accept - exposing this could reveal sensitive file locations or acknowledged vulnerabilities.

---

## Best Practices

### For Developers

1. **Never commit secrets** - Use environment variables or secret managers
2. **Use .env.example** - Template with empty values for documentation
3. **Rotate exposed secrets** - If committed, assume compromised
4. **Review git history** - Use `git log -p` to audit past commits

### Secret Management Alternatives

| Tool | Use Case |
|------|----------|
| Environment variables | Simple apps, local dev |
| .env files (gitignored) | Local development |
| AWS Secrets Manager | AWS deployments |
| HashiCorp Vault | Enterprise, multi-cloud |
| 1Password CLI | Team secrets sharing |
| doppler.com | Cross-platform secrets |

### If Secrets Were Already Committed

```
WARNING: Git history is permanent. If you pushed secrets:
1. Rotate ALL exposed credentials immediately
2. Consider using BFG Repo-Cleaner or git-filter-repo
3. Force push required (coordinate with team)
4. Assume the secret is compromised regardless
```

---

## Integration with .gitignore Workflow

When warnings involve files that should be ignored:

1. Offer to add file patterns to `.gitignore`
2. If accepted:
   - Add pattern to `.gitignore`
   - Unstage the file: `git reset HEAD <file>`
   - Re-run security scan on remaining staged files

Common patterns to suggest:
- `.env` - Environment files
- `*.pem`, `*.key` - Key files
- `credentials.*` - Credential files
- `secrets.*` - Secret files
