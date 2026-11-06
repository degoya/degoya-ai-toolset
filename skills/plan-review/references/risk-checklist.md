# Risk checklist (pre-mortem)

Guiding question: **"It is three months later and the feature has failed. Why?"** Find at least one plausible answer per category. Where the plan is silent, that is a finding.

## Business
- Can the goal be stated in one sentence and measured (metric, threshold)?
- Is the user group named? Are groups forgotten (admins, guests, mobile users, screen-reader users)?
- Does the feature contradict existing features, processes or promises (terms, marketing)?
- What happens to existing data and users during the switch?

## Technical
- Dependencies: other teams, external APIs, libraries, licences. What happens if one fails or changes?
- Data migration: direction, duration, reversibility, behaviour if aborted halfway.
- Backward compatibility: old clients/app versions, API versions, stored links, caches.
- Scale and performance: expected load, data volume in 1–2 years, N+1 queries, limits of external services.

## Failure cases
- Empty, too long, invalid, duplicate, malicious input.
- Timeouts, partial outages, retries (idempotency).
- Concurrency: two users, two tabs, double clicks, race conditions.
- Offline/poor connectivity (apps!).
- Rollback: how do we roll back, and what happens to data created in the meantime?

## Security and privacy
- Authentication and authorisation: who may do what? Horizontal privilege escalation (foreign IDs)?
- Input validation and output encoding.
- Personal data: legal basis, purpose, retention, deletion, processors (see skill `eu-legal-compliance`).
- Secrets; logs free of personal data and tokens.

## Operations
- Monitoring and alerting: how does the team notice breakage before users report it?
- Logging, tracing, error reporting.
- Feature flag, gradual rollout, kill switch.
- Deployment order (DB before code? backend before app release?), app-store review times.

## Planning
- Is the estimate realistic? Are there reference values?
- Are roadmap dependencies and order consistent?
- Is the scope too large for one sprint? Then cut vertical slices.
- Who decides in case of conflicts? Is an owner named?

## Rating per risk
| Risk | Likelihood (low/medium/high) | Impact (low/medium/high) | Countermeasure | Owner |
