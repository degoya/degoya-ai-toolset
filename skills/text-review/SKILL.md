---
name: text-review
description: 'Reviews and improves copy to native-speaker level in any language: spelling, grammar, punctuation, idiom, style, tone, consistency, terminology and readability, for UI strings, websites, app-store listings, docs, emails and marketing. Use for "proofread this", "does this sound natural?", "native speaker check", "review this copy", "improve microcopy", "Korrektur lesen".'
---

# Text review (native-speaker level)

Goal: the text reads as if a native-speaking professional wrote it for exactly this audience. Correctness is mandatory; sounding natural is the goal.

## Workflow

1. **Set the frame** (infer from context, ask briefly otherwise):
   - Language and variant: de-DE / de-AT / de-CH, en-US / en-GB, fr-FR / fr-CA, es-ES / es-MX, pt-PT / pt-BR …
   - Text type: UI/microcopy, website, app store, legal, docs, marketing, email.
   - Audience and register: formal/informal (du/Sie, tu/vous, tú/usted).
   - Existing style guide or glossary in the repo (`docs/styleguide*`, `glossary*`, translation files).
2. **Review on four levels** (in this order):
   1. **Correctness:** spelling, grammar, punctuation, language-specific typography.
   2. **Idiom:** does it sound natural? No calques (DE "Sinn machen" → "sinnvoll sein"), false friends (eventually ≠ eventuell, actual ≠ aktuell), unnatural word order.
   3. **Style and tone:** fits audience and brand, active rather than passive, short sentences, no filler, no AI clichés ("in today's fast-paced world", "let's dive in", "seamless", "delve", "leverage", "unlock").
   4. **Consistency:** terms, form of address, spellings, number/date formats, capitalisation of buttons and headings.
3. **Output** in the format below.

## Language-specific typography (selection)
| Language | Quotes | Notes |
|---|---|---|
| de-DE/AT | „…“ or »…« | decimal comma, thousands dot; non-breaking space in "z. B.", "10 %", "5 €"; en dash – with spaces |
| de-CH | «…» | no ß (ss), thousands apostrophe 1'000 |
| en-US | “…” | serial comma common, date MM/DD/YYYY, title case in headings depending on style guide |
| en-GB | ‘…’ | -ise/-our, date DD/MM/YYYY, mostly sentence case |
| fr-FR | « … » | non-breaking space before : ; ! ? and inside « » |
| es | «…» or “…” | ¿…? and ¡…! |

## UI and microcopy rules
- Buttons name the action ("Save", "Delete account"), not "OK"/"Yes".
- Error messages: what happened, why, what the user can do. No blame.
- Mind length (German is often ~30 % longer than English), see skill `i18n`.
- Plain language where accessibility matters, see skill `accessibility`.

## Output format
```
## Text review: <subject> (<language/variant>, <register>)

### Summary
<overall impression, number of corrections per level>

### Corrections
| # | Original | Suggestion | Level | Reason |

### Revised text
<full corrected version>

### Glossary/consistency
<agreed terms and spellings>
```

## Rules
- Do not change content or meaning. If in doubt about content, ask instead of rewriting.
- Explain corrections so the author learns. Mark style issues as suggestions, not errors.
- Leave brand names, code, variables (`{name}`, `%s`, `{{count}}`) and HTML untouched.
- Write explanations in the user's language; corrected text stays in the text's language.
- If you are not confident in a language, say so and recommend a native-speaker review.
