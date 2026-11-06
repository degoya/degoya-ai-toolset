---
name: api-design-review
description: Reviews and designs REST, GraphQL and webhook APIs and OpenAPI specs: resources and naming, HTTP methods and status codes, error format (RFC 9457), pagination, filtering, versioning, idempotency, auth, rate limits, backward compatibility and docs. Use for "review API design", "review OpenAPI", "design an endpoint", "is this API good?", "is this a breaking change?".
---

# API design review

Goal: an API that is consistent, predictable, secure and can evolve without breaking changes.

## Workflow
1. **Context:** consumers (own frontend, apps, partners, public), existing conventions in the project (other endpoints, style guide), spec (OpenAPI/GraphQL schema) or code.
2. **Review** with the checklist; consistency with the existing API beats the textbook.
3. **Report** with concrete change suggestions (spec excerpts).

## REST checklist
- [ ] Resources as plural nouns (`/orders/{id}/items`), no verbs in paths (exception: clearly named actions such as `POST /orders/{id}/cancellation`).
- [ ] Consistent casing (paths kebab-case, fields camelCase or snake_case, never mixed).
- [ ] Correct methods: GET safe and idempotent, PUT/DELETE idempotent, PATCH for partial updates, POST to create.
- [ ] Appropriate status codes: 200/201 (+ `Location`)/204, 400 vs. 422 used consistently, 401 vs. 403, 404, 409 conflict, 429 with `Retry-After`, 5xx only for server errors.
- [ ] Uniform error format, e.g. Problem Details (RFC 9457: `type`, `title`, `status`, `detail`, `instance`, plus field errors); no stack traces.
- [ ] Pagination (cursor-based for large/changing data), filtering, sorting, field selection consistent; limits with a maximum.
- [ ] Idempotency keys for non-idempotent writes with side effects (payments, orders).
- [ ] Concurrency: ETag/`If-Match` or version field against lost updates.
- [ ] Date/time in ISO 8601 with time zone (UTC); money as integer minor units or decimal string plus currency (ISO 4217); IDs as strings.
- [ ] Versioning defined (URL `/v1` or header), deprecation strategy (`Deprecation`/`Sunset` headers, timeline).
- [ ] Auth (OAuth 2.1/OIDC; API keys server-side only), scopes, object-level authorisation (see skill `security-check`), documented rate limits.
- [ ] No over-fetching leaks (internal fields, personal data the consumer does not need).

## GraphQL additionally
- [ ] Deliberate nullability, connections pattern for lists, query depth/cost limits, errors in `errors` with codes, mutations with input types and payloads.

## Webhooks additionally
- [ ] Signature (HMAC) with timestamp, retries with backoff, idempotency on the receiver (event ID), event versioning.

## Detecting breaking changes
Breaking: removing or renaming a field/endpoint, changing a type, making an optional field required, removing enum values, changing semantics/defaults, stricter validation. Not breaking: new optional field, new endpoint. Tools: `oasdiff`, `openapi-diff`, GraphQL Inspector.

## Report format
```
## API review: <subject>
### Summary
### Findings
| # | Endpoint/field | Finding | Severity | Breaking? | Suggestion |
### Spec suggestions
<OpenAPI/schema excerpts>
```
Write the report in the user's language.
