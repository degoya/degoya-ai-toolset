---
name: i18n
description: Plans and reviews internationalisation and localisation of websites and apps: externalised strings, plural rules, date/number/currency formats, RTL, text expansion, language switcher, URL structure, hreflang, translation workflow and QA. Use for "make this multilingual", "i18n", "l10n", "add a language", "localisation check", "find hard-coded strings".
---

# Internationalisation (i18n) and localisation (l10n)

**i18n** = build the code so it can handle any language/region. **l10n** = adapt content to a specific language/region. Check both.

## Workflow
1. **Current state:** detect framework and i18n library (e.g. i18next/react-intl, vue-i18n, @nuxtjs/i18n, next-intl, Angular i18n, Laravel/Symfony translation, gettext, Android `strings.xml`, iOS String Catalogs). Clarify target languages and regions.
2. **Check the code** with the checklist below. Find hard-coded strings by searching (text between tags, string literals in UI components, alerts, emails, error messages, PDFs).
3. **Check content:** translations complete, placeholders intact, length fits, culturally appropriate. Language quality via skill `text-review`.
4. **Report** with findings, fixes and a migration plan if needed.

## Checklist i18n (code)
- [ ] All user-visible text in resource files with meaningful keys (`checkout.button.pay`), including emails, push, PDFs, backend error messages, alt text, `<title>`, meta.
- [ ] No sentences assembled from fragments ("You have " + n + " messages"); use ICU MessageFormat with placeholders.
- [ ] **Plurals** via CLDR rules (`one`, `few`, `many`, `other`; Polish, Russian, Arabic have up to 6 forms).
- [ ] **Gender/context** via `select`, not code branches.
- [ ] Dates, times, numbers, currencies, lists via `Intl.*` or platform APIs, never formatted by hand. Time zones: store UTC, display local.
- [ ] Locale-aware sorting and search (`Intl.Collator`, umlauts, accents).
- [ ] UTF-8 everywhere (MySQL: `utf8mb4`); emoji and non-Latin scripts tested.
- [ ] **RTL** (Arabic, Hebrew): `dir="rtl"`, CSS logical properties (`margin-inline-start` instead of `margin-left`), mirrored icons where direction carries meaning.
- [ ] **Text length:** layout tolerates +30–40 % (German, Finnish) and very short text (Chinese). No text in images.
- [ ] Fonts cover all target scripts (fallback fonts).
- [ ] Input: names, addresses, phone numbers, postcodes not validated against one country's format.
- [ ] Fallback chain defined (de-AT → de → en); missing keys detected in build/CI.
- [ ] `lang` attribute per page and on language changes in content.

## Checklist web structure and SEO
- [ ] Own URL per language (subdirectory `/de/`, `/en/` recommended), not cookie/JS-only switching.
- [ ] Reciprocal `hreflang` plus `x-default`; self-referencing canonical per language version.
- [ ] No forced redirect by IP/browser language; suggestion banner instead.
- [ ] Visible language switcher, languages named in their own language ("Deutsch", "Français"), no flags for languages.
- [ ] Translated slugs, meta titles, descriptions, structured data. Details: skill `seo-geo`.

## Checklist l10n (content and law)
- [ ] Currencies, prices, taxes, shipping per market; units of measurement.
- [ ] Legal texts (imprint, privacy, terms) per market and language, see skill `eu-legal-compliance`.
- [ ] Images, colours, examples, holidays, form of address culturally appropriate.
- [ ] App stores: localised descriptions, screenshots, keywords.

## Translation workflow
- Define the source language, maintain a glossary and style guide, give translators context (screenshots, description, max. length) in resource files.
- Use a TMS (e.g. Crowdin, Lokalise, Phrase, Weblate) synced via CI instead of manual file hand-offs.
- Machine translation only with native-speaker post-editing, especially for legal and marketing text.
- Pseudo-localisation (e.g. `[Ŝéţţîñĝš ~~~]`) in tests to find hard-coded strings and truncation.

## Report format
```
## i18n review: <subject>
Stack: … · Languages: … · Status: <not prepared / partial / complete>

### Findings
| # | Location (file:line) | Issue | Level | Fix |

### Migration plan (if needed)
1. … (steps with a verifiable result)
```
Write the report in the user's language.
