# WCAG 2.2 AA checklist (practical)

## Perceivable
- [ ] **1.1.1** Informative images have meaningful alt text; decorative ones `alt=""`. Icon buttons have an accessible name.
- [ ] **1.2.x** Videos: captions; audio: transcript; audio description where visual content is essential.
- [ ] **1.3.1** Semantics: heading hierarchy (one h1, no skipped levels), lists, tables with `<th>`/`scope`, landmarks (`header`, `nav`, `main`, `footer`).
- [ ] **1.3.5** Form fields for personal data use `autocomplete`.
- [ ] **1.4.1** Information not conveyed by colour alone (errors also by text/icon).
- [ ] **1.4.3** Text contrast ≥ 4.5:1, large text (≥ 24 px or 18.66 px bold) ≥ 3:1.
- [ ] **1.4.4 / 1.4.10** 200 % zoom without loss; reflow at 320 CSS px width without horizontal scrolling.
- [ ] **1.4.11** Non-text contrast ≥ 3:1 (field borders, focus indicator, icons).
- [ ] **1.4.12** Text spacing adjustable without clipping.
- [ ] **1.4.13** Tooltips/hover content: dismissible (Esc), hoverable, persistent.

## Operable
- [ ] **2.1.1 / 2.1.2** Everything keyboard-operable, no keyboard trap.
- [ ] **2.4.1** "Skip to content" link.
- [ ] **2.4.2** Meaningful `<title>` per page/screen.
- [ ] **2.4.3** Logical focus order; modals trap focus and return it on close.
- [ ] **2.4.4** Link text understandable ("More about pricing" instead of "here").
- [ ] **2.4.7** Focus visible; **2.4.11 (new in 2.2)** focus not hidden by sticky headers/cookie banners.
- [ ] **2.5.7 (new in 2.2)** Drag-and-drop has a single-pointer alternative.
- [ ] **2.5.8 (new in 2.2)** Target size at least 24×24 CSS px (apps: 44×44 pt iOS, 48×48 dp Android recommended).
- [ ] **2.2.x** Time limits adjustable; carousels/animations pausable; `prefers-reduced-motion` respected.

## Understandable
- [ ] **3.1.1 / 3.1.2** `lang` on `<html>` and on language changes within content.
- [ ] **3.2.x** No unexpected context changes on focus/input; consistent navigation; **3.2.6 (new in 2.2)** help in a consistent place.
- [ ] **3.3.1 / 3.3.3** Errors described in text with a correction suggestion, linked to the field (`aria-describedby`).
- [ ] **3.3.2** Visible labels (placeholder is not a label), required fields marked.
- [ ] **3.3.7 (new in 2.2)** Do not ask for previously entered data again.
- [ ] **3.3.8 (new in 2.2)** Login without cognitive test (password managers/paste allowed, no puzzle-only CAPTCHA).

## Robust
- [ ] **4.1.2** Name, role, value for all controls (custom components with correct ARIA roles and states such as `aria-expanded`).
- [ ] **4.1.3** Status messages via `role="status"`/`aria-live` ("3 results found", "Saved").

## Mobile apps additionally
- [ ] `contentDescription` (Android) / `accessibilityLabel` (iOS) set.
- [ ] Dynamic font sizes (Dynamic Type, sp) supported.
- [ ] Portrait and landscape.

## Documents
- [ ] PDFs tagged (PDF/UA), reading order, alt texts, document language.

## Accessibility statement (EAA/BFSG)
- [ ] Description of the service, how requirements are met, competent market surveillance authority; easy to find.
