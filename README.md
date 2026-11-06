# degoya-ai-toolset

A Claude Code plugin with review and quality skills for software, web and app projects.
Run every applicable check at once with **`/audit`**, or use each skill on its own.

## Usage

```text
/degoya-ai-toolset:audit                       # full audit of the current project
/degoya-ai-toolset:audit security,a11y,legal   # only selected areas
/degoya-ai-toolset:audit src/checkout          # limit to a path
/degoya-ai-toolset:plan-review docs/plan.md    # any single skill, directly
```

Skills also trigger automatically when you ask for them in plain language, e.g. "check this plan", "is this GDPR compliant?", "run an accessibility check".

## Skills

| Skill | `/audit` area | Purpose |
|---|---|---|
| `plan-review` | `plan` | Review plans, roadmap items and features before/after implementation, sharpen SMART acceptance criteria, pre-mortem, DoD check |
| `eu-legal-compliance` | `legal` | GDPR/ePrivacy, third-party rights (copyright, trademarks, image rights), AI labelling (EU AI Act), mandatory legal texts |
| `dsfa` | `dpia` | DPIA threshold analysis and draft (GDPR Art. 35) |
| `text-review` | `text` | Native-level review of copy in any language: grammar, idiom, tone, consistency |
| `accessibility` | `a11y` | WCAG 2.2 AA, EN 301 549, European Accessibility Act / German BFSG |
| `i18n` | `i18n` | Internationalisation and localisation: strings, plurals, formats, RTL, hreflang, workflow |
| `seo-geo` | `seo` | Classic SEO and GEO (visibility in AI answers) |
| `security-check` | `security` | OWASP Top 10, ASVS, MASVS, OWASP Top 10 for LLM apps |
| `performance` | `perf` | Core Web Vitals, bundle, caching, DB/API latency, app start-up, performance budget |
| `test-strategy` | `tests` | Test strategy and test plan derived from acceptance criteria and risks |
| `api-design-review` | `api` | REST/GraphQL/webhooks/OpenAPI review, breaking-change detection |
| `license-check` | `licenses` | Open-source and asset licences, copyleft, attribution, SBOM |
| `release-notes` | – | Changelog, release notes, app-store "What's new", SemVer suggestion |
| `commit-pr-text` | – | Conventional Commit messages and PR descriptions |

## Installation (Claude Code)

```bash
/plugin marketplace add degoya/degoya-ai-toolset
/plugin install degoya-ai-toolset@degoya-ai-toolset
```

The repository is private, so Claude Code needs GitHub access (e.g. `gh auth login`).

Local testing: `claude --plugin-dir ./degoya-ai-toolset`

## Language

The plugin is written in English. Reports are written in the language the user writes in, unless asked otherwise.

## Disclaimer

`eu-legal-compliance`, `dsfa`, `license-check` and `accessibility` produce a structured pre-assessment, not legal advice. Involve a qualified professional (lawyer, data protection officer, accessibility auditor) where the report says so.
