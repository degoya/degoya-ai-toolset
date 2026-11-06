# degoya-ai-toolset

A Claude Code plugin with review and quality skills for software, web and app projects.
Run every applicable check at once with **`/audit`**, pick groups or single checks from a menu, or use each skill on its own.

- [Requirements](#requirements)
- [Installation](#installation)
- [Updating and removing](#updating-and-removing)
- [Quick start](#quick-start)
- [The `/audit` command](#the-audit-command)
- [Using single skills](#using-single-skills)
- [Skill reference](#skill-reference)
- [Reports](#reports)
- [Language](#language)
- [Customising for your project](#customising-for-your-project)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Disclaimer](#disclaimer)

## Requirements

- [Claude Code](https://code.claude.com) with plugin support (CLI, desktop or IDE extension).
- Read access to this GitHub repository. It is private, so Claude Code must be able to authenticate to GitHub, e.g. via the GitHub CLI (`gh auth login`) or a configured git credential helper.
- Optional, for deeper automated checks: Node.js (`npx` for axe, Lighthouse, license-checker), and the audit tools of your stack (`npm audit`, `composer audit`, `pip-audit` …). Skills fall back to manual review when a tool is missing.

## Installation

Inside Claude Code:

```text
/plugin marketplace add degoya/degoya-ai-toolset
/plugin install degoya-ai-toolset@degoya-ai-toolset
/reload-plugins
```

1. `marketplace add` registers this repository as a plugin source.
2. `plugin install` installs the plugin itself. **Adding the marketplace alone is not enough.**
3. `reload-plugins` activates it in the running session (or restart Claude Code).

Alternatively open `/plugin`, choose **Discover**, select `degoya-ai-toolset` and install it there.

Check that it worked: type `/degoya-ai-toolset:` and the commands and skills should appear in the autocomplete list.

### Install for a whole team (project scope)

To make the plugin available to everyone working on a project, add it to the project's `.claude/settings.json` and commit that file:

```json
{
  "extraKnownMarketplaces": {
    "degoya-ai-toolset": {
      "source": { "source": "github", "repo": "degoya/degoya-ai-toolset" }
    }
  },
  "enabledPlugins": {
    "degoya-ai-toolset@degoya-ai-toolset": true
  }
}
```

Team members are asked to trust and install it when they open the project. Each person still needs GitHub access to this private repository.

### Local development

```bash
git clone https://github.com/degoya/degoya-ai-toolset.git
claude --plugin-dir ./degoya-ai-toolset
```

## Updating and removing

```text
/plugin marketplace update degoya-ai-toolset   # fetch the latest version
/reload-plugins

/plugin uninstall degoya-ai-toolset@degoya-ai-toolset
/plugin marketplace remove degoya-ai-toolset
```

## Quick start

Open Claude Code in the project you want to check and run:

```text
/degoya-ai-toolset:audit
```

A menu opens. Choose **Full audit** and Claude detects the project type, runs every applicable check and writes a report to `audit/`.

## The `/audit` command

### Interactive menu

`/degoya-ai-toolset:audit` without arguments opens a selection menu (arrow keys, space to select, enter to confirm):

1. **What would you like to audit?**
   - **Full audit** – all applicable areas
   - **By group** – then choose one or more groups
   - **Individual checks** – then choose single checks per group, plus depth (full or quick)
   - **Quick scan** – all areas, automated checks and the top 5 findings per area

### Groups

| Group | Checks | Typical audience |
|---|---|---|
| `content` | text review, SEO/GEO, i18n (content side), accessibility (content side: alt texts, headings, link texts, plain language) | marketing, editors, UX writers |
| `code` | security, performance, tests, API design, i18n (code side), accessibility (code side: semantics, keyboard, ARIA) | developers |
| `compliance` | EU legal compliance, DPIA, licences, accessibility (EAA/BFSG conformance and statement) | management, legal, DPO |
| `plan` | plan, spec and roadmap review incl. acceptance criteria | product owner, team |

### Arguments (skip the menu)

```text
/degoya-ai-toolset:audit all                      # full audit, no menu
/degoya-ai-toolset:audit content                  # one group
/degoya-ai-toolset:audit code compliance          # several groups
/degoya-ai-toolset:audit security,a11y,licenses   # single checks
/degoya-ai-toolset:audit code licenses            # group plus single check
/degoya-ai-toolset:audit all src/checkout         # limit to a path
/degoya-ai-toolset:audit all --quick              # quick scan
```

Check names: `plan`, `legal`, `dpia`, `text`, `a11y`, `i18n`, `seo`, `security`, `perf`, `tests`, `api`, `licenses`.

### What happens during an audit

1. Claude inspects the repository and detects project type, stack, locales, personal data and AI features.
2. It selects the applicable checks and names any it skips (with the reason).
3. Each check loads its skill and follows the skill's checklist. Where possible, checks run in parallel as subagents.
4. Safe, read-only tools are run where available (dependency audit, licence scan, accessibility scan against a local dev server). Nothing destructive is run and nothing is sent to external services without asking.
5. Findings are deduplicated and consolidated into one report. Nothing is changed in your code; at the end Claude offers to fix the top actions.

## Using single skills

Every skill works on its own, independent of `/audit`. Two ways:

**Call it directly:**

```text
/degoya-ai-toolset:plan-review docs/roadmap-q1.md
/degoya-ai-toolset:security-check src/api
/degoya-ai-toolset:text-review "paste your copy here"
/degoya-ai-toolset:release-notes v1.3.0..HEAD
```

**Or just ask in plain language.** Claude picks the right skill automatically:

- "Review this plan before we start" → `plan-review`
- "Are these acceptance criteria SMART?" → `plan-review`
- "Is our cookie banner GDPR compliant?" → `eu-legal-compliance`
- "Do we need a DPIA for this feature?" → `dsfa`
- "Proofread the French landing page" → `text-review`
- "Check the checkout for accessibility" → `accessibility`
- "Find hard-coded strings" → `i18n`
- "How do we show up in ChatGPT answers?" → `seo-geo`
- "Write the PR description" → `commit-pr-text`

## Skill reference

| Skill | `/audit` check (group) | Purpose | Example input |
|---|---|---|---|
| `plan-review` | `plan` (plan) | Pre/post-implementation review of plans, roadmap items and features; pre-mortem; SMART acceptance criteria; Definition of Done check; Go / Go with conditions / No-go | plan document, ticket, PR |
| `eu-legal-compliance` | `legal` (compliance) | GDPR/ePrivacy, third-party rights, AI labelling (EU AI Act Art. 50), imprint, consumer law, DSA, wording rules | website, app, feature, legal texts |
| `dsfa` | `dpia` (compliance) | DPIA threshold analysis and draft (GDPR Art. 35) | feature or processing description |
| `license-check` | `licenses` (compliance) | Open-source and asset licences, copyleft, attribution, SBOM | repository |
| `accessibility` | `a11y` (content, code, compliance) | WCAG 2.2 AA, EN 301 549, EAA/BFSG, with code fixes | pages, components, app screens |
| `text-review` | `text` (content) | Native-level review of copy in any language | UI strings, web copy, store listing |
| `seo-geo` | `seo` (content) | Technical SEO, content, structured data, GEO (AI answer visibility) | website, page |
| `i18n` | `i18n` (content, code) | Strings, plurals, formats, RTL, hreflang, translation workflow | codebase, translation files |
| `security-check` | `security` (code) | OWASP Top 10, ASVS, MASVS, OWASP Top 10 for LLM apps | code, config, plan |
| `performance` | `perf` (code) | Core Web Vitals, bundle, caching, DB/API latency, app start-up | site, API, app |
| `test-strategy` | `tests` (code) | Test strategy and plan from acceptance criteria and risks; review of existing tests | feature, test suite |
| `api-design-review` | `api` (code) | REST/GraphQL/webhooks/OpenAPI review, breaking-change detection | OpenAPI file, routes |
| `release-notes` | – | Changelog, release notes, app-store "What's new", SemVer suggestion | git range, PR list |
| `commit-pr-text` | – | Conventional Commit messages and PR descriptions | current diff |

`release-notes` and `commit-pr-text` generate texts and are therefore not part of `/audit`.

## Reports

- `/audit` writes `audit/AUDIT-REPORT-<group>-<YYYY-MM-DD>.md` and shows the executive summary in the chat.
- Single skills answer in the chat; ask "save this as a file" if you want it written to disk.
- Common severity scale: **Critical** (legal violation, exploitable vulnerability, data loss, blocks a user group), **High**, **Medium**, **Low**.
- Every finding names a location (file:line, URL, screen) and a fix.
- Tip: add `audit/` to `.gitignore` if reports should not be committed, or commit them to track progress over time.

## Language

The plugin itself is written in English. Reports and answers are written in the language you write in (German prompt → German report). Text reviews keep the text in its original language.

## Customising for your project

The skills read project context automatically. To steer them, add any of these to your project:

| File | Used by |
|---|---|
| `docs/dod.md` or a "Definition of Done" section in `CONTRIBUTING.md` | `plan-review`, `test-strategy` |
| `docs/styleguide.md`, `glossary.md` | `text-review`, `i18n` |
| `CLAUDE.md` with target markets, locales, stack, audience | all skills |
| `.github/pull_request_template.md` | `commit-pr-text` |

## Troubleshooting

| Problem | Fix |
|---|---|
| `Unknown command: /degoya-ai-toolset:audit` | The marketplace was added but the plugin is not installed. Run `/plugin install degoya-ai-toolset@degoya-ai-toolset`, then `/reload-plugins`. |
| Marketplace cannot be added / repository not found | The repository is private. Run `gh auth login` (or set up git credentials) with an account that has access, then try again. |
| Old behaviour after an update | `/plugin marketplace update degoya-ai-toolset`, then `/reload-plugins` or restart Claude Code. |
| Skill does not trigger automatically | Call it explicitly with `/degoya-ai-toolset:<skill>`. |
| No menu appears for `/audit` | Your Claude Code version may not support interactive questions; the command then shows a numbered list. Or pass arguments directly, e.g. `/degoya-ai-toolset:audit all`. |
| Automated checks are skipped | The tool (e.g. `npx`, `composer`) is not installed or no dev server is running. The report lists what was not checked. |

## Contributing

Structure:

```text
.claude-plugin/
  plugin.json          # plugin manifest (name, version, description)
  marketplace.json     # marketplace entry pointing to this repo
commands/
  audit.md             # the /audit command
skills/
  <skill-name>/
    SKILL.md           # frontmatter (name, description) + instructions
    references/        # checklists loaded on demand
    templates/         # output templates
```

Adding a skill:

1. Create `skills/<name>/SKILL.md` with `name` (same as the folder) and a one-sentence `description` that includes trigger phrases.
2. Keep `SKILL.md` short; move long checklists into `references/`.
3. Write in English; reports in the user's language.
4. If it is an audit, add a check name and group to the tables in `commands/audit.md` and this README.
5. Bump `version` in `.claude-plugin/plugin.json` and `marketplace.json`.
6. Validate: `claude plugin validate .`
7. Commit with a Conventional Commit message, no co-author trailers.

## Disclaimer

`eu-legal-compliance`, `dsfa`, `license-check` and `accessibility` produce a structured pre-assessment, not legal advice. Laws, guidelines and deadlines change; the skills say when to verify the current state and when to involve a qualified professional (lawyer, data protection officer, accessibility auditor).
