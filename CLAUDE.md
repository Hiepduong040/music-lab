# MusicA — Claude Code orientation

Project context loaded automatically by Claude Code. Keep this file short — link to docs for detail.

## What this is

Vue 3 prototype for **MusicA**, an e-commerce marketplace that connects buyers and artists for music **tac quyen** transactions. NOT a rights-issuance platform, NOT a streaming service.

## Critical rules (do not violate)

1. **Terminology**: use "giao dịch tác quyền" / "gói tác quyền" / "thương mại điện tử". Never use outdated rights-issuance wording in UI copy (legal contract body excepted).
2. **Reuse primitives**: any waveform → `<WaveBars>` from `src/components/ui/`. Any tick list → `<CheckList>`. Any section header → `<SectionHead>`.
3. **Design tokens only**: colors / radius / shadow come from CSS variables in `src/styles/main.css`. No hardcoded brand hex.
4. **Stack**: Vue 3 + Vite + Pinia + vue-router. No Tailwind, no SCSS, no UI library.

## Files to read before non-trivial work

- [docs/BUSINESS_MODEL.md](docs/BUSINESS_MODEL.md) — domain, pricing formula, glossary
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — folder layout, conventions, when to add a new component
- [docs/DESIGN_SYSTEM.md](docs/DESIGN_SYSTEM.md) — tokens, primitives, animation rules

The skill `.claude/skills/musica-design/SKILL.md` enforces these rules — it should auto-load when working in this repo.

## Run

```bash
npm install
npm run dev      # http://localhost:5173
npm run build
```

Dev server is preconfigured in `.claude/launch.json` under name `musica-dev`.

## Repo layout (quick)

```
src/
├─ components/{layout,ui,product,checkout}/
├─ composables/   stores/   data/   styles/   router/   views/
docs/             {DESIGN_SYSTEM, ARCHITECTURE, BUSINESS_MODEL}.md
.claude/skills/musica-design/SKILL.md
```
