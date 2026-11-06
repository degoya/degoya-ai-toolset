---
name: commit-pr-text
description: Writes and reviews commit messages (Conventional Commits) and pull request descriptions from the current diff: summary, motivation, changes, test evidence, risks, screenshots note, breaking changes, linked tickets. Use for "write a commit message", "PR description", "summarise my changes", "review my commit message".
---

# Commit and PR texts

## Commit messages
Conventional Commits format:
```
<type>(<scope>): <short imperative description, ≤ 72 chars, no period>

<body: why, not how; lines ≤ 72 chars>

BREAKING CHANGE: <what and how to migrate>
Refs: #123
```
Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

Rules:
- One commit = one logical change. Suggest splitting a mixed diff.
- Follow the language and conventions of the existing history (`git log --oneline -20`); project rules (CONTRIBUTING.md, commitlint config) take precedence.
- The subject describes the effect ("fix(cart): calculate total correctly with voucher"), not "Update file.ts" or "WIP".
- Add no trailers (e.g. `Co-Authored-By`) unless the project or user asks for them.

## PR description
Workflow:
1. Read the diff against the target branch (`git diff <base>...HEAD`), include commits and linked tickets.
2. Use the repo's PR template if present (`.github/pull_request_template.md` etc.), otherwise this one:

```markdown
## What and why
<1–3 sentences: problem and solution from the user's/reviewer's view>

## Changes
- …

## How was this tested?
- [ ] Unit/integration tests: …
- [ ] Manual: <steps>
- [ ] Accessibility / translations / performance, if affected

## Risks and rollback
<what can go wrong, feature flag, migration, rollback path>

## Screenshots / video
<before/after for UI changes>

## Breaking changes
<none | description + migration>

Closes #…
```
3. Check: is the PR too big (rule of thumb > 400 changed lines excluding generated code)? Suggest splitting. Does the diff contain debug code, commented-out code, secrets or unintended files? Point them out.

## Rules
- Describe only what is in the diff; do not embellish. State untested parts honestly as "not tested".
- For release notes from many PRs use skill `release-notes`.
