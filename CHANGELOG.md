# Changelog

All notable changes to **Ascend DS 2.0 — Code Design System** are documented here.

---

## [v1.1] — 2026-05-12

### New Components
- **AIButton** (`primitives/AIButton`) — Standalone AI action button extracted from Figma node 2016:3283. Two sizes: `default` (36px, card badge) and `large` (48px, standalone CTA). Active state shows a teal ring when the AI panel is open. Uses a hybrid img-sphere + inline-SVG-spark approach to preserve Figma filter effects while keeping the spark fill token-driven (`--icon-action-inverse`).
- **Stack** (`foundations/Stack`) — Flexbox layout primitive with direction, gap, align, justify, and wrap props. Accepts any `as` element type.
- **Grid** (`foundations/Grid`) — CSS Grid layout primitive with `cols`, `gap`, and `Grid.Item` for spanning. Token-driven spacing.
- **Container** (`foundations/Container`) — Max-width wrapper with responsive padding. Default max-width 1280px.

### Improved Components
- **MarketplaceCard** — Full internal state machine (Default → Hover → Ascend Analysis → Default) driven by `useState`. No state props required for standard usage. `showCtu` and `showCts` now auto-derive from `toolType` (`Accelerators` → CTU, `Agents` → CTS). AI badge replaced with the new `AIButton` component. `state` prop still available as an escape hatch for controlled usage.
- **MarketplaceTag** — Gradient colours corrected from Figma variable definitions (CTU: `#D3DEF1→#E6E0F9`, CTS: `#D6F6E6→#E5F4E1`, Projects: `#D7EAED→#D2EDF3`). Added two inset box-shadows per tag type using `--shadow-color-inner-shadow-color-*` tokens to match the Figma emboss effect. Font corrected to 12px / 600 weight / 20px line-height. Default labels updated to "Use in client service" and "Internal use".

### New Assets
- `ai-button-sphere.svg` — AI button gradient sphere with Figma drop + inner shadow filters baked in
- `ai-button-spark.svg` — Spark icon for AI button
- Progress pill assets: `nav-progress-pill-day/noon/evening/night.png` (4 files)
- Progress popup assets: `nav-progress-popup-day/noon/evening/night.png` (4 files)
- `avatar-default.png`, `banner-ribbon.png`

### Docs
- New docs page: `/components/ai-button` — sizes, states, interactive toggle, in-context demo, design guidelines, props table
- New docs page: `/components/stack`, `/components/grid`, `/components/container`
- Rewritten docs page: `/components/marketplace-card` — all 4 states, live tag demos, CTA variants, like state, controlled state explorer
- Sidebar: AI Button entry added under AI ✦ section; Layout Primitives section added

### Work Log
- `_work-logs/work-log.html` — decisions 17–24 logged, Layer 7 flowchart subgraph (MarketplaceCard & AIButton), timeline items 9–11, interactive modal with zoom/pan for the decision flowchart
- Auto-change logger: `PostToolUse` hook writes to `_work-logs/auto-log.tsv` (gitignored, local only)

### Security
- Upgraded Next.js `15.3.1 → 15.5.18` in `apps/docs` and `apps/home` — resolves 2 critical, 10 high, 15 moderate CVEs (cache poisoning, SSRF, RCE, DoS, XSS, middleware bypass)
- Added `.npmrc` with `audit-level=high`. One remaining moderate advisory (GHSA-qx2v-qp2m-jg93) is PostCSS pinned as an exact internal dep by Next.js — not exploitable in build-time CSS compilation context

### Key Architecture Decisions
- **CSS `var()` in SVG loaded via `<img>` does not work** — SVG is isolated from page CSS; presentation attributes like `stop-color` cannot reference CSS custom properties. Must use hardcoded hex values in external SVG files.
- **npm `overrides` cannot override exact direct deps** — when a package pins a dep at an exact version (e.g. `"postcss": "8.4.31"`), npm overrides are ignored for that resolution.

---

## [v1.0] — 2026-04-28

### Foundation
- Monorepo setup with Turborepo — packages: `@ds/tokens`, `@ds/components`, `@ds/icons`, `@ds/assets`
- Token pipeline: Figma → raw JSON → `dist/css/tokens.css`, `dist/js/tokens.js`, Tailwind config
- Styling philosophy: inline `CSSProperties` across all components — no Tailwind, no CSS Modules
- 3-layer component architecture: `foundations` → `primitives` → `patterns`

### Components (35+)
AIOrb, AIPrompt, AISearchBar, AscendAnalysis, AskAscendPanel, Avatar, Banner, BottomNavigation, BottomSearchBar, Breadcrumb, Button, CheckboxRadio, Chip, Comments, FilterSort, FormField, Header, Loader, Logo, MarketplaceCard, MeetingCard, Menu, NewExperiencePopUp, Panel, Popover, Search, Stepper, Tabs, Tag, Toast, Toggle, Tooltip, TopNavigation

### Docs Site
- `apps/docs` — Next.js 15 docs website with live component demos, props tables, code blocks
- Sidebar navigation covering all components

### Tooling
- `tooling/eslint` — shared ESLint config with `no-hardcoded-typography` custom rule
- `scripts/figma-sync.js` — pulls Figma API tokens to raw JSON
- `scripts/copy-assets.js` — syncs assets to `apps/docs/public/`

---

*Format based on [Keep a Changelog](https://keepachangelog.com). Versions follow [Semantic Versioning](https://semver.org).*
