---
name: rupeerookie-maintainer
description: Build, review, debug, and improve the RupeeRookie Indian paper-trading simulator and investor academy. Use for work in this repository involving its React interface, Express APIs, market-data calculations, portfolio simulation, Academy, accessibility, PWA, responsive layouts, or user progress systems.
---

# RupeeRookie Maintainer

Maintain RupeeRookie as a trustworthy educational simulator for teenagers and first-time Indian-market learners. Preserve its emphasis on learning, reflection and risk awareness rather than trading frequency.

## Understand the repository

Before changing code, inspect the affected component and its shared state, types and helpers. Important locations are:

- `src/App.tsx`: lazy page loading, app-level navigation and global overlays
- `src/components/`: screens, hubs, dialogs and responsive navigation
- `src/context/SimulatorContext.tsx`: accounts, orders, portfolio, XP and persisted simulator state
- `src/context/AccessibilityContext.tsx`: device-local accessibility preferences
- `src/data/indianCompanies.ts`: curated company catalogue and fallback fundamentals
- `src/server/`: external market-data adapters
- `src/utils/`: formatting and calculation helpers
- `server.ts`: Express routes, sessions, AI availability and market-data aggregation
- `public/`: PWA manifest, service worker and static assets

Reuse existing components, formatters, types and persistence conventions. Do not introduce a second source of truth for portfolio values, market changes, XP or user activity.

## Product invariants

- Treat all orders and portfolios as simulations using virtual money.
- Keep the educational-not-investment-advice notice visible wherever an output could resemble a recommendation.
- Never imply that data is live unless the active provider and timestamp support that claim.
- Label figures as `Live`, `Delayed`, `Last close` or `Simulated` as appropriate.
- When the market is closed, remove live-style wording and show the last available session timestamp.
- Calculate absolute change as `price - previousClose` and percentage change as `(change / previousClose) * 100`. Avoid maintaining independently editable change values.
- Use Indian currency formatting and locale conventions through the existing formatting helpers.
- A new user must begin with no holdings, trades, profit history, ranking or unlocked activity badges.
- Award XP and achievements only for persisted, verifiable user actions. Never seed identical fictional progress for every user.
- Keep Trader DNA and meaningful rankings locked until their minimum genuine-activity thresholds are met.
- AI availability, sources and timestamps must reflect real configuration. If no AI key is available, show an unavailable state instead of simulated AI claims.
- Do not encourage rapid or excessive trading. Reward lessons, risk checks, written plans, patience and post-trade reflection.

## Make interface changes

Place features according to the existing information architecture:

- **Home:** daily mission, portfolio summary, learning progress and market status
- **Markets:** discovery, company search, filters and watchlist entry points
- **Portfolio:** positions, orders, watchlist and portfolio risk
- **Practice:** Replay OS, trade planning and journal workflows
- **Learn:** Academy, case studies, portfolio models, AI Coach and calculators
- **Progress:** Trader DNA, challenges and achievements
- **Support:** help, data controls and privacy

Desktop navigation should remain concise. Mobile uses Home, Markets, Portfolio, Learn and More. Put secondary actions inside the existing More sheet or contextual menus rather than adding another permanent navigation row.

For every visual change:

- Check 390px mobile, 768px tablet, 1024px laptop and 1440px desktop layouts.
- Avoid horizontal page scrolling, clipped labels and fixed-width controls.
- Use responsive wrapping, grids or a scrollable tablist where the content cannot fit.
- Keep touch targets at least 44px when the large-touch setting is enabled.
- Preserve keyboard access, visible focus, Escape-to-close and descriptive labels for icon-only controls.
- Respect high contrast, reduced motion, text scaling and dyslexia-friendly reading settings.
- Provide a plain-language text summary for charts and heatmaps; do not rely on color alone.
- Use skeletons only for genuine loading and explicit empty states for users with no activity.

## Work with market data

Prefer server-side adapters over direct third-party browser calls. Treat remote data as optional and fallible.

- Validate numeric provider fields before merging them into the catalogue.
- Keep provider source and retrieval timestamp with the resulting quote.
- Derive change and percentage from one consistent price/previous-close pair.
- Preserve a clearly labelled fallback when a provider is unavailable.
- Never silently replace unavailable real data with invented live-looking numbers.
- Make retry and stale states understandable without blocking Academy or locally stored records.

When modifying company data or quote aggregation, verify every catalogue entry for finite prices, positive previous closes and internally consistent change math.

## Preserve state and privacy

Scope browser-storage keys by user ID when data is personal. Scope company-specific drafts by both user ID and symbol. Maintain compatibility with existing stored records when changing a schema, or add a small migration.

Screenshots, journal notes, learning records and preferences remain on the device unless the user explicitly configures a sync/export integration. Do not add external transmission, analytics or cloud sync as an incidental implementation detail.

Session changes must preserve the inactivity logout behavior. Never log secrets, passwords, API keys, tokens or personal journal contents.

## Keep performance smooth

- Preserve route-level lazy loading and the existing page skeletons.
- Avoid rendering the full company catalogue when pagination, virtualization or progressive disclosure is suitable.
- Keep advanced screeners inside their drawer.
- Memoize expensive derived portfolio and market calculations when their inputs are stable.
- Avoid refetch loops and duplicate polling intervals.
- Preserve the user's filters and scroll position when navigating between sections.
- Respect reduced-motion preferences for transitions and animated market elements.

## Validate changes

Run these checks after implementation:

```bash
npm run lint
npm run build
```

For market-data or portfolio changes, also check:

- `change ≈ price - previousClose`
- `changePercent ≈ change / previousClose × 100`
- portfolio value equals cash plus current holding values
- P&L signs and percentages agree
- closed-market and stale-data labels are correct

For progress changes, test a new user, a partially active user and a qualifying active user. For responsive changes, inspect representative mobile, tablet, laptop and desktop widths.

Do not declare the task complete if TypeScript or the production build fails. Report external-provider limitations separately from application defects.
