---
name: seo-geo
description: 'Reviews and optimises websites for classic search engines (SEO) and for AI answer engines (GEO/AEO: ChatGPT, Perplexity, Google AI Overviews/AI Mode, Claude, Gemini): technical SEO, content, structured data, internal linking, international SEO, citability in AI answers. Use for "SEO check", "GEO", "show up in AI answers", "meta tags", "structured data", "improve rankings", "llms.txt".'
---

# SEO and GEO

**SEO:** be found in search engines. **GEO** (generative engine optimisation): be understood, cited and recommended by AI systems. Both rest on the same foundation: technically accessible, clear content, trustworthy.

## Workflow
1. **Clarify goals:** which pages, audience, markets/languages, key search queries and the questions users ask AI assistants.
2. **Technical check** (code or URL): checklist A.
3. **Content check:** checklist B.
4. **GEO check:** checklist C.
5. **Report** with prioritised actions (impact × effort).

## Workspace hygiene (mandatory when running tools)

Never leave tool artefacts in the user's project. Only `audit/` reports (and files the user asked for) may be written to the repo.

- Run browser-based and scanning tools (Lighthouse, Chrome/Puppeteer/Playwright, axe CLI, Pa11y, SBOM/licence scanners) from a fresh temp directory **outside** the repo, and pass all output paths as absolute paths:
  ```bash
  AUDIT_TMP="$(mktemp -d "${TMPDIR:-/tmp}/degoya-audit.XXXXXX")"
  (cd "$AUDIT_TMP" && npx lighthouse "$URL" --quiet \
     --chrome-flags="--headless=new --user-data-dir=$AUDIT_TMP/chrome-profile" \
     --output=json --output-path="$AUDIT_TMP/lighthouse.json")
  # read results from $AUDIT_TMP, then:
  rm -rf "$AUDIT_TMP"
  ```
- **WSL:** Lighthouse/chrome-launcher may start Windows Chrome and build its profile path from Windows env vars that are empty in WSL, creating folders like `undefined:\Users\undefined\AppData\Local\lighthouse.*` in the current directory. Running from `$AUDIT_TMP` keeps them out of the repo; alternatively point `CHROME_PATH` to a Linux Chrome/Chromium.
- Before finishing, run `git status --porcelain` and compare with the state before the run. Remove anything the run created outside `audit/` (only files you created – never touch the user's own changes) and report what was cleaned up.
- Do not install dependencies into the project (`npm install` in the repo, editing `package.json`/lockfiles) to run a check; use `npx`/temporary installs instead.

## A. Technical SEO
- [ ] Indexable: no accidental `noindex`, `robots.txt` does not block important paths/resources, XML sitemap exists and is referenced in robots.txt.
- [ ] Content in the initial HTML (SSR/SSG), not client-rendered only; also important for AI crawlers, which often do not execute JavaScript.
- [ ] Unique `<title>` (approx. 50–60 characters) and meta description (approx. 140–160 characters) per page.
- [ ] Correct canonicals, no duplicates (http/https, www, trailing slash, parameters).
- [ ] Clean, descriptive URLs; 301 on moves, no redirect chains, no 404s on internal links.
- [ ] Core Web Vitals green: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 (see skill `performance`).
- [ ] Mobile-friendly, HTTPS, semantic HTML (one h1, logical headings).
- [ ] Images: descriptive file names, alt text, modern formats, `width`/`height`.
- [ ] Multilingual: `hreflang` + `x-default`, own URLs per language (see skill `i18n`).
- [ ] Structured data (JSON-LD, schema.org) as appropriate: Organization, WebSite, BreadcrumbList, Article, Product/Offer, FAQPage, SoftwareApplication, LocalBusiness. Validate with the Rich Results Test.
- [ ] Open Graph / social tags.

## B. Content
- [ ] Each page answers one clear search intent (informational, commercial, transactional, navigational).
- [ ] The key answer comes early and clearly; headings as questions or statements that carry the structure.
- [ ] E-E-A-T: author with expertise, sources, publish/update date, about/imprint, real experience and examples.
- [ ] No cannibalisation (several pages for the same query), no thin content.
- [ ] Internal linking: topic clusters with pillar page, descriptive anchor text, no orphan pages.

## C. GEO (visibility in AI answers)
- [ ] **Citable statements:** short, factual, self-contained paragraphs; concrete numbers, definitions, comparisons, step-by-step guides, tables.
- [ ] **Evidence:** sources, statistics with origin and date, expert quotes increase the chance of being cited.
- [ ] **Clear entities:** consistent company, product and person names; Organization schema with `sameAs` (LinkedIn, Wikipedia/Wikidata, Crunchbase, GitHub).
- [ ] **FAQ and comparison content** for the questions users ask AI assistants ("best … for …", "X vs. Y").
- [ ] **Presence beyond your own site:** mentions in trade articles, review sites, forums, directories, Wikipedia where relevant; AI systems weigh consensus across sources.
- [ ] **Control crawler access deliberately:** decide in `robots.txt` which AI crawlers are allowed (e.g. `OAI-SearchBot`, `GPTBot`, `PerplexityBot`, `ClaudeBot`, `Google-Extended`). Allowing search crawlers while blocking training crawlers is possible.
- [ ] **llms.txt** (optional proposal, not a binding standard): curated Markdown overview of key pages at `/llms.txt`.
- [ ] **Freshness:** visible update date, maintain outdated content.
- [ ] Measure: segment referral traffic from AI platforms in analytics; regularly ask key questions in ChatGPT, Perplexity, Gemini, Google AI Mode and record mentions.

## Report format
```
## SEO/GEO review: <subject>
### Summary
### Actions (prioritised)
| # | Area (technical/content/GEO) | Finding | Action | Impact (H/M/L) | Effort (H/M/L) |
### Code/text suggestions
<title/meta, JSON-LD, robots.txt, paragraph rewrites>
```

## Rules
- No ranking guarantees, no black-hat tactics (keyword stuffing, hidden text, bought links, cloaking for AI crawlers).
- Search engine and AI platform rules change often: verify concrete thresholds and crawler names against current docs.
- Write for humans; optimisation must not hurt readability (skill `text-review`).
- Write the report in the user's language.
