---
name: plan-review
description: Reviews code plans, roadmap items, user stories and features before or after implementation, finds risks and gaps, and sharpens acceptance criteria using SMART. Use for "review this plan", "pre-implementation review", "check acceptance criteria", "is this feature done?", "roadmap check", "pre-mortem".
---

# Plan review

Evaluates plans and features before they cost money, and checks after implementation whether they deliver what was promised. The skill assesses and suggests. It does not implement anything and does not replace line-level code review.

## Modes

| Mode | When | Focus |
|---|---|---|
| **Pre-review** | before implementation | risks, gaps, failure cases, sharpening acceptance criteria |
| **Post-review** | after implementation (diff, PR, demo) | compare the result with criteria and DoD, with evidence |
| **Criteria only** | a story/ticket exists | only checklist C (SMART) and D (DoD separation) |

If the mode is not stated: code, a diff or a PR means post-review, otherwise pre-review.

## Workflow

1. **Clarify context.** Goal, user group, constraints (time, budget, stack) and existing DoD. Look in the material and the repo first (README, CLAUDE.md, `docs/`, DoD file). If something essential is missing, ask at most 3 targeted questions. Otherwise continue with clearly marked assumptions.
2. **Pre-mortem** with [references/risk-checklist.md](references/risk-checklist.md): "It is three months later and the feature has failed. Why?"
3. **Best practices per area** with [references/best-practices.md](references/best-practices.md). Only check areas the plan touches.
4. **Acceptance criteria**: check and improve each one with [references/acceptance-criteria.md](references/acceptance-criteria.md). If criteria are missing, propose them.
5. **DoD check** with [references/dod-template.md](references/dod-template.md) or the team's own DoD. Move misplaced items to the right list.
6. **Post-review only:** rate each criterion as *met / not met / not verifiable*, with evidence (test, measurement, code location, screenshot). Run tests yourself where possible.
7. **Report** using [templates/review-report.md](templates/review-report.md).

If the plan touches law, accessibility, copy, localisation or SEO, point to the skills `eu-legal-compliance`, `accessibility`, `text-review`, `i18n` and `seo-geo` and include only their most important points.

## Rating rules

**Severity**
- **Blocker:** makes the goal unreachable, violates law/security or loses data. Must be solved before starting.
- **High:** likely costly error or rework. Solve before starting or accept consciously.
- **Medium:** quality or maintainability issue. Fix in the plan.
- **Low:** improvement, no risk.

**Recommendation**
- **Go:** no blockers, no open high findings, all criteria SMART.
- **Go with conditions:** no blockers; every high finding has a named countermeasure.
- **No-go:** at least one blocker, or the goal is too unclear to write criteria.

## Limits

- Do not invent facts. Mark missing information as an open question addressed to PO, Dev or QA.
- Label assumptions explicitly.
- Keep findings concrete: quote the plan, name the impact, make a suggestion.
- Five important findings beat thirty small ones. Collect low findings at the end.
- Write the report in the user's language.
