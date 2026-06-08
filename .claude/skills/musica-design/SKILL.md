---
name: musica-design
description: Use this skill whenever you write, edit, or review code in the MusicA repo (Vue 3 prototype for mua tác quyền âm nhạc marketplace). Triggers automatically when working with .vue / .css / .js files under src/, when adding new pages or components, when writing user-facing copy in Vietnamese, or when modifying pricing / variant / contract logic. The skill enforces the design tokens, the WaveBars / CheckList / SectionHead primitives, the marketplace terminology (tác quyền, NOT license), and the architectural layering documented in docs/.
---

# MusicA Design & Architecture Skill

You are working in **MusicA** — a Vue 3 prototype for a music **tác quyền** (copyright/rights) marketplace that brokers between buyers and artists. Before writing any code, follow these rules.

## 1. Read these docs first (always)

When the task involves UI / copy / structure, read in order:
1. [docs/BUSINESS_MODEL.md](../../../docs/BUSINESS_MODEL.md) — domain & glossary
2. [docs/ARCHITECTURE.md](../../../docs/ARCHITECTURE.md) — file layout & conventions
3. [docs/DESIGN_SYSTEM.md](../../../docs/DESIGN_SYSTEM.md) — tokens & components

Don't skip. The user has explicitly approved this design language — deviating without permission wastes the user's time.

## 2. Terminology — STRICT

This is a marketplace, NOT a licensing platform. Use these words:

| Use this                          | Never write                            |
|-----------------------------------|----------------------------------------|
| Mua tác quyền                     | Cấp phép, license, licensing           |
| Gói tác quyền                     | License package                        |
| Bộ tài sản tác quyền              | License files                          |
| Sàn môi giới                      | Nền tảng cấp phép                      |
| Hợp đồng tác quyền                | Hợp đồng cấp phép (only OK inside legal contract body) |
| Giao dịch                         | Cấp phép (as a verb)                   |
| Mã giao dịch                      | Mã license                             |

Exception: inside the formal contract body in `CheckoutView` step 2, legal Vietnamese ("Bên A", "Bên B", "quyền sử dụng") is fine because it mirrors actual contract language.

## 3. Visual tokens — STRICT

- Palette: white / blue (`--c-blue-*`) / teal (`--c-teal-*`). Gradient `--grad-brand` for primary actions.
- Font: only `'Plus Jakarta Sans'` (already loaded). Don't add other fonts.
- Border radius: `--radius-lg` (22px) for main cards, `--radius-md` for inner, `--radius-full` for pills.
- Shadow: `--shadow-md` for elevated cards, `--shadow-glow` only for primary buttons / play FAB.
- Animation easing: `--ease-out` for enter, `--ease-in-out` for loops. Duration 200–350ms for micro, 600–900ms for reveal.

❌ Never hardcode color hex when a token exists. ❌ Never invent a new radius scale. ❌ Never write inline style for static values.

## 4. Primitive components — REUSE, DON'T REINVENT

These exist in `src/components/ui/`:

- `<WaveBars>` — ALL waveforms in the app use this. Props: `peaks, bars, size (xs|sm|md|lg), variant (solid|translucent|muted), progress, animate, animateOnHover`. Read its source at [src/components/ui/WaveBars.vue](../../../src/components/ui/WaveBars.vue) before "improving" waveforms.
- `<CheckList>` — ticked list for deliverables, benefits, contract clauses. Props: `items` (array of strings or `{ label, hint }`).
- `<SectionHead>` — section header with eyebrow + h2 + description + optional actions slot.

If your task is to "show a wave / sound bars / equalizer" — the answer is `<WaveBars>` 99% of the time. If your task is "show a list with checkmarks" — the answer is `<CheckList>`.

Adding a new primitive? Drop it in `components/ui/` and update `docs/DESIGN_SYSTEM.md` section 4.

## 5. Folder placement rules

```
components/layout/  → AppHeader, AppFooter, BrandLogo  (mounted in App.vue)
components/ui/      → primitives: WaveBars, CheckList, SectionHead, …
components/product/ → product-domain: ProductCard, future ProductPlayer
components/checkout/→ flow-domain: future SignaturePad, PaymentMethods
views/              → one per route, page-level orchestration
composables/        → reusable logic (useReveal, …)
stores/             → pinia stores
data/               → mock data + domain helpers
```

A component used in ≥ 2 places must live in `components/` not `views/`. A component < 60 LoC used once may stay inline.

## 6. Variant / pricing logic

Read [docs/BUSINESS_MODEL.md § 3](../../../docs/BUSINESS_MODEL.md) for the formula. The canonical implementation lives in `ProductView.vue` computed `calc`. If you change the formula, update both `BUSINESS_MODEL.md` and the test value in `ProductView.vue`.

## 7. Animation rules

- Scroll-reveal: add class `reveal` to the element. The `useReveal()` composable in `App.vue` will toggle `is-visible` when scrolled into view.
- Page transitions are handled by `App.vue` — don't add additional transitions on `<router-view>`.
- Use `pulseRing` keyframe (defined in `styles/main.css`) for live indicators.

Don't write new keyframes if an existing one fits. Don't use parallax. Don't use scroll-jacking.

## 8. Mobile responsiveness

Target breakpoints: 1024 / 760 / 640 / 480 px. Test mobile (375 viewport) before saying done. Header at < 640 hides text labels, keeps only icon buttons + primary CTA. Hero stats fold to 2-col.

## 9. Before declaring "done"

- [ ] `npm run build` passes
- [ ] **Every view template has a single root element** (e.g. `<div class="xyz-view">`). Multiple roots create a Vue fragment, and `App.vue` wraps `<router-view>` in `<Transition mode="out-in">` which silently fails on fragments — the URL updates but the page never renders until F5. Catches: any new view, any view edited from a single section to multiple.
- [ ] Navigating between views via in-app links actually mounts the new view (not just URL change)
- [ ] No `cấp phép` / `license` strings in user-facing copy (only inside contract body)
- [ ] Used existing primitives (no new waveform code, no new check-list pattern)
- [ ] CSS uses tokens (no hardcoded hex from the brand palette)
- [ ] Mobile viewport doesn't horizontally scroll

## 10. When in doubt

Show the user a short proposal (1-3 sentences) referencing the relevant doc section. Don't invent design decisions silently.
