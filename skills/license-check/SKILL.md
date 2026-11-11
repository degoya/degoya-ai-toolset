---
name: license-check
description: 'Checks open-source dependencies, embedded assets (fonts, icons, images) and copied code for licences and their obligations: compatibility with the business model (proprietary, SaaS, app store), copyleft (GPL, LGPL, AGPL, MPL), attribution, NOTICE files, SBOM. Use for "check licences", "can we use this library?", "GPL problem?", "open-source compliance", "create an SBOM", "third-party notices".'
---

# Licence check

Pre-assessment of licence compliance. Not legal advice; involve a professional for copyleft conflicts or unclear licences.

## Workflow
1. **Clarify the distribution model:** proprietary software, SaaS (server only), distributed software (desktop, mobile app, firmware, JS shipped to browsers), open source (which licence?). Obligations depend on it.
2. **Build an inventory**, including transitive dependencies:
   - JS: `npx license-checker --summary` or `npx license-checker-rseidelsohn`
   - PHP: `composer licenses`
   - Python: `pip-licenses`
   - Go/Rust/Java/.NET: `go-licenses`, `cargo deny`, Gradle/Maven licence plugins, `dotnet-project-licenses`
   - Cross-ecosystem/SBOM: `syft`, `cdxgen`, ScanCode, ORT, FOSSA (CycloneDX or SPDX)
   - Manually as well: fonts, icon sets, images, videos, copied code snippets, AI-generated code with recognisable copies.
3. **Classify** using the table below and check against the distribution model.
4. **Derive obligations** (attribution, ship licence texts, offer source code, mark changes).
5. **Report.**

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

## Licence classes
| Class | Examples | Typical obligations | Risk for proprietary products |
|---|---|---|---|
| Permissive | MIT, BSD-2/3, ISC, Apache-2.0, Zlib | ship copyright + licence text; Apache: NOTICE, mark changes, patent clause | low |
| Weak copyleft | LGPL-2.1/3.0, MPL-2.0, EPL-2.0 | disclose changes to the library; LGPL: replaceability (dynamic linking) | medium, depends on integration |
| Strong copyleft | GPL-2.0, GPL-3.0 | derivative work under GPL incl. source when distributed | high for distributed software |
| Network copyleft | AGPL-3.0, SSPL | like GPL, also when used over a network (SaaS) | high, including SaaS |
| Non-commercial / source-available | CC BY-NC, BUSL, Elastic License, Commons Clause | usage restrictions | often incompatible |
| No licence | code without LICENSE | all rights reserved | not usable without permission |
| Content | CC BY, CC BY-SA, CC0, OFL (fonts) | attribution, share-alike | depends on licence |

Notes:
- GPL software in app stores is problematic because of store terms.
- Stack Overflow snippets: CC BY-SA (version depends on date).
- Dual licences: document which one you chose.

## Report format
```
## Licence check: <project>
Distribution model: … · Tool: … · Number of components: …
### Summary
### Critical findings
| Component | Version | Licence | Integration (direct/transitive) | Problem | Recommendation (replace/isolate/buy licence/clarify) |
### Obligations
- Generate and ship third-party notices (e.g. "Open-source licences" in app/footer)
- …
### Unclear licences
```

## Rules
- Licence metadata in packages can be wrong; for critical cases check the LICENSE file in the source repo.
- For vulnerabilities in dependencies refer to skill `security-check`.
- Enforce in CI permanently (allow/deny lists, e.g. `cargo deny`, `license-checker --onlyAllow`, ORT).
- Write the report in the user's language.
