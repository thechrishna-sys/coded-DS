# Design System — AI Guidelines

## What this project is
A design system extracted from Figma, built into code, and showcased on a website.
Vibe coding designers use this site to see components, understand their API, and copy/generate code.

## How it works
Figma → tokens → components → docs website

## Folder responsibilities
- `packages/tokens/` — All design decisions (colors, spacing, typography). Single source of truth. Never hardcode values — always use tokens.
- `packages/components/` — React components built using tokens. Organized as foundations → primitives → patterns.
- `packages/icons/` — SVG icons converted to React components.
- `apps/docs/` — The showcase website. Imports from packages/components and packages/tokens.
- `scripts/` — Automation. figma-sync pulls from Figma API. transform-tokens converts raw JSON to CSS/JS/Tailwind.
- `tooling/` — Shared ESLint, TypeScript, Tailwind config. Never modify these per-package.

## Rules
- Never hardcode colors, spacing, or typography — always reference a token
- Every component lives in its own folder: ComponentName/ComponentName.tsx + index.ts
- Tokens in dist/ are auto-generated — never edit them manually
- All components must work without any props (sensible defaults required)

## Figma Rules — When Creating or Editing Frames/Components in Figma

### Variables & Tokens
- Always bind variables to every property that accepts them: fills, strokes, corner radius, padding, gap, opacity, effects
- Never leave a hardcoded hex color, spacing number, or radius value unbound if an equivalent variable exists
- If the target file has no shared library attached, create a local variable collection named `Ascend DS / <category>` (e.g. `Ascend DS / AI`, `Ascend DS / Core`) and populate it before binding
- Variable naming convention: `category/subcategory/name` — e.g. `colour/stroke/ai-gradient`, `number/padding-16`, `number/corner-radius-8`

### Component Structure
- Every component created in Figma must use auto-layout (not absolute positioning)
- Layer names must match the React component and prop names exactly — e.g. a frame for `<AscendAnalysis>` is named `Ascend Analysis`
- Group related child layers under named frames that reflect their role: `Header`, `Body`, `Footer`, `Icon`, `Label`
- Use components and instances, not detached copies — if a pattern repeats, make it a Figma component

### Naming Conventions
- Frame/component names: Title Case, no underscores — e.g. `AI Orb`, `Prompt Chip`, `Ascend Analysis`
- Variant properties: match the React prop name and type — e.g. `Size=Large`, `Size=Small`, `Show Actions=True`
- Variable collections: `Ascend DS / <Category>` — keep one collection per design domain

### Text Styles (Typography)
- The DS uses shared text styles — NOT variables — for typography. Never set `fontSize`, `fontName`, or `lineHeight` as raw values on text nodes.
- Always apply a text style via `node.textStyleId` using `figma.importStyleByKeyAsync(key)` before setting any text property.
- The DS library must be published and enabled in the target file before any text style can be imported. Verify this first — `figma.getLocalTextStyles()` returning empty is the signal it is not linked.
- Match text styles using `fontSize + fontWeight + lineHeight` together — never by size and weight alone. Two styles share 14px Regular:
  - `Body/Medium/Regular_14` — lineHeight 24px, letterSpacing 0 — use for all body text, labels, descriptions
  - `All Caps Title/Regular_14` — lineHeight 20px, letterSpacing 4px — use only when text is genuinely ALL CAPS
- After applying any `All Caps Title/*` style, verify `node.characters === node.characters.toUpperCase()` — if not, reassign to the matching `Body/Medium/*` style.
- Font style name normalisation: Figma stores Open Sans semibold as `"Semi Bold"` (with space) but DS style keys use `"SemiBold"` (no space). Normalise before lookup: `style.replace('Semi Bold', 'SemiBold')`.

### Verification Before Finishing
- After creating a frame, select it and confirm every fill, stroke, radius, and spacing value shows a variable chip (purple dot) in the properties panel — not a raw value
- Confirm every text node shows a text style chip (the style name appears in the text panel) — not raw font properties
- If any property is unbound, bind it before considering the task done

## Typography Rules (apply to ALL pages, ALL components, ALL CSS)

These rules govern every `font-size` and `line-height` value the agent introduces or modifies. They are also enforced by the Figma file — Figma is the singular source of truth and already complies.

1. **Font-size must be an even integer (px).** Allowed: 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30, 32, 36, 40, 48, 56 …  
   Never use odd font sizes (no 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33, 35, etc.).

2. **Line-height must equal font-size + 8 px.** Examples:
   | Font-size | Line-height |
   |---|---|
   | 10 | 18 |
   | 12 | 20 |
   | 14 | 22 |
   | 16 | 24 |
   | 18 | 26 |
   | 20 | 28 |
   | 22 | 30 |
   | 24 | 32 |
   | 26 | 34 |
   | 28 | 36 |
   | 30 | 38 |
   | 32 | 40 |
   | 36 | 44 |

3. **Applies to:** raw CSS values, inline `style={{ fontSize, lineHeight }}` props, anywhere typography appears in code. Reference an existing `--font-size-*` / `--line-height-*` token if it complies; otherwise write the literal pixel value matching the rule.

4. **Figma is the singular source of truth.** **Do NOT auto-regenerate `packages/tokens/dist/css/tokens.css`** (i.e. do not run `npm run figma:sync` or `npm run tokens:transform`). Figma's text styles already comply with the rule. If a `tokens.css` value looks stale or non-compliant, do not patch the generated file in isolation — treat Figma as canonical and surface the drift to the user instead.

5. **Never** set `line-height: 1`, `normal`, `1.5`, or any unitless ratio. Always an explicit pixel value matching size + 8.
