---
name: eu-legal-compliance
description: Legal pre-assessment of websites, apps, features and copy under EU and German/Austrian law: GDPR and ePrivacy/TDDDG (cookies, consent, privacy notice, DPAs), third-party rights (copyright, trademarks, image and personality rights, licences), AI labelling under the EU AI Act Art. 50, mandatory information (imprint, terms, withdrawal, pricing) and legally sound wording. Use for "GDPR check", "is this legally OK", "third-party rights", "AI labelling", "check imprint/privacy policy", "cookie banner".
---

# EU legal compliance

Structured legal **pre-assessment** for digital products in the EU/DACH region. The result is a prioritised list of issues with location and suggestion. Not legal advice: every report ends with a note on when a qualified professional is needed.

## Workflow

1. **Set the frame:** target markets (DE, AT, CH, EU, worldwide?), B2C or B2B, type of offering (website, shop, SaaS, app, AI feature), role (provider, deployer, processor). Mark missing information as assumptions.
2. **Inventory:** pages, forms, tracking/cookies, embedded third-party services (fonts, CDNs, maps, video, analytics, chat, payment), AI features, used images/texts/code. Search the code for third-party domains, SDKs and `<script>` includes.
3. **Check** with the relevant references:
   - [references/privacy.md](references/privacy.md): GDPR, TDDDG/ePrivacy, international transfers, data subject rights
   - [references/third-party-rights.md](references/third-party-rights.md): copyright, trademarks, image/personality rights, open-source licences, unfair competition
   - [references/ai-labelling.md](references/ai-labelling.md): AI Act, transparency obligations, labelling AI content
   - [references/mandatory-information.md](references/mandatory-information.md): imprint, terms, consumer law, accessibility statement, DSA, wording rules
4. **Report** (format below).

## Rating
- **Critical:** obviously actionable by competitors (cease-and-desist) or subject to fines (e.g. tracking without consent, missing imprint, third-party image without licence).
- **High:** likely unlawful, but interpretation not clear-cut.
- **Medium:** incomplete or unclear.
- **Note:** best practice, no concrete risk.

## Report format

```
## Legal pre-assessment: <subject>
Date: <date> · Target markets: … · Assumptions: …

### Summary
<1–3 sentences, number of findings per level>

### Findings
| # | Area | Location | Finding | Legal basis | Level | Suggestion |

### Wording suggestions
<concrete text for banners, notices, labels – in the language of the offering>

### Open points for a professional
<what only a lawyer / DPO / tax adviser can decide>
```

## Rules
- Cite legal bases with article/section. Invent nothing. If the legal situation is disputed or changing, say so explicitly.
- The law changes (AI Act deadlines, rulings, guidelines). When in doubt, check current sources (EUR-Lex, gesetze-im-internet.de, EDPB, DSK, national DPAs) rather than answering from memory.
- Never call a template "safe from cease-and-desist".
- Recommend reputable generators for full legal texts rather than inventing them; check existing texts against the checklists.
- Legal texts must be understandable in the language of the offering; use skills `text-review` and `i18n` for other languages.
- Write the report in the user's language.
