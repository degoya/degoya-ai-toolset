# Definition of Done (template)

Applies to **all** of the team's work, not to a single story. If the project has its own DoD (e.g. `docs/dod.md`, `CONTRIBUTING.md`), that one wins. This template is the default when none exists.

## Separation

| | Acceptance criteria | Definition of Done |
|---|---|---|
| Applies to | one story / backlog item | all of the team's work |
| Content | functional requirements from the user's view | quality standards |
| Defined by | PO with the team, per story | the team, once, rarely changed |
| Example | "Max. 20 search results per page" | "Code reviewed, unit tests green" |

## Default DoD
- [ ] All acceptance criteria met and evidenced
- [ ] Code reviewed by at least one other person
- [ ] Automated tests for new logic exist and pass
- [ ] Linter, type check and build without errors
- [ ] No new security warnings (dependencies, secret scan)
- [ ] Accessibility of new UI checked (keyboard, contrast, labels)
- [ ] Strings externalised and translated, or translation requested
- [ ] Docs and changelog updated
- [ ] Monitoring/logging for new paths in place
- [ ] Accepted on staging/test environment
- [ ] Feature flag or rollback path documented
