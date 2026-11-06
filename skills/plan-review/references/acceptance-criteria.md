# Acceptance criteria

Acceptance criteria are the conditions a story must meet for the PO and users to accept it.

**Why they matter:**
- **Clarity:** remove misunderstandings and room for interpretation from requirements.
- **Measurability:** define exactly when a feature works ("loads in under 2 seconds" instead of "loads fast").
- **Test basis:** serve testers and the PO directly as the acceptance checklist.
- **Separation:** apply to one specific story; the Definition of Done applies to all of the team's work.

## Check per criterion

| Point | Question | ✗ if … |
|---|---|---|
| **S**pecific | Does it describe concrete, observable behaviour? | ambiguous words, no actor, no outcome |
| **M**easurable | Is there a clear pass/fail threshold? | "fast", "easy", "intuitive", "many" |
| **A**chievable | Can it be done in the sprint with available means? | depends on unresolved third parties, unrealistic threshold |
| **R**elevant | Does it serve the story's goal, from the user's view? | technical detail without user value |
| **T**ime-bound | Does it have a time reference where it matters? | deadlines, freshness or response times missing although relevant |

Additionally:
- **Testable:** maps directly to a test.
- **Outcome, not recipe:** describes *what*, not *how* (no table, framework or class names).
- **Independent:** verifiable without other criteria.
- **Right level of detail:** neither vague nor pixel-level; negative and error cases covered.

"T" does not apply to every criterion. Only mark ✗ when time matters for the behaviour, otherwise "n/a".

## Anti-patterns
- Filler words: fast, easy, user-friendly, modern, looks good, robust, flexible.
- Implementation instructions instead of outcomes ("uses a Redis cache").
- Criteria that are really DoD items ("code is reviewed", "unit tests pass").
- Compound criteria joining several behaviours with "and".
- Happy path only.

## Formats
**Rule-based (default):** checklist of short, verifiable statements.
**Given/When/Then (for flows):** from BDD, see [../templates/criterion-gwt.md](../templates/criterion-gwt.md).

## Examples

| Bad | Better |
|---|---|
| Search is fast. | With 100,000 products, search shows the first 20 results in under 1 s (p95). |
| Users can register easily. | Registration only requires email and password; after submitting, the user receives a confirmation email within 2 minutes. |
| Errors are displayed well. | For an invalid email, the text "Please enter a valid email address" appears next to the field and focus moves to the field. |
| Ratings are up to date. | Displayed credit ratings are no older than 30 days; older ones are marked "outdated". |
| Data is stored in PostgreSQL with an index. | (Implementation detail, not a criterion. Move to the technical design.) |
| Code is tested. | (DoD item. Move to the DoD.) |

## Who and when
The PO initiates, the team checks feasibility and testability, the Scrum Master facilitates. Criteria are written during refinement and fixed before development starts.

Sources: Agile Academy (Akzeptanzkriterien), Atlassian (What are acceptance criteria?).
