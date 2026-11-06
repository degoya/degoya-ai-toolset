---
name: accessibility
description: Audits websites, web apps, mobile apps, designs and documents for accessibility against WCAG 2.2 AA, EN 301 549 and the European Accessibility Act (German BFSG), in code and design, with concrete fixes. Use for "check accessibility", "a11y audit", "WCAG check", "EAA/BFSG compliance", "screen reader support", "check contrast", "barrierefrei".
---

# Accessibility (WCAG 2.2 AA / EAA)

Goal: everyone can perceive, operate and understand the product, and it works with assistive technologies (POUR principles).

## Legal frame (short)
- **European Accessibility Act** (Directive (EU) 2019/882), applicable since 28 June 2025; in Germany implemented by the **BFSG**. Covers e.g. e-commerce and online services for consumers. Micro-enterprises are exempt for services.
- Technical standard: **EN 301 549**, which references **WCAG 2.1 AA** for the web. Recommend WCAG 2.2 AA as the target (it includes 2.1).
- Public sector: Web Accessibility Directive (EU) 2016/2102 (DE: BITV 2.0).
- Required: an accessibility statement/information about the service.

## Workflow
1. **Scope:** pages/screens, key flows (sign-up, search, checkout, contact), platform (web, iOS, Android).
2. **Automated checks** when code/URL is available: axe-core (`@axe-core/cli`, `@axe-core/playwright`), Lighthouse, Pa11y; Android Accessibility Scanner, Xcode Accessibility Inspector. Automation finds only about 30–40 % of issues.
3. **Manual checks** with [references/wcag-checklist.md](references/wcag-checklist.md): keyboard, screen-reader semantics, zoom/reflow, contrast, forms, media.
4. **Report** with fixes.

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

## Report format
```
## Accessibility audit: <subject>
Standard: WCAG 2.2 AA · Scope: <pages/flows> · Method: <automated/manual/tools>

### Summary
<conformance: full / partial / none; number of findings per level>

### Findings
| # | Location | Issue | WCAG criterion | Level | Affected users | Fix (code) |

### Not checked
<what cannot be judged without real users/assistive tech>
```
**Levels:** Critical (blocks a user group completely, e.g. checkout not keyboard-operable), High, Medium, Low.

## Rules
- Native HTML/native components first, ARIA only when needed ("No ARIA is better than bad ARIA").
- Fixes as concrete code in the project's stack.
- Never claim conformance that was not tested. Recommend real tests with screen readers (NVDA, JAWS, VoiceOver, TalkBack) and disabled users.
- Overlays/"accessibility widgets" are not a solution and do not create conformance.
- Write the report in the user's language.
