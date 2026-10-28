# Acre &amp; Key — Real Estate Marketplace

A production-grade, fully responsive real-estate marketplace built with **React 19 + Vite 7 + Tailwind CSS v4 + Framer Motion 14**. Search, filter, sort and page through a catalogue of 24 verified listings across 8 U.S. metros, open rich property dossiers, shortlist homes, run head-to-head comparisons, browse agent profiles, sign in, and submit your own listing through a four-step wizard.

> **Live demo:** deploy `dist/index.html` to any static host (see [Deployment](#deployment)). The production build is a single self-contained HTML file, so Netlify, Vercel, GitHub Pages, S3 or `npx serve dist` all work unchanged.

---

## Table of contents

1. [Quick start](#quick-start)
2. [Features](#features)
3. [Routes](#routes)
4. [Tech stack](#tech-stack)
5. [Project structure](#project-structure)
6. [Data model](#data-model)
7. [State &amp; persistence](#state--persistence)
8. [Design system](#design-system)
9. [Responsive behaviour &amp; screenshots](#responsive-behaviour--screenshots)
10. [Deployment](#deployment)
11. [Credits](#credits)

---

## Quick start

```bash
# 1. install dependencies (Node 20+ recommended)
npm install

# 2. start the dev server with HMR
npm run dev          # → http://localhost:5173

# 3. type-check
npx tsc --noEmit

# 4. production build (outputs a single inlined dist/index.html)
npm run build

# 5. preview the production build locally
npm run preview
```

No environment variables, API keys or backend services are required — the marketplace runs entirely on a typed local dataset plus browser `localStorage`.

---

## Features

### Discovery &amp; landing page
- **Search-first hero** with a live search console: Buy/Rent toggle, location autocomplete (cities + neighborhoods), home-style chips, bedroom filter and a purpose-aware budget slider that reports *"N homes match your criteria"* as you drag.
- Layered ambient background (blueprint grid, film grain, soft light blooms), Ken Burns hero imagery, an animated market-stat card and a rotating "just listed" chip.
- **Live ticker** of the most-viewed listings, **category bento grid** (6 housing types with live counts), **hand-picked listings** with animated tab filtering, a **neighborhood files** section with a sticky rail + auto-advancing spotlight, a four-step "how it works" timeline, agent carousel, rotating testimonial block, seller CTA and an animated stats band (count-up on scroll).
- Scroll-progress bar, scroll-reveal animations and hover micro-interactions throughout.

### Search, filters, sorting &amp; pagination (`/listings`)
- **Keyword** search across title, address, district, city, description, amenities and style.
- **Filters:** purpose (all / buy / rent), dual price sliders with quick presets, bedrooms, bathrooms, home style (8 types), metro area, minimum interior area, and 20 amenities (all combinable, AND-logic).
- **Sorting:** recommended, newest, price ↑/↓, largest interior, most bedrooms, most viewed.
- **Pagination:** 6 per page with numbered control, ellipsis collapsing, prev/next and a "Page X of Y · 13–18 of 24" readout; auto-scrolls to the top of the results.
- **URL-synced state:** every filter, sort and page is written to the query string (shareable/bookmarkable) and external links (hero search, footer, category tiles) are adopted back into the UI.
- **Three result views:** grid, list and a **split map view** with a stylised site-plan map whose price pins highlight in sync with card hover.
- Removable active-filter chips, sticky desktop filter rail, slide-in mobile filter drawer, and a designed empty state.

### Property detail (`/property/:id`)
- **Image gallery** with Ken-Burns-free crossfades, thumbnail rail, directional transitions, a full-screen **lightbox** (arrow-key + Esc navigation, filmstrip, counter) and a "+N more" tile.
- Key-facts grid (beds, baths, interior, lot, year built, floors, parking, energy rating), full description, listing highlights, **amenities grid**, and a location block pairing the stylised map with parks/transit/noise/school data.
- **Interactive mortgage calculator**: down-payment slider, rate and term inputs, live monthly figure, animated donut breakdown of principal &amp; interest / tax / insurance / HOA, plus total-cost-of-loan.
- Listing-history timeline, price-per-sq-ft, days-on-market, view counter and star rating.
- Sticky rail with the asking price, the assigned **agent card**, a validated **tour-request form** with a success state, click-to-call, wishlist toggle, HOA note.
- Save / compare / **copy share link** / print actions in the contextual top bar, breadcrumbs, and "similar homes nearby" scored by city, budget band, style and bedroom count.
- Graceful fallback screen for unknown or withdrawn listing IDs.

### Wishlist (`/wishlist`)
- Heart any card or detail page; state persists in `localStorage` and animates on toggle.
- Portfolio summary tiles (aggregate purchase value, average $/sqft, rentals saved, total bedrooms) with count-up animation.
- Grid/list view toggle, share-to-clipboard, clear-all, "match me with an agent" CTA, a **recently viewed** rail, and a curated fallback when the list is empty.

### Comparison (`/compare`)
- Up to **4 properties** side by side over 17 attribute rows (price, price/sqft, beds, baths, interior, lot, year built, floors, parking, HOA, energy, style, purpose, location, address, listed date, agent rating) plus an amenities row.
- **Best-in-row detection**: the strongest number in each comparable row is badged (cheapest, largest, newest) and tinted; amenities shared by every home in the tray are highlighted.
- Floating **compare tray** on every page with live thumbnails, per-item removal, slot placeholders and a spring-animated entrance.
- In-page picker modal with search to fill an empty slot; designed empty state when the tray is clear.

### Agent profiles (`/agents`, `/agents/:id`)
- Roster with search + metro filtering, verified badges, ratings, sales/volume/live-listing counts, languages, call/email/profile actions and a "how we hire" band.
- Agent dossier: portrait hero, animated stat counters, credentials, languages, territory and response time, their **live listings**, a validated contact form with success state, and a review breakdown with animated distribution bars.

### Authentication UI (`/auth`)
- Split-screen sign-in / create-account experience with an animated Ken-Burns visual panel, perk list and a security note.
- Tabbed modes, inline field validation (name, email format, 8-char password, confirm match, terms), password visibility toggle, remember-me, forgot-password toast, loading state on submit, **demo login**, and redirect-back-to-origin after sign-in.
- Signed-in users get an avatar menu in the navbar (saved homes, compare tray, my listings, sign out).

### Property submission (`/submit`)
- **Four-step wizard** — Basics → Location → Presentation → Review — with an animated stepper, progress bar, per-step validation and directional step transitions.
- Covers purpose, style, headline (with counter), price, year built, beds/baths/interior/lot/parking/floors, address, metro and district (with map-pin preview), 900-char description, three highlights, amenity chips and a photo picker (3–6 shots, ordered).
- **Live preview card** in a sticky rail that updates as you type, plus a listing checklist that ticks off as requirements are met.
- On submit the listing is minted into the catalogue (id `user-*`, flagged *Pending review*), stored locally, and immediately viewable at its own detail URL — a confirmation screen with listing stats follows.

### Cross-cutting
- Toast notification system (success / info / warn) with auto-dismiss progress bars.
- `HashRouter` so the deployed single-file build works from any path or host.
- Keyboard-focusable controls, ARIA labels on icon buttons, `aria-current` on pagination, `prefers-reduced-motion` support, lazy-loaded imagery, and semantic landmark markup.

---

## Routes

| Path | Page | Notes |
| --- | --- | --- |
| `/` | Landing | Hero search console, ticker, bento, featured, neighborhoods, steps, agents, testimonials, seller CTA, stats |
| `/listings` | Marketplace | Filters, sorting, pagination, grid/list/map views, URL-synced query string |
| `/property/:id` | Property dossier | Gallery + lightbox, amenities, location, mortgage calculator, agent, tour form, similar homes |
| `/compare` | Comparison table | Up to 4 homes, best-value badges, picker modal |
| `/wishlist` | Saved homes | Summary stats, share, clear, recently viewed |
| `/agents` | Agent roster | Search + metro filter, stats band |
| `/agents/:id` | Agent profile | Bio, credentials, live listings, contact form, reviews |
| `/submit` | List a home | 4-step wizard, live preview, checklist |
| `/auth` | Sign in / sign up | Standalone full-screen layout |
| `*` | 404 | Editorial fallback with featured picks |

---

## Tech stack

| Concern | Choice |
| --- | --- |
| UI library | React 19 (function components + hooks only) |
| Build tool | Vite 7 (`@vitejs/plugin-react`, `vite-plugin-singlefile`) |
| Styling | Tailwind CSS v4 (`@theme` design tokens, custom utilities, no config file) |
| Animation | Framer Motion 14 (layout animations, `AnimatePresence`, scroll reveals, springs) |
| Routing | React Router 7 (`HashRouter`, URL-driven search params) |
| Icons | lucide-react |
| State | React Context + `useReducer`-style hooks, persisted to `localStorage` |
| Language | TypeScript (strict, path alias `@/*` → `src/*`) |
| Photography | Pexels CDN (hot-linked, no binary assets in the repo) |

---

## Project structure

```
src/
├── App.tsx                     # Router + global providers
├── main.tsx                    # React entry
├── index.css                   # Tailwind v4 theme tokens, utilities, keyframes
├── components/
│   ├── layout/
│   │   ├── Layout.tsx          # Shell: navbar, outlet, footer, compare tray, scroll manager
│   │   ├── Navbar.tsx          # Sticky nav, user menu, mobile drawer, Logo
│   │   ├── Footer.tsx          # Newsletter, sitemap, city marquee, oversized wordmark
│   │   └── CompareBar.tsx      # Floating comparison tray
│   ├── ui/
│   │   ├── Primitives.tsx      # Button, Reveal, SectionHead, Counter, Stars, Tag, Field, EmptyState…
│   │   └── Toasts.tsx          # Notification host
│   ├── PropertyCard.tsx        # Grid + list card variants (wishlist / compare built in)
│   ├── SearchConsole.tsx       # Hero & compact search with live match count
│   ├── FilterPanel.tsx         # Full filter surface + shared Filters type
│   ├── Gallery.tsx             # Property gallery + lightbox
│   ├── MortgageCalculator.tsx  # Affordability tool with donut breakdown
│   └── StylizedMap.tsx         # Site-plan map with live price pins
├── data/
│   ├── properties.ts           # Property type, 24 listings, taxonomies, formatters
│   ├── agents.ts               # Agent type + 6 profiles
│   └── media.ts                # Pexels URL helpers and curated shot pools
├── pages/                      # Home, Listings, PropertyDetail, Compare, Wishlist,
│                               # Agents, AgentDetail, Submit, Auth, NotFound
├── store/AppStore.tsx          # Wishlist / compare / auth / submissions / toasts context
└── utils/cn.ts                 # clsx + tailwind-merge helper
```

**Reusable component architecture:** every surface is composed from the same primitives — `PropertyCard` is used on the landing page, listings (grid *and* list variants), map split-view, wishlist, similar-homes and agent dossiers; `FilterPanel` is rendered once and reused for both the desktop rail and the mobile drawer; `StylizedMap` powers the listings map view, the property location block and the submission pin preview; `Field` + `inputClass` standardise every form in the app.

---

## Data model

```ts
interface Property {
  id: string; title: string; purpose: "buy" | "rent";
  type: "house" | "apartment" | "villa" | "penthouse" | "townhouse" | "loft" | "condo" | "land";
  price: number; beds: number; baths: number; area: number; lot?: number;
  yearBuilt: number; garage: number; floors: number;
  city: string; state: string; neighborhood: string; address: string;
  lat: number; lng: number; pin: { x: number; y: number };
  images: string[]; description: string; highlights: string[]; amenities: string[];
  flags: string[]; agentId: string; listedAt: string;
  views: number; rating: number; reviews: number; hoa?: number;
  energy: string; status: "available" | "under-offer" | "new";
}
```

Helper formatters (`money`, `priceLabel`, `compactMoney`, `areaLabel`) and selectors (`getProperty`, `similarProperties`) live alongside the data so no component re-implements pricing logic.

---

## State &amp; persistence

`src/store/AppStore.tsx` exposes a single `useApp()` context:

| Slice | Persisted | Behaviour |
| --- | --- | --- |
| `user` | ✅ | Sign in / sign up / demo login / sign out, derived display name |
| `wishlist` | ✅ | Toggle, clear, membership test, toast feedback |
| `compare` | ✅ | Toggle with a hard 4-slot cap, per-item removal, clear |
| `viewed` | ✅ | Rolling 12-item "recently viewed" history |
| `submissions` | ✅ | User-created listings, merged into the searchable catalogue |
| `toasts` | — | Ephemeral notification queue with auto-dismiss |

All persistence is namespaced under a single `acre-and-key:v1` key and degrades gracefully (try/catch) when storage is unavailable, e.g. private browsing.

---

## Design system

Defined as Tailwind v4 `@theme` tokens in `src/index.css` — no `tailwind.config.js`.

| Token | Values | Usage |
| --- | --- | --- |
| `ink-*` | `#080c0a → #41564b` | Deep green-black surfaces, type |
| `bone-*` | `#fbfaf6 → #b8b09c` | Warm paper backgrounds |
| `moss-*` | `#8fc5ab → #10301f` | Primary brand, links, success |
| `clay-*` | `#f0a98a → #ab4723` | Wishlist, price cuts, CTAs |
| `gold-*` | `#f0cd85 → #c9932f` | Rentals, ratings, accents |
| Fonts | **Bricolage Grotesque** (display), **Manrope** (body), **DM Mono** (data/labels) | Editorial + technical contrast |
| Textures | `.grain`, `.bg-blueprint`, `.bg-blueprint-dark`, `.bg-hatch` | Layered ambient surfaces |
| Motion | `.animate-kenburns`, `.animate-marquee`, `.animate-ping-slow` | Living background elements |

Radius is deliberately mixed (`rounded-xl` → `rounded-[30px]`) rather than a single blanket rounding, and shadows use two custom tokens (`shadow-card`, `shadow-lift`).

---

## Responsive behaviour &amp; screenshots

Layouts are authored mobile-first with three tuned breakpoints.

| Breakpoint | Target devices | Notable adaptations |
| --- | --- | --- |
| `< 640px` | Phones (360–430 px) | Single-column cards, hamburger → full-screen drawer, slide-in filter panel, compare tray thumbnails scroll horizontally, floating chips collapse |
| `640–1023px` | Tablets (768–834 px) | 2-column card grid, filters stay in a drawer, hero stats reveal, gallery thumbnails 2×2 |
| `≥ 1024px` | Laptops / desktop (1280–1920 px) | 3-column card grid, sticky filter rail, split map view, sticky agent rail, full navbar with user menu |

**Capturing the deliverable screenshots** — open the running app, then in Chrome DevTools:

```
Desktop  → Device toolbar off, 1440 × 900
Tablet   → Device toolbar on, "iPad Air" (820 × 1180) or 834 × 1112
Mobile   → Device toolbar on, "iPhone 14 Pro" (393 × 852) or 390 × 844
```

Capture these seven routes at each size for a complete set: `/`, `/listings`, `/listings` with the filter drawer open (mobile/tablet), `/property/p-03`, `/compare` (add 3 homes first), `/wishlist`, `/agents/maya-ellison` and `/submit` (step 3). Store them under `docs/screenshots/<viewport>/<route>.png`.

---

## Deployment

```bash
npm run build      # → dist/index.html (single self-contained file)
```

Because the router is a `HashRouter` and the bundle is inlined, the build needs **no server rewrites** and can be dropped anywhere:

| Host | Command / action |
| --- | --- |
| Netlify | Drag &amp; drop the `dist` folder, or `netlify deploy --prod --dir=dist` |
| Vercel | `vercel --prod` with build command `npm run build`, output directory `dist` |
| GitHub Pages | Push `dist/` to a `gh-pages` branch (or use `peaceiris/actions-gh-pages`) |
| Any static server | `npx serve dist` |

---

## Credits

- **Photography:** [Pexels](https://www.pexels.com) — hot-linked via CDN, no assets committed to the repo.
- **Icons:** [lucide](https://lucide.dev).
- **Typefaces:** Bricolage Grotesque, Manrope and DM Mono via Google Fonts.
- All listings, agents, reviews, prices and market statistics are **fictional demo data** created for this project.
