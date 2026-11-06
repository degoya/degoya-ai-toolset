---
description: Run all applicable degoya-ai-toolset checks (legal, privacy, accessibility, security, performance, i18n, SEO/GEO, text, tests, API, licences, plan) over the current project and produce one consolidated report.
argument-hint: "[all|content|code|compliance|plan] [areas] [path] [--quick] – no argument opens a selection menu"
---

# /audit

Run a consolidated quality audit of this project using the skills of the degoya-ai-toolset plugin.

Arguments: `$ARGUMENTS`

## 1. Parse arguments

- **Group** (optional, first argument): runs a predefined set of areas.

  | Group | Areas | Typical audience |
  |---|---|---|
  | `content` | `text`, `seo`, `i18n` (content side), `a11y` (content side: alt texts, headings, link texts, plain language) | marketing, editors, UX writers |
  | `code` | `security`, `perf`, `tests`, `api`, `i18n` (code side), `a11y` (code side: semantics, keyboard, ARIA) | developers |
  | `compliance` | `legal`, `dpia`, `licenses`, `a11y` (EAA/BFSG conformance and statement) | management, legal, DPO |
  | `plan` | `plan` | PO, team |
  | `all` (default) | every applicable area | everyone |

  Groups and areas can be combined, e.g. `/audit code licenses`. Areas belonging to several groups are checked only from that group's angle.
- **Areas** (comma-separated, optional): `plan`, `legal`, `dpia`, `text`, `a11y`, `i18n`, `seo`, `security`, `perf`, `tests`, `api`, `licenses`.
- **Path** (optional): limit the audit to this directory or file.
- `--quick`: only automated checks and the top findings per area (max. 5), no deep manual review.

If no group or area is given, do **not** assume `all`: show the interactive selection in step 1a. `all` must be passed explicitly to skip the menu.

## 1a. Interactive selection (when called without group/area)

Use the `AskUserQuestion` tool so the user can pick with arrow keys. Write questions and labels in the user's language if it is clear from the conversation, otherwise in English.

**Question 1** (single choice, header "Audit"): "What would you like to audit?"
- **Full audit (Recommended)** – all applicable areas
- **By group** – content, code, compliance or plan
- **Individual checks** – pick single checks
- **Quick scan** – all areas, automated checks and top 5 findings each (`--quick`)

**If "By group"**, ask (multiSelect, header "Groups"): "Which groups?"
- **Content** – copy, SEO/GEO, i18n content, accessible content
- **Code** – security, performance, tests, API, i18n/a11y in code
- **Compliance** – legal, DPIA, licences, EAA/BFSG conformance
- **Plan** – plans, specs, roadmap, acceptance criteria

**If "Individual checks"**, ask these four questions in one call (all multiSelect; the user may leave a question empty):
1. header "Content": Text review (`text`) · SEO/GEO (`seo`) · i18n (`i18n`) · Accessibility (`a11y`)
2. header "Code": Security (`security`) · Performance (`perf`) · Tests (`tests`) · API design (`api`)
3. header "Compliance": Legal/GDPR/AI Act (`legal`) · DPIA (`dpia`) · Licences (`licenses`) · Plan review (`plan`)
4. header "Depth" (single choice): Full review (Recommended) · Quick scan

If nothing at all was selected, ask again once; then stop.

After the selection, state in one line what will run (e.g. "Running: security, perf, licences – full review") and continue. If the `AskUserQuestion` tool is not available, show the same options as a numbered list and wait for the answer.

## 2. Detect the project

Inspect the repository (README, CLAUDE.md, package manifests, framework config, `docs/`, routes, mobile project files) and determine:
- Type: website, web app, SaaS, API/backend, mobile app, library, CLI.
- Stack and languages, target markets/locales, whether personal data or AI features are involved.
- Existing reports, Definition of Done, style guides, glossaries.

## 3. Select areas

Use the requested areas, or select automatically:

| Area | Skill | Run when |
|---|---|---|
| `plan` | `plan-review` (post-review mode) | plan, spec, roadmap or ticket documents exist |
| `legal` | `eu-legal-compliance` | public website/app, personal data, third-party assets, AI features |
| `dpia` | `dsfa` (threshold analysis only) | personal data is processed |
| `text` | `text-review` | user-facing copy exists (UI strings, content, store texts) |
| `a11y` | `accessibility` | any user interface |
| `i18n` | `i18n` | UI exists; full depth if more than one locale or planned |
| `seo` | `seo-geo` | public website or web app with public pages |
| `security` | `security-check` | always |
| `perf` | `performance` | UI, API or backend exists |
| `tests` | `test-strategy` (review of existing tests) | always |
| `api` | `api-design-review` | API, OpenAPI/GraphQL schema or webhooks exist |
| `licenses` | `license-check` | dependencies or third-party assets exist |

Skip an area only with a one-line reason. `release-notes` and `commit-pr-text` are generators, not audits, and are not part of `/audit`.

## 4. Run the checks

For each selected area, apply the corresponding skill's checklist and rules exactly as the skill describes them (load the skill, do not work from memory).

- If a subagent/Task tool is available, run independent areas in parallel, one subagent per area, each given: project summary from step 2, scope/path, the skill name to load, `--quick` if set, and the instruction to return findings in the table format below. Otherwise run them one after another.
- Run safe, read-only automated tools when available (e.g. dependency audit, licence scan, axe/Lighthouse against a local dev server). Never run anything destructive, never send data to external services without asking.
- **Keep the repo clean:** before starting, record `git status --porcelain`. Run all tools from a temp directory outside the repo (`mktemp -d`), with Chrome profiles (`--user-data-dir`) and outputs in that temp directory – see the "Workspace hygiene" section of the skills. Subagents must follow the same rule. After all checks, compare `git status --porcelain` with the recorded state, delete anything the audit created outside `audit/`, delete the temp directory, and list the clean-up in the report.
- Every finding must point to a concrete location (file:line, URL, screen) and carry a fix suggestion.

## 5. Consolidate

Write the report to `audit/AUDIT-REPORT-<group>-<YYYY-MM-DD>.md` (create the folder if needed) and show the executive summary in the chat.

```markdown
# Project audit – <project> – <date>

Scope: <path or whole repo> · Group: <group> · Areas: <list> · Mode: full | quick
Project: <type, stack, locales>

## Executive summary
<3–5 sentences: overall state, biggest risks, go/no-go for release if relevant>

| Area | Critical | High | Medium | Low | Status |
|---|---|---|---|---|---|
| Security | 0 | 2 | 3 | 1 | needs work |

## Top 10 actions
| # | Area | Finding | Location | Severity | Fix | Effort (S/M/L) |

## Findings by area
### <Area>
<the skill's own report format>

## Skipped areas and not checked
<area – reason>

## Open questions
<question – addressed to (PO/Dev/QA/Legal/DPO)>
```

Severity scale across all areas: **Critical** (legal violation, exploitable vulnerability, data loss, blocks a user group completely), **High**, **Medium**, **Low**.

## 6. Rules

- Report in the language the user writes in.
- No invented facts; mark assumptions and anything that needs a human expert (lawyer, DPO, accessibility auditor).
- Do not fix anything during the audit. At the end, offer to fix the top actions or open issues for them.
- Deduplicate findings that several areas report (e.g. third-party tracking script appears in legal, privacy and performance): list once, tag all areas.
