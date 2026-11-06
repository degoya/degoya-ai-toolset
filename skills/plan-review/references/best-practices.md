# Best practices per area

Only check areas the plan touches.

| Area | Question | Typical finding |
|---|---|---|
| Architecture | Clear responsibilities, no needless coupling, decision documented (ADR)? | New component reads another service's DB tables directly |
| Code | Simplest solution (KISS/YAGNI), existing building blocks reused? | Custom framework for a one-off problem |
| Data | Schema changes migratable and reversible, indexes, data quality? | Column renamed without a transition phase |
| API | Versioned, consistent error codes, pagination, idempotent writes? | POST without idempotency key for payments |
| Tests | Test pyramid planned, every acceptance criterion has at least one test? | Only manual tests for core logic |
| Security | OWASP Top 10 considered, no secrets in code, least privilege? | Admin endpoint "protected" only by the frontend |
| Privacy | Data minimisation, legal basis, deletion concept? | New tracking library without consent |
| UX | Empty states, loading states, error messages clear and actionable? | "Error 500" without telling users what to do |
| Accessibility | WCAG 2.2 AA (keyboard, contrast, labels, focus)? | New modal without focus trap and Escape |
| Performance | Budget defined (e.g. LCP < 2.5 s, API p95 < 300 ms), caching? | Whole list loaded unfiltered |
| Localisation | Strings externalised, formats localised, room for longer text? | Hard-coded strings, fixed date format |
| Operations | Measurable SLOs, alerting, rollback plan, runbook? | No way to switch the feature off without a deployment |
| Docs | User and developer docs, changelog entry? | New environment variable documented nowhere |

## Stack-specific
If the stack is known (from repo, `package.json`, `composer.json`, Gradle, Podfile etc.), add the framework's best practices (e.g. server vs. client components, ORM migrations, mobile lifecycle). Look up current documentation when in doubt instead of judging from memory.
