---
name: release-notes
description: Writes release notes, changelogs (Keep a Changelog), app-store "What's new" texts and customer updates from commits, PRs, tickets or diffs, tailored per audience (users, developers, support), with a SemVer suggestion and breaking-change notes. Use for "write release notes", "changelog", "what's new", "app store update text", "which version number".
---

# Release notes and changelog

## Workflow
1. **Collect sources:** `git log <last-tag>..HEAD`, merged PRs, tickets, existing `CHANGELOG.md`. Use Conventional Commits (`feat:`, `fix:`, `BREAKING CHANGE:`) if present.
2. **Classify** per Keep a Changelog: Added, Changed, Deprecated, Removed, Fixed, Security. Internal changes (refactoring, CI, tests) only in the developer changelog.
3. **Suggest a version** (SemVer): MAJOR for breaking changes, MINOR for new features, PATCH for fixes. Give the reason.
4. **Write per audience:**
   - **Users** (website, app store, in-app): benefits rather than technology, short sentences, most important first. Respect store limits (Apple "What's New" max. 4,000 characters, Google Play "What's new" max. 500 characters per language).
   - **Developers/API consumers:** breaking changes with migration steps, deprecations with dates, new endpoints/parameters.
   - **Support/sales:** what changes for customers, known limitations, FAQ.
5. **Check:** no internal code names, bare ticket numbers or customer names; security fixes only as detailed as is safe. Language quality via skill `text-review`, translations via skill `i18n`.

## CHANGELOG.md template
```markdown
## [1.4.0] - 2026-11-06
### Added
- Download invoices as PDF (#123)
### Changed
- Search shows 20 instead of 10 results per page
### Fixed
- Crash when opening empty projects on Android 14
### Security
- Updated dependency X to 2.3.1 (CVE-…)
```

## User release notes template
```
New in version 1.4
• Invoices as PDF: download invoices directly from your account.
• Find things faster: search now shows more results at a glance.
• Fixed: the app no longer crashes when opening empty projects.
```

## Rules
- Only include what is actually in the release; mark anything unclear as a question.
- Breaking changes always at the top and in bold.
- ISO dates in the changelog.
- Write in the language of the product's audience; ask if unclear.
