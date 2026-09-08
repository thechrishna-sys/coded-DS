# Ascend Design System

A component library and token system built from Figma, for building consistent Ascend product experiences.

## What's inside

```
apps/
  docs/          → Component showcase website (Next.js)
packages/
  components/    → React component library
  tokens/        → Design tokens (colors, spacing, typography)
  icons/         → Icon set
```

## Getting started

**Prerequisites:** Node.js 18+, npm 9+

```bash
# Install dependencies
npm install

# Start the docs site and watch for changes
npm run dev
```

The docs site runs at `http://localhost:3000`.

## Components

| Component | Description |
|---|---|
| `Avatar` | User and entity avatars |
| `Banner` | Full-width promotional banners |
| `Breadcrumb` | Page hierarchy navigation |
| `Button` | Primary, secondary, ghost, and destructive actions |
| `Carousel` | Horizontal scrolling content |
| `Checkbox` / `CheckboxGroup` | Single and grouped checkbox controls |
| `Radio` / `RadioGroup` | Single and grouped radio controls |
| `Card` | General-purpose content container |
| `FilterButton` | Toggle button for filter state |
| `FilterDropdown` | Filter button with floating single-select menu |
| `FilterDropdownMenu` | Standalone filter + sort dropdown panel |
| `FilterPanel` | Sidebar accordion filter panel |
| `SortDropdown` | Sort control with collapsed/open states |
| `FormField` | Input, textarea, and select form controls |
| `Header` | Page-level header with title and actions |
| `IconButton` | Icon-only action button |
| `Logo` | Ascend brand logo |
| `MarketplaceCard` | Tool/integration cards for marketplace views |
| `MeetingCard` | Meeting summary cards |
| `NewExperiencePopUp` | Onboarding / feature announcement popover |
| `Popover` | Generic floating popover |
| `Search` | Search input control |
| `AISearchBar` | AI-enhanced search bar |
| `BottomSearchBar` | Mobile bottom search bar with tools |
| `Tabs` | Horizontal tab navigation |
| `Toast` / `InlineMessage` | Feedback messages |
| `ToggleMenu` / `ToggleSwitch` | Toggle controls |
| `Tooltip` | Hover tooltip |
| `TopNavigation` | Top app navigation bar |
| `BottomNavigation` | Mobile bottom navigation bar |
| `AIOrb` | Animated AI presence indicator |
| `AscendAnalysis` | AI analysis display card |
| `PromptChip` / `SearchPrompt` / `PromptCards` | AI prompt entry surfaces |
| `AskAscendPanel` | AI chat panel |
| `ContextTag` / `StatusTag` / `MarketplaceTag` / `LabelTag` | Tag variants |

## Design tokens

Tokens live in `packages/tokens/dist/css/tokens.css` and are loaded globally. They follow a two-layer structure:

**Global** — raw palette values:
```css
--color-primary-100: #007680;
--color-neutral-900: #282828;
--spacing-space-16: 16px;
```

**Semantic** — purpose-named aliases:
```css
--text-primary: #282828;
--icon-action-default: #007680;
--surface-fill-brand-300: #e2eeef;
--stroke-cool-neutral-600: #dfe7e8;
```

Always use semantic tokens in components. Global tokens are the source of truth for the semantic layer — not for direct use.

## Using a component

Components are not yet published to npm. To use them, clone this repo and reference the package locally or run the docs site.

```tsx
import { Button, Avatar, FilterPanel } from '@ds/components'

<Button variant="primary" size="medium">Get started</Button>
```

## Tech stack

- **React 18** with TypeScript
- **Next.js 15** (docs site)
- **Turborepo** (monorepo build orchestration)
- **CSS custom properties** for tokens — no Tailwind in components
- **Inline styles** — components are fully portable with no external CSS dependency
- **Material Symbols Rounded** for icons (font-based)

## Contributing

1. Fork the repo
2. Create a branch: `git checkout -b feat/your-component`
3. Add your component in `packages/components/src/primitives/YourComponent/`
4. Export it from `packages/components/src/index.ts`
5. Add a docs page in `apps/docs/app/(docs)/components/your-component/`
6. Open a pull request

## License

MIT
