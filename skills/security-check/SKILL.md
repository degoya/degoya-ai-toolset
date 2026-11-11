---
name: security-check
description: 'Security review of plans, code, configuration and dependencies for web, app and API projects based on OWASP Top 10, OWASP ASVS and MASVS (mobile): auth, access control, input handling, secrets, dependencies, headers, cloud configuration and LLM-specific risks. Use for "security review", "is this secure?", "find vulnerabilities", "OWASP check", "pentest preparation", "check for prompt injection".'
---

# Security check

Find exploitable weaknesses before others do. Focus on real, evidenced risks with a concrete attack scenario, not theoretical style issues.

## Workflow
1. **Map the attack surface:** entry points (routes, APIs, forms, uploads, webhooks, deep links), trust boundaries, sensitive data, roles.
2. **Automated checks** where available: dependency audit (`npm audit`, `composer audit`, `pip-audit`, `osv-scanner`), secret scan (`gitleaks`, `trufflehog`), SAST (`semgrep`). Filter results for relevance.
3. **Manual review** with the checklist. For every finding, walk through the attack scenario and show the affected code.
4. **Report.**

## Workspace hygiene (mandatory when running tools)

Never leave tool artefacts in the user's project. Only `audit/` reports (and files the user asked for) may be written to the repo.

- Run browser-based and scanning tools (Lighthouse, Chrome/Puppeteer/Playwright, axe CLI, Pa11y, SBOM/licence scanners) from a fresh temp directory **outside** the repo, and pass all output paths as absolute paths:
  ```bash
  AUDIT_TMP="$(mktemp -d "${TMPDIR:-/tmp}/degoya-audit.XXXXXX")"
  (cd "$AUDIT_TMP" && npx lighthouse "$URL" --quiet \
     --chrome-flags="--headless=new --user-data-dir=$AUDIT_TMP/chrome-profile" \
     --output=json --output-path="$AUDIT_TMP/lighthouse.json")
  # read results from $AUDIT_TMP, then:
  rm -rf "$AUDIT_TMP"
  ```
- **WSL:** Lighthouse/chrome-launcher may start Windows Chrome and build its profile path from Windows env vars that are empty in WSL, creating folders like `undefined:\Users\undefined\AppData\Local\lighthouse.*` in the current directory. Running from `$AUDIT_TMP` keeps them out of the repo; alternatively point `CHROME_PATH` to a Linux Chrome/Chromium.
- Before finishing, run `git status --porcelain` and compare with the state before the run. Remove anything the run created outside `audit/` (only files you created – never touch the user's own changes) and report what was cleaned up.
- Do not install dependencies into the project (`npm install` in the repo, editing `package.json`/lockfiles) to run a check; use `npx`/temporary installs instead.

## Checklist
**Access control (OWASP A01)**
- [ ] Every route/API checks authentication and authorisation server-side; never frontend-only.
- [ ] Object access checked against owner/tenant (IDOR: `/api/orders/123` of another user).
- [ ] Admin functions separated and protected; least privilege for service accounts.

**Cryptography and secrets (A02)**
- [ ] TLS everywhere, HSTS; passwords hashed with Argon2id/bcrypt/scrypt; no home-made crypto.
- [ ] No secrets in the repo, client bundle, app binary or logs; rotation possible.

**Injection (A03)**
- [ ] Parameterised queries/ORM; no string concatenation for SQL, NoSQL, shell, LDAP.
- [ ] Output encoding against XSS; no `innerHTML`/`v-html`/`dangerouslySetInnerHTML` with user data without a sanitiser; Content Security Policy.

**Design and configuration (A04/A05)**
- [ ] Rate limiting for login, password reset, expensive endpoints; protection against user enumeration.
- [ ] Security headers: CSP, `X-Content-Type-Options`, `Referrer-Policy`, `frame-ancestors`; cookies `Secure`, `HttpOnly`, `SameSite`.
- [ ] Restrictive CORS, debug/stack traces off in production, default accounts removed.
- [ ] Cloud: no public buckets, minimal network rules.

**Components (A06)**
- [ ] Dependencies up to date, known CVEs assessed, lockfile committed (see also skill `license-check`).

**Authentication (A07)**
- [ ] MFA available, secure session handling (rotation after login, expiry, logout invalidates server-side), JWT signature and `alg` verified.

**Integrity (A08)**
- [ ] No unsafe deserialisation; webhooks signature-verified; CI/CD protected (branch protection, pinned actions).

**Logging (A09)**
- [ ] Security-relevant events logged without passwords, tokens or personal data in clear text; alerting.

**SSRF (A10)**
- [ ] Server-side requests to user-supplied URLs only via allowlist; internal addresses (169.254.169.254, localhost, private ranges) blocked.

**Uploads**
- [ ] Type checked by content, size limit, stored outside the web root, random names, never executed.

**Mobile (OWASP MASVS)**
- [ ] Sensitive data in Keychain/Keystore, not SharedPreferences/UserDefaults; no clear-text traffic; deep links validated; no secrets in the binary.

**LLM/AI features (OWASP Top 10 for LLM Applications)**
- [ ] Prompt injection (direct and indirect via documents/websites) considered; model output treated as untrusted.
- [ ] Tools/agents with minimal permissions, confirmation for consequential actions.
- [ ] No secrets or other users' data in the context; output encoded before rendering.
- [ ] Cost/abuse limits.

## Report format
```
## Security check: <subject>
### Summary
### Findings
| # | Location | Weakness | Category (OWASP) | Severity (Critical/High/Medium/Low) | Attack scenario | Fix |
### Not checked
```
Severity roughly by exploitability × impact (CVSS logic).

## Rules
- Only report findings with a plausible attack path; mark uncertain ones "to verify".
- Never run exploits against third-party systems. Test only your own environments with permission.
- Do not repeat real secrets in the report; name the location and recommend rotation.
- Write the report in the user's language.
