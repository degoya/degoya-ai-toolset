---
name: performance
description: Reviews and improves performance of websites, web apps, mobile apps and APIs: Core Web Vitals (LCP, INP, CLS), load time, bundle size, images, caching, database queries, API latency, app start-up and memory, with a performance budget. Use for "it's too slow", "performance review", "Core Web Vitals", "improve Lighthouse score", "load time", "bundle too big", "N+1".
---

# Performance

Measure before optimising. Every recommendation names its expected effect on a metric.

## Workflow
1. **Set a budget** (if none exists, propose these defaults):
   - Web: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1 (75th percentile of real users, mobile); initial JS ≤ 200 KB compressed.
   - API: p95 ≤ 300 ms for reads, ≤ 1 s for writes; error rate < 0.1 %.
   - App: cold start ≤ 2 s, 60 fps while scrolling, no ANRs.
2. **Measure:** field data (CrUX, RUM, analytics) before lab data (Lighthouse, WebPageTest, Chrome DevTools Performance, profilers, `EXPLAIN ANALYZE`, APM traces, Android Studio/Xcode Instruments).
3. **Find the bottleneck** with the checklist, biggest lever first.
4. **Report** with actions and a before/after measurement plan.

## Frontend checklist
- [ ] **LCP:** LCP element (usually hero image/heading) early in the HTML, not lazy-loaded, `fetchpriority="high"`, preload; server response (TTFB) ≤ 0.8 s; no render-blocking CSS/JS.
- [ ] **INP:** split long tasks (> 50 ms), debounce expensive handlers, reduce hydration, review and defer third-party scripts (tag manager, chat, A/B tests).
- [ ] **CLS:** `width`/`height` or `aspect-ratio` for images/videos/embeds; reserve space for banners and ads; `font-display` plus matching fallback metrics.
- [ ] Images: AVIF/WebP, `srcset`/`sizes`, lazy loading below the fold, CDN.
- [ ] JS: route-based code splitting, tree shaking, replace heavy libraries (Moment → date-fns/Intl), bundle analysis.
- [ ] Caching: long `Cache-Control` for versioned assets, Brotli/gzip, HTTP/2 or 3.
- [ ] Fonts: few weights, subsetting, self-hosted (also GDPR).

## Backend and data checklist
- [ ] No N+1 queries (eager loading); indexes for filters/sorting; pagination instead of "load everything".
- [ ] Caching layer (HTTP, application, query) with clear invalidation.
- [ ] Expensive work asynchronous (queue) rather than in the request.
- [ ] Small payloads (only needed fields), compression.
- [ ] Timeouts, connection pools, limits for external services.

## Mobile checklist
- [ ] Cold start: minimise start-up work, lazy initialisation.
- [ ] Virtualised lists, images decoded at display size, no heavy work on the main thread.
- [ ] Network: caching, batching, offline support; app size.

## Report format
```
## Performance review: <subject>
Budget: … · Measurement basis: <field/lab, device, network>
### Current values
| Metric | Current | Target | Status |
### Actions (biggest lever first)
| # | Bottleneck | Action | Expected effect | Effort |
### Measurement plan
<how before/after will be proven>
```
Write the report in the user's language.
