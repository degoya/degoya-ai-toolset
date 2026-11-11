---
name: test-strategy
description: 'Designs and reviews test strategies and test plans for features, websites, apps and APIs: test pyramid, which tests at which level, derivation from acceptance criteria, test data, edge cases, E2E, accessibility, performance and security tests, CI integration. Use for "how should we test this?", "test plan", "test strategy", "what tests do we need", "review test coverage".'
---

# Test strategy

Goal: maximum confidence with as few, fast and stable tests as possible. Every acceptance criterion has at least one test.

## Workflow
1. **Basis:** acceptance criteria (sharpen them first with skill `plan-review` if needed), risks, architecture, existing test tooling in the repo.
2. **Prioritise by risk:** what costs most if it breaks (money, data, law, trust)? Test there most thoroughly.
3. **Assign levels** (test pyramid):
   - **Unit** (many, milliseconds): business logic, calculations, validation, edge cases.
   - **Integration** (some): DB, queues, external APIs (with test containers or contract tests), components with real dependencies.
   - **E2E** (few): critical user journeys (sign-up, checkout, login), e.g. Playwright, Cypress, Detox, Maestro, XCUITest, Espresso.
   - **Cross-cutting:** accessibility (axe in E2E), visual regression, performance budgets (Lighthouse CI, k6), security (SAST, dependency scan), localisation (pseudo-locale).
4. **Derive test cases** per criterion: happy path, negative case, boundaries (equivalence classes, 0/1/max/max+1, empty, special characters, time zones, languages), concurrency, failures of external services.
5. **Test data and environments:** fixtures/factories, no real personal data, deterministic time/randomness.
6. **CI:** fast tests on every push, E2E before merge or nightly; fix flaky tests immediately instead of ignoring them.
7. **Output** in the format below.

## Reviewing existing tests
- [ ] Do tests check behaviour rather than implementation details?
- [ ] Are assertions meaningful (not just "does not throw")?
- [ ] Mocks only at system boundaries, not for the logic under test?
- [ ] Coverage of critical paths rather than percentage chasing; consider mutation testing for core logic.
- [ ] Runtime and stability (flakiness rate).

## Output format
```
## Test strategy: <subject>
### Risks and focus areas
### Test matrix
| Acceptance criterion / risk | Level | Test case(s) | Tool | Automated? |
### Edge cases
### Test data and environment
### CI integration
### Gaps in existing tests
```

## Rules
- Never disable or weaken tests to get green.
- Where possible, propose test code in the project's style and run it locally.
- Write the output in the user's language.
