# Project memory

## Product intent & audience

- Landing page for "Arbitrage Bro Scan," a cryptocurrency arbitrage scanner.
- Audience: crypto traders looking for real-time arbitrage spread opportunities.
- Goal: showcase scanner capabilities with live data previews and drive engagement.

## Brand personality & voice

- Dark space-themed aesthetic.
- Technical but accessible; trader-oriented.
- Bilingual: English (default) and Russian.

## Visual design & UX direction

- Dark space theme: deep backgrounds, cosmic accents.
- Multi-section single-page landing layout.
- Real-time spread data preview components.
- Interactive UI elements throughout.
- Language switcher in top menu (`EN`/`RU` button toggle); English is the default on load.
- Repeated button press toggles between EN and RU.

## Decisions, constraints & rationale

- Framework: SvelteKit.
- English set as the default language; Russian available via toggle — user requested English as standard.
- Language switcher implemented as a simple toggle button (RU ↔ EN) rather than a dropdown, given only two languages.
- GitHub repository visibility cannot be changed from within Bolt/code; must be done via GitHub Settings → Danger Zone → Change repository visibility → Public.
- Bolt creates new GitHub repositories as private by default.
- Production build (`npm run build`) passes without errors after language switcher addition.

## Open questions & assumptions

- Only EN and RU languages are needed; no plan for additional locales yet.
- Language persistence (localStorage/cookie) not confirmed — current implementation may default to English on every page load.
- GitHub repo is not yet connected; public visibility must be configured manually after repo creation.
- Content for real-time spread data previews may need actual API integration (not yet confirmed).