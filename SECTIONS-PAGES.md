# CoopLumen — Landing Page Sections & Pages

> **Document type:** Front-end specification (sections & pages only)
> **Status:** Draft for review
> **Scope:** Defines **40 landing-page sections** plus a **separate Pages list (7 routes)**.
> **Explicitly out of scope:** component code, page code, stylesheets, and any implementation of the landing page. Nothing in this document is built yet.
> **Source of truth:** every colour, route, feature and metric below is derived from the existing CoopLumen codebase — `frontend/src/app/globals.css`, `README.md`, `PRD.md`, `docs/architecture.md`, `docs/openapi.yaml`, `SECURITY.md`, `CONTRIBUTING.md`, `.github/FUNDING.yml`.

---

## Table of Contents

1. [Purpose & Scope](#1-purpose--scope)
2. [How to Read This Document](#2-how-to-read-this-document)
3. [Project Context Snapshot](#3-project-context-snapshot)
4. [Colour System](#4-colour-system)
   - [4.1 Base Palettes](#41-base-palettes)
   - [4.2 Semantic Colour Tokens](#42-semantic-colour-tokens)
   - [4.3 Gradient Tokens](#43-gradient-tokens)
   - [4.4 Gradient → Section Mapping](#44-gradient--section-mapping)
   - [4.5 Contrast & Accessibility Rules](#45-contrast--accessibility-rules)
   - [4.6 Implementation Constraints](#46-implementation-constraints)
5. [Navigation Map](#5-navigation-map)
6. [The 40 Landing Page Sections](#6-the-40-landing-page-sections)
7. [Pages (separate list, outside the 40)](#7-pages-separate-list-outside-the-40)
8. [Appendices](#8-appendices)
   - [Appendix A — Existing component reuse map](#appendix-a--existing-component-reuse-map)
   - [Appendix B — Reserved routes beyond the 40](#appendix-b--reserved-routes-beyond-the-40)
   - [Appendix C — External platform inventory](#appendix-c--external-platform-inventory)
   - [Closing note](#closing-note)

---

## 1. Purpose & Scope

This document is a **review artefact**. It lists every section and page the CoopLumen landing page needs, so the composition, wording and colour direction can be agreed **before** any code is written.

It answers three questions:

1. **What are the 40 sections of the landing page?** — [Section 6](#6-the-40-landing-page-sections)
2. **What pages sit behind the landing page?** — [Section 7](#7-pages-separate-list-outside-the-40)
3. **What colours and gradients do those sections use?** — [Section 4](#4-colour-system)

**Not in scope:** React components, `page.tsx` files, `*.module.css` files, copywriting, imagery. This is a specification, not an implementation.

**Blockchain-native requirement:** the landing page must make a visitor _feel_ that they are on the Stellar network — live ledger activity, wallet connection, transaction lifecycles, asset codes, trustlines, explorer deep-links and network state are first-class content, not decoration.

---

## 2. How to Read This Document

Every section entry uses the same fields:

| Field                         | Meaning                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------- |
| **Type**                      | `Global Section` (renders on every route) or `Landing Section` (renders on the landing route only)  |
| **Anchor**                    | The proposed in-page anchor id for scroll-linked navigation                                         |
| **Description**               | What the section says and why it exists                                                             |
| **Content blocks**            | The blocks the section is composed of, top to bottom                                                |
| **CTAs & navigation targets** | Every button or link, and where it navigates (internal route, in-page anchor, or external platform) |
| **Colour treatment**          | Base tokens plus the gradient token from [4.3](#43-gradient-tokens)                                 |

Page entries use: **Page / Route / Description / Rendered sections / Primary CTAs**.

Legend used throughout:

- `▲` external link (opens a blockchain platform or third-party site in a new tab)
- `→` internal navigation (route or in-page anchor)
- 🟣 `primary` gradient · 🟢 `secondary` gradient · 🔵 `info` gradient · 🧊 `surface` gradient

---

## 3. Project Context Snapshot

Grounding facts the sections are built on.

**Stack**

| Layer         | Technology                                       | Version                                               |
| ------------- | ------------------------------------------------ | ----------------------------------------------------- |
| Frontend      | Next.js App Router, React, TypeScript (strict)   | `next ^15.5.25`, `react ^18.3.1`, `typescript ^5.5.3` |
| Styling       | **CSS Modules** + design tokens in `globals.css` | —                                                     |
| Data fetching | SWR                                              | `^2.2.5`                                              |
| Wallet        | `@stellar/freighter-api`                         | `^2.0.0`                                              |
| Forms         | `react-hook-form` + `zod`                        | `^7.86.0`, `^4.5.4`                                   |
| Backend       | Node.js 20, Express 4, TypeScript                | port `4000`                                           |
| Blockchain    | Stellar SDK / Horizon REST                       | `@stellar/stellar-sdk` 12.3                           |
| Database      | PostgreSQL 16                                    | off-chain metadata                                    |

**Existing routes today**

- `/` — renders the `Dashboard` shell (`app/page.tsx`)
- `/communities` — community discovery (`app/communities/page.tsx`)
- `/api/health` — backend health proxy (`app/api/health/route.ts`)

There is **no landing page yet**. This document defines it.

**Product surface (from `PRD.md` and `docs/openapi.yaml`)**

- Community tokens (`build-issue`, `build-burn`, `build-airdrop`, `transfer`)
- Trustlines (`build-trustline`, `/accounts/{publicKey}/trustlines`)
- Multi-signature distributor treasury (Phase 2)
- P2P micro-lending (`/loans`, `/loans/{id}/disburse`, `/repay`, `/default`, `/events`)
- Reputation scoring (`/reputation`, `/reputation/{address}`)
- DAO governance on Soroban (Phase 3)
- Identity / KYC / oracles — SEP-12, World ID, `/prices/xlm` (Phase 4)
- Fees (`/fees/estimate`), balances (`/balances/{publicKey}`), history (`/balances/{publicKey}/history`)

**Roadmap (from `PRD.md` §8)** — Phase 1 Foundation (now) → Phase 2 P2P Lending → Phase 3 DAO Governance → Phase 4 Identity, KYC & Oracles.

**Live components the landing page can reuse** — `Button`, `Card`, `Badge`, `Alert`, `Table`, `Modal`, `Tooltip`, `EmptyState`, `LoadingSkeleton`, `Spinner`, `ProgressBar`, `Pagination`, `QRCode`, `NetworkBadge`, `StellarAddress`, `CopyToClipboard`, `WalletConnect`, `BalancePanel`, `NetworkWarning`, `LoanCard`, `ReputationPanel`, `CommunityCard`, `ThemeToggle`.

---

## 4. Colour System

The palette below is **already in the product**. It is copied verbatim from the raw palette in `frontend/src/app/globals.css` (`--palette-light-*` / `--palette-dark-*`). The landing page must not introduce a competing palette — it extends this one.

### 4.1 Base Palettes

These raw values are declared once per theme and are **never consumed directly**; they are pointed at by the semantic tokens in [4.2](#42-semantic-colour-tokens).

**Light theme**

| Raw token                        | Hex       | Role in the landing page                                                |
| -------------------------------- | --------- | ----------------------------------------------------------------------- |
| `--palette-light-primary`        | `#5B4BD6` | Brand violet — headlines, primary CTAs, start of every "lumen" gradient |
| `--palette-light-secondary`      | `#0A7D63` | Ledger teal — end of every gradient, success, treasury accents          |
| `--palette-light-bg`             | `#EEEEF5` | Page background                                                         |
| `--palette-light-surface`        | `#F8F8FC` | Cards, panels, sticky header                                            |
| `--palette-light-surface-raised` | `#FFFFFF` | Raised cards, modals, code blocks                                       |
| `--palette-light-border`         | `#D8D8E6` | 1px hairlines and dividers                                              |
| `--palette-light-text`           | `#1A1A2E` | Body and heading text                                                   |
| `--palette-light-text-muted`     | `#55556E` | Captions, helper text, section eyebrows                                 |
| `--palette-light-text-inverse`   | `#FFFFFF` | Text on dark or gradient fills                                          |
| `--palette-light-error`          | `#C0392B` | Defaulted loans, failed transactions                                    |
| `--palette-light-success`        | `#0A7D63` | Repaid loans, active network, verified trustline                        |
| `--palette-light-info`           | `#175FBF` | Info banners, governance accents, focus ring                            |
| `--palette-light-warning`        | `#8A6100` | Pending signatures, testnet notices                                     |

**Dark theme**

| Raw token                       | Hex       | Role in the landing page                           |
| ------------------------------- | --------- | -------------------------------------------------- |
| `--palette-dark-primary`        | `#6C5CE7` | Brand violet, brightened for dark surfaces         |
| `--palette-dark-secondary`      | `#00B894` | Ledger teal, neon — heavy gradient and accent work |
| `--palette-dark-bg`             | `#0F0F1A` | Page background — the "night ledger" canvas        |
| `--palette-dark-surface`        | `#1A1A2E` | Cards and sections                                 |
| `--palette-dark-surface-raised` | `#23233D` | Raised cards, code blocks, sticky header           |
| `--palette-dark-border`         | `#2D2D44` | Hairlines                                          |
| `--palette-dark-text`           | `#E0E0F0` | Body and heading text                              |
| `--palette-dark-text-muted`     | `#8888AA` | Secondary text                                     |
| `--palette-dark-text-inverse`   | `#0F0F1A` | Text on bright fills — see the warning in 4.5      |
| `--palette-dark-error`          | `#E17055` | Failures, defaults                                 |
| `--palette-dark-success`        | `#00B894` | Success states                                     |
| `--palette-dark-info`           | `#74B9FF` | Info, governance, focus ring                       |
| `--palette-dark-warning`        | `#FDCB6E` | Warnings, pending states                           |

**The chromatic spine.** Violet → teal is the brand axis: `#5B4BD6 → #0A7D63` in light, `#6C5CE7 → #00B894` in dark. Every gradient in [4.3](#43-gradient-tokens) travels along that axis. It reads as "ledger / network / crypto" without importing an off-brand cliché such as bitcoin orange or ethereum blue.

### 4.2 Semantic Colour Tokens

Components read these, never the raw palette. Any landing-page CSS must consume these names.

| Token                    | Light resolves to | Dark resolves to |
| ------------------------ | ----------------- | ---------------- |
| `--color-primary`        | `#5B4BD6`         | `#6C5CE7`        |
| `--color-secondary`      | `#0A7D63`         | `#00B894`        |
| `--color-bg`             | `#EEEEF5`         | `#0F0F1A`        |
| `--color-surface`        | `#F8F8FC`         | `#1A1A2E`        |
| `--color-surface-raised` | `#FFFFFF`         | `#23233D`        |
| `--color-border`         | `#D8D8E6`         | `#2D2D44`        |
| `--color-text`           | `#1A1A2E`         | `#E0E0F0`        |
| `--color-text-muted`     | `#55556E`         | `#8888AA`        |
| `--color-text-inverse`   | `#FFFFFF`         | `#0F0F1A`        |
| `--color-error`          | `#C0392B`         | `#E17055`        |
| `--color-success`        | `#0A7D63`         | `#00B894`        |
| `--color-info`           | `#175FBF`         | `#74B9FF`        |
| `--color-warning`        | `#8A6100`         | `#FDCB6E`        |

Derived tokens already declared and available to gradients and states: `--color-primary-hover`, `--color-primary-active`, `--color-primary-subtle`, `--color-secondary-hover`, `--color-secondary-active`, `--color-secondary-subtle`, `--color-surface-hover`, `--color-border-strong`, `--color-text-on-primary`, `--color-error-subtle`, `--color-success-subtle`, `--color-info-subtle`, `--color-warning-subtle`, `--color-skeleton-base`, `--color-skeleton-highlight`, `--color-focus-ring`.

Also available for layout: `--space-0 … --space-24` (4px base), `--radius-none/sm/md/lg/xl/pill/circle` (with `--radius` aliasing `--radius-md`), breakpoints `--breakpoint-sm 640px`, `--breakpoint-md 768px`, `--breakpoint-lg 1024px`, `--breakpoint-xl 1280px`, and `--container-max-xl 1200px`.

### 4.3 Gradient Tokens

The codebase currently contains **no gradients** (only a `linear-gradient` shimmer inside `LoadingSkeleton.module.css`). The landing page introduces a new, self-contained `--gradient-*` family.

Design rule: gradients are declared **once, theme-agnostically**, and built from the _semantic_ tokens with `color-mix(in srgb, …)`. Because those tokens already flip with the theme, light and dark need **no duplicated gradient declarations**.

```css
:root,
:root.light,
:root.dark {
  /* Hero: violet core, bending through a violet/teal blend, into ledger teal. */
  --gradient-hero: linear-gradient(
    135deg,
    color-mix(in srgb, var(--color-primary) 86%, #000) 0%,
    var(--color-primary) 44%,
    color-mix(in srgb, var(--color-primary) 46%, var(--color-secondary)) 74%,
    var(--color-secondary) 100%
  );

  /* Treasury: teal into violet — funds flowing back into the community. */
  --gradient-treasury: linear-gradient(140deg, var(--color-secondary), var(--color-primary));

  /* Governance: violet into info blue — proposals, votes, snapshots. */
  --gradient-governance: linear-gradient(120deg, var(--color-primary), var(--color-info));

  /* Ledger: a translucent sweep used as a shimmer strip behind live data. */
  --gradient-ledger: linear-gradient(
    90deg,
    transparent 0%,
    var(--color-primary-subtle) 25%,
    var(--color-secondary-subtle) 50%,
    var(--color-primary-subtle) 75%,
    transparent 100%
  );

  /* Surface: the default, barely-there section wash. */
  --gradient-surface: linear-gradient(180deg, var(--color-surface-raised), var(--color-surface));

  /* Token: soft violet/teal tint for asset, trustline and balance cards. */
  --gradient-token: linear-gradient(
    135deg,
    var(--color-primary-subtle),
    var(--color-secondary-subtle)
  );

  /* Trust: success into info tint — security, audits, escrow. */
  --gradient-trust: linear-gradient(120deg, var(--color-success-subtle), var(--color-info-subtle));

  /* CTA: the primary band/button fill, with a hover step further along the axis. */
  --gradient-cta: linear-gradient(
    100deg,
    var(--color-primary),
    color-mix(in srgb, var(--color-primary) 40%, var(--color-secondary))
  );
  --gradient-cta-hover: linear-gradient(
    100deg,
    color-mix(in srgb, var(--color-primary) 60%, var(--color-secondary)),
    var(--color-secondary)
  );

  /* Aurora: ambient light leaking in from two corners over the page canvas. */
  --gradient-aurora:
    radial-gradient(60% 80% at 12% 0%, var(--color-primary-subtle), transparent 60%),
    radial-gradient(50% 70% at 88% 10%, var(--color-secondary-subtle), transparent 65%),
    var(--color-bg);

  /* Mesh: three-point ambient field for the developer/ecosystem section. */
  --gradient-mesh:
    radial-gradient(45% 60% at 20% 20%, var(--color-primary-subtle), transparent 70%),
    radial-gradient(40% 55% at 80% 30%, var(--color-info-subtle), transparent 70%),
    radial-gradient(50% 60% at 55% 95%, var(--color-secondary-subtle), transparent 72%),
    var(--color-surface);

  /* Seam: a 1px "blockchain seam" divider between major sections. */
  --gradient-seam: linear-gradient(
    90deg,
    transparent 0%,
    var(--color-primary) 30%,
    var(--color-secondary) 70%,
    transparent 100%
  );

  /* Gradient text, used with background-clip: text on hero and stat numbers. */
  --gradient-text-hero: linear-gradient(90deg, var(--color-primary), var(--color-secondary));
}
```

**Usage helper patterns** — shown only to fix intent, not to be built now:

```css
.gradient-text {
  background-image: var(--gradient-text-hero);
  background-clip: text;
  color: transparent;
}

.section-seam {
  height: 1px;
  background-image: var(--gradient-seam);
}

/* Gradient border without extra DOM: paint the gradient, inset the surface. */
.gradient-border {
  border: 1px solid transparent;
  background:
    linear-gradient(var(--color-surface), var(--color-surface)) padding-box,
    var(--gradient-cta) border-box;
}
```

### 4.4 Gradient → Section Mapping

| Gradient token                               | Sections that use it                                                                                                              | Effect                                                      |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| 🟣 `--gradient-hero`                         | 5 Hero, 7 Wallet Connect Hero Panel, 39 Final CTA Band                                                                            | Full-bleed violet→teal wash, white/dark-inverse text on top |
| 🟣 `--gradient-cta` / `--gradient-cta-hover` | 5 Hero primary CTA, 9 Three-Pillar CTAs, 34 Pricing, 37 Newsletter, 39 Final CTA Band, 40 Contact Us submit                       | Button and band fills; hover slides further toward teal     |
| `--gradient-ledger`                          | 3 Network Status Bar, 6 Live Network Metrics Strip, 24 Live On-Chain Activity Feed                                                | Translucent animated sweep behind live figures              |
| 🟢 `--gradient-treasury`                     | 17 Shared Multi-Signature Treasury, 18 Batch Disbursement & Airdrop, 35 Community & Support Hub                                   | Teal→violet, "funds coming home"                            |
| 🔵 `--gradient-governance`                   | 21 DAO Governance & Soroban Proposals, 31 Public Roadmap Timeline (Phase 3 marker)                                                | Violet→info blue, deliberate and civic                      |
| 🧊 `--gradient-aurora`                       | 10 Financial Exclusion Problem, 11 How CoopLumen Works, 13 Comparison Matrix, 30 Community Stories, 31 Roadmap, 38 Blog/Changelog | Ambient corner light over the page canvas                   |
| 🧊 `--gradient-surface`                      | All default sections not listed above (12, 14–16, 19, 20, 22, 23, 25–29, 32, 33, 36)                                              | Near-invisible raised→flat wash for depth without noise     |
| `--gradient-token`                           | 15 Community Token Issuance, 16 Trustline Onboarding, 26 Asset & Trustline Explorer, 20 Reputation & Credit Signals               | Soft violet/teal tint for asset-centric cards               |
| 🟩 `--gradient-trust`                        | 33 Security, Transparency & Open Source, 22 Identity/KYC/Oracles                                                                  | Success→info tint for reassurance                           |
| 🧊 `--gradient-mesh`                         | 11 How CoopLumen Works (developer variant), 14 Architecture Transparency, 33 (code side)                                          | Three-point ambient field for technical content             |
| 〰️ `--gradient-seam`                         | Between every major section group (A→B→C→D→E→F→G)                                                                                 | 1px violet→teal hairline; the "blockchain seam"             |
| 🔤 `--gradient-text-hero`                    | 5 Hero headline, 6 Metrics Strip numbers, 32 Impact Metrics                                                                       | Text-clipped gradient for headline emphasis                 |

**Rules of restraint**

1. Never stack two gradient families inside one viewport band — a gradient section is always followed by a flat `--color-surface` or `--color-bg` section.
2. Decorative gradients (`ledger`, `seam`, `aurora`, `mesh`) stay under ~18% alpha via the `-subtle` tokens; only `hero`, `treasury`, `governance` and `cta` are fully saturated.
3. Body copy never sits on a saturated gradient; it always sits on `--color-surface` / `--color-bg`.

### 4.5 Contrast & Accessibility Rules

| Foreground | Background                  | Ratio    | Verdict                                  |
| ---------- | --------------------------- | -------- | ---------------------------------------- |
| `#FFFFFF`  | `#5B4BD6` (light primary)   | ≈ 6.2:1  | ✅ AA for all text                       |
| `#FFFFFF`  | `#0A7D63` (light secondary) | ≈ 5.2:1  | ✅ AA for all text                       |
| `#FFFFFF`  | `#6C5CE7` (dark primary)    | ≈ 4.7:1  | ✅ AA normal, ✅ AAA large               |
| `#FFFFFF`  | `#00B894` (dark secondary)  | ≈ 2.6:1  | ❌ **fails** — must not carry white text |
| `#0F0F1A`  | `#00B894` (dark secondary)  | ≈ 8.0:1  | ✅ use `--color-text-inverse` instead    |
| `#E0E0F0`  | `#0F0F1A` (dark bg)         | ≈ 14.6:1 | ✅ AAA                                   |
| `#1A1A2E`  | `#EEEEF5` (light bg)        | ≈ 15.4:1 | ✅ AAA                                   |
| `#8888AA`  | `#0F0F1A` (dark bg)         | ≈ 6.6:1  | ✅ AA                                    |

**Binding rules**

1. On the **dark** theme, the teal end of `--gradient-hero` / `--gradient-treasury` / `--gradient-cta` must use `var(--color-text-inverse)` (`#0F0F1A`), or teal must be confined to accents, icons, seams and dividers. Never put white text over `#00B894`.
2. Because `--gradient-hero` and `--gradient-cta` blend violet with teal, text laid over them must be checked at **both ends** and at the blend midpoint, not just at the CSS start colour.
3. Focus is a token, never a per-section choice: `outline: var(--focus-ring-width) solid var(--color-focus-ring)` with `outline-offset: var(--focus-ring-offset)`, already set globally on `:focus-visible`.
4. Status meaning is never carried by gradient alone — pair it with a `Badge` variant (`success` / `warning` / `error` / `info` / `neutral`) plus visible text.
5. All motion (the `ledger` shimmer, the transaction-lifecycle animation in section 23, hover gradients) must respect `prefers-reduced-motion: reduce`, matching `Spinner` and `LoadingSkeleton`.
6. Both themes must be complete: every section is reviewed in light **and** dark before sign-off, because `ThemeToggle` and `THEME_INIT_SCRIPT` give users a real, explicit choice.

### 4.6 Implementation Constraints

These come from the existing codebase and tests; the landing page must not break them.

1. **CSS Modules + tokens only.** Styling in this repo is `Component.module.css` consuming `globals.css` custom properties (per `docs/architecture.md`). **Tailwind is not installed** — no `tailwind.config.*`, no `tailwindcss` dependency, no PostCSS config. The utility class names currently in `app/communities/page.tsx` (`bg-gray-50`, `dark:bg-[#0a0a0a]`, `text-3xl`) resolve to nothing; the landing page must not follow that pattern.
2. **Token families.** `components/__tests__/designTokens.test.ts` asserts the existence of the `--color-*`, `--space-*` and `--radius-*` families, that every status colour has a `-subtle` companion, and that every `var(--x)` **without a fallback** is declared. New `--gradient-*` tokens use their own prefix so they cannot collide, but each must be declared in `globals.css` before use.
3. **Breakpoints.** The same test asserts that **every** `@media (min-width: Npx)` uses exactly `640`, `768`, `1024` or `1280`. Responsive rules must use those steps and nothing else.
4. **Spacing scale.** Only `--space-0 … --space-24`; no arbitrary rem values, so vertical rhythm stays on the 4px grid.
5. **Naming.** One component per file, PascalCase `.tsx`, matching `.module.css`, tests in a sibling `__tests__/` directory — the convention used throughout `frontend/src/components`.
6. **Accessibility baseline.** Semantic landmarks (`header`, `nav`, `main`, `section`, `footer`), one `h1` per page, `aria-label` on icon-only controls, decorative graphics `aria-hidden`, and `NetworkBadge`-style `role="status"` for live-updating figures.

---

## 5. Navigation Map

**Primary header links** (`→` internal, `▲` external)

| Label             | Destination                                    | Notes                                                                 |
| ----------------- | ---------------------------------------------- | --------------------------------------------------------------------- |
| Product           | `→ #three-pillar-value-proposition`            | Scrolls to the landing pillars; sub-menu items jump to sections 15–22 |
| Communities       | `→ /communities`                               | Discover Communities page (P1)                                        |
| Developers        | `→ /developers`                                | Developer & API page (P4)                                             |
| Roadmap           | `→ #public-roadmap-timeline`                   | Landing section 31                                                    |
| Pricing           | `→ #pricing-community-plans`                   | Landing section 34 (and reserved `/pricing`)                          |
| Connect Freighter | `→` `useWallet().connect()`                    | Opens the Freighter extension; a failure surfaces `NetworkWarning`    |
| GitHub            | `▲ https://github.com/Tybravo/CoopLumen-Index` | Repository, from the workspace remote                                 |
| Docs              | `▲ https://developers.stellar.org`             | Stellar developer documentation                                       |

**In-page flow (the order a visitor is walked through)**

```
3  Network Status Bar
   ↓
5  Hero → 7 Wallet Connect → 6 Live Metrics         (grab attention, prove it is live)
   ↓
10 Problem → 11 How It Works → 12 Why Stellar → 13 Comparison → 14 Architecture
   ↓
15–22  Eight core capability sections
   ↓
23 Transaction Lifecycle → 24 Activity Feed → 25 Discovery → 26 Asset Explorer
   → 27 Fee Calculator → 28 Network Health
   ↓
29 Personas → 30 Stories → 31 Roadmap → 32 Impact → 33 Security
   ↓
34 Pricing → 35 Support → 36 Contribute → 37 Newsletter
   ↓
38 Changelog → 39 Final CTA → 40 Contact Us
   ↓
4  Footer
```

**Key cross-links**

- `CommunityCard`'s link overlay `→ /communities/{id}` — the card already links this way today.
- Hero primary CTA `→ /` (the Dashboard) once a wallet is connected, otherwise `→ #wallet-connect-hero-panel`.
- Every transaction hash, asset code and Stellar address on the landing page deep-links `▲` to Stellar Expert / Horizon for the configured network.
- Footer legal and content links `→ /privacy`, `→ /terms`, `→ /status`, `→ /blog`, `→ /changelog`.
- Contribution CTAs `→ /about#contributors` and `▲` GitHub issues / Discussions.

**External blockchain platforms referenced by the landing page**

| Platform                                 | Purpose                            | Appears in sections  |
| ---------------------------------------- | ---------------------------------- | -------------------- |
| Stellar Expert (`stellar.expert`)        | Ledger, account and asset explorer | 3, 23, 24, 26, 35    |
| Horizon (`horizon-testnet.stellar.org`)  | The API the app actually talks to  | 3, 12, 14, 23, 28    |
| `stellar.org` / `developers.stellar.org` | Protocol docs, Soroban             | 8, 12, 14, 21, 36    |
| Freighter (`freighter.app`)              | Wallet extension                   | 2, 5, 7, 12, 35      |
| `@stellar/stellar-sdk` (npm)             | SDK, pinned at 12.3                | 8, 14, 37            |
| Stellar Developers Discord               | Community help                     | 35, 40               |
| GitHub repo + Discussions                | Source, issues, discussion         | 2, 4, 31, 36, 38, 40 |
| Open Collective (`cooplumen`)            | Funding                            | 4, 36                |

---

## 6. The 40 Landing Page Sections

Groups: **A** Global Chrome (1–4) · **B** Hero & First Impression (5–9) · **C** Problem → Solution (10–14) · **D** Core Product Features (15–22) · **E** Blockchain-Native Experience (23–28) · **F** Proof, Trust & Roadmap (29–33) · **G** Conversion, Community & Content (34–40).

### Group A — Global Chrome

#### 1. Header Navigation Bar

- **Type:** Global Section (renders on every route)
- **Anchor:** `#header-navigation-bar`
- **Description:** The always-present orientation layer. It reuses the sticky-header pattern already in `Dashboard.module.css` (sticky, `z-index: 10`, `border-bottom: var(--color-border)`, `background: var(--color-surface)`) and carries the ◆ mark, the `CoopLumen` wordmark and the "Decentralized Community Finance" tagline from `Dashboard.tsx`. Its job is to make the Stellar context visible within one second of arrival: the network pill and the wallet button live in the bar itself, not buried in a menu.
- **Content blocks:**
  1. Left: ◆ logo (gradient-clipped) + wordmark + tagline (tagline hidden below `sm`, as today).
  2. Centre: primary nav — `Product`, `Communities`, `Developers`, `Roadmap`, `Pricing`.
  3. Right: `NetworkBadge` (TESTNET/MAINNET), `ThemeToggle`, **Connect Freighter** `Button` that swaps to the connected state (short key + XLM balance chip) exactly like `WalletConnect`.
  4. Mobile trigger opening section 2.
- **CTAs & navigation targets:** Product `→ #three-pillar-value-proposition`; Communities `→ /communities`; Developers `→ /developers`; Roadmap `→ #public-roadmap-timeline`; Pricing `→ #pricing-community-plans`; Connect Freighter `→` wallet `connect()`; wordmark `→ /`.
- **Colour treatment:** `--color-surface` fill, `--color-border` hairline, violet logo. On scroll, a 1px `--gradient-seam` appears along the lower edge.

#### 2. Mobile Navigation Drawer

- **Type:** Global Section
- **Anchor:** `#mobile-navigation-drawer`
- **Description:** Navigation below the `lg` breakpoint. A slide-over panel that reuses the `Modal` conventions (`--color-overlay`, focus trap, `Esc` to close, `--z-index-modal`) so keyboard and screen-reader behaviour matches the rest of the product rather than inventing a second pattern.
- **Content blocks:**
  1. Close button + ◆ wordmark.
  2. Nav list with an expandable `Product` group linking to sections 15–22 by anchor.
  3. Network + theme row.
  4. Pinned bottom block: **Connect Freighter** CTA and a short "Testnet only — no real funds move" `Alert`.
- **CTAs & navigation targets:** every header destination; `→ #wallet-connect-hero-panel`; `▲ https://github.com/Tybravo/CoopLumen-Index`.
- **Colour treatment:** `--color-surface-raised` panel over `--color-overlay`; the slide animates with `--theme-transition-duration` and is disabled under reduced motion.

#### 3. Network Status Bar

- **Type:** Global Section
- **Anchor:** `#network-status-bar`
- **Description:** A thin strip directly beneath the header that answers "is this real, and is it live?". It reads genuine network state — the configured network from `NEXT_PUBLIC_STELLAR_NETWORK`, Horizon reachability, latest ledger close and the close cadence Stellar is known for (~3–5s). It is the smallest possible proof that CoopLumen is not a mock-up.
- **Content blocks:**
  1. `NetworkBadge`-style pill with a pulsing indicator dot.
  2. "Latest ledger closed 2s ago" ticking counter.
  3. Horizon status as a polite `role="status"` live region.
  4. Average fee fraction + an "≈4s finality" note.
  5. Deep-link to Stellar Expert for the current network.
- **CTAs & navigation targets:** `▲ stellar.expert` (testnet/mainnet, new tab); `→ #fee-remittance-calculator`.
- **Colour treatment:** `--color-surface` base with a `--gradient-ledger` sweep; the indicator uses `--color-success` when healthy and `--color-warning` when degraded.

#### 4. Site Footer

- **Type:** Global Section
- **Anchor:** `#site-footer`
- **Description:** The closing orientation layer, and the home of every route that does not deserve header space. It also carries the licence and "built on Stellar" statements that make the project's open-source and blockchain commitments explicit.
- **Content blocks:**
  1. Brand column: ◆ mark, one-line mission, `NetworkBadge`, MIT licence note.
  2. **Product**: Issue tokens `→ #community-token-issuance`, Treasury `→ #shared-multisig-treasury`, Lending `→ #p2p-micro-lending-pools`, Governance `→ #dao-governance-soroban`, Communities `→ /communities`.
  3. **Developers**: API reference `→ /developers`, Architecture `→ #architecture-transparency`, Roadmap `→ #public-roadmap-timeline`, Changelog `→ #blog-changelog-preview`, Self-hosting `→ /developers`.
  4. **Community**: GitHub `▲`, Discord `▲`, Stellar Developers Discord `▲`, Open Collective `▲`, Contribute `→ #contribution-bounty-spotlight`.
  5. **Legal**: Privacy `→ /privacy`, Terms `→ /terms`, Security `→ /security`, Status `→ /status`.
  6. Bottom bar: © CoopLumen Contributors, "Built on Stellar", repeat theme toggle.
- **CTAs & navigation targets:** all of the above; newsletter `→ #newsletter-testnet-waitlist`.
- **Colour treatment:** `--color-bg` base with a `--gradient-seam` hairline above; column headings in `--color-text-muted`; link hover shifts to `--color-primary`.

### Group B — Hero & First Impression

#### 5. Hero Section

- **Type:** Landing Section
- **Anchor:** `#hero-section`
- **Description:** The single most important viewport on the site. It states what CoopLumen is, who it is for, and what network it runs on — then immediately offers two exits: launch the app, or read the docs. It must feel like a blockchain product from the first line: an asset code, a short Stellar address, a live balance chip and the word "testnet" are visible above the fold.
- **Content blocks:**
  1. Eyebrow pill: `NetworkBadge` + "Open source · Testnet".
  2. `h1` with gradient-clipped text: "Community finance, settled on Stellar."
  3. Sub-headline: any cooperative, NGO, diaspora or savings circle can issue a token, run a lending pool and govern a shared treasury — no bank in the middle.
  4. Dual CTA row: **Launch on Testnet** (primary) and **Read the Docs** (secondary).
  5. Live wallet chip: connected short key `GABC…WXYZ`, XLM balance and community-token balance, mirroring `WalletConnect` + `BalancePanel`.
  6. A small `StellarAddress` + `CopyToClipboard` sample showing a real `[CODE]/[ISSUER]` pair for flavour.
- **CTAs & navigation targets:** **Launch on Testnet** `→ /` (or `→ #wallet-connect-hero-panel` when no wallet is connected); **Read the Docs** `→ /developers`; asset code `▲` Stellar Expert; "How it works" text link `→ #how-cooplumen-works`.
- **Colour treatment:** 🟣 `--gradient-hero` full-bleed; headline text on the gradient; `--gradient-text-hero` for the emphasis word; CTA fill `--gradient-cta` with `--gradient-cta-hover`. Teal side of the gradient carries no white text in dark mode.

#### 6. Live Network Metrics Strip

- **Type:** Landing Section
- **Anchor:** `#live-network-metrics-strip`
- **Description:** Converts the hero's promise into evidence. A horizontal band of real, polled numbers — communities registered, active wallets, on-chain transaction volume, average fee, settlement time — matching the Phase 1 / Phase 2 targets already published in `PRD.md` §9. Numbers animate up on first view and refresh on an interval.
- **Content blocks:**
  1. Five metric tiles: Communities, Active wallets, On-chain transactions, Average fee, Finality.
  2. Target-vs-current micro-labels (e.g. "10 → 100 communities").
  3. `role="status"` region so updates are announced politely.
  4. "as of <timestamp>" caption with a manual refresh control.
  5. `LoadingSkeleton` placeholders while the first request is in flight.
- **CTAs & navigation targets:** "See all communities" `→ /communities`; "How we measure" `→ #impact-metrics`; tile deep-links `▲` Stellar Expert.
- **Colour treatment:** `--color-surface` base with a slow `--gradient-ledger` sweep behind the numbers; figures use `--gradient-text-hero`; tiles use `--radius-lg` and `--color-border`.

#### 7. Wallet Connect Hero Panel

- **Type:** Landing Section
- **Anchor:** `#wallet-connect-hero-panel`
- **Description:** The blockchain moment. Instead of describing a wallet, the page lets the visitor connect Freighter and see their own Stellar account — balances, network and trustlines — before they sign up for anything. This is the section that makes the landing page feel like an application rather than a brochure.
- **Content blocks:**
  1. Illustrated 3-step onboarding: 1) Install Freighter `▲`, 2) Connect, 3) Establish a trustline.
  2. The real `WalletConnect` component embedded, including the connected state (short key, network, XLM balance, Disconnect).
  3. `NetworkWarning` when the extension's network differs from `NEXT_PUBLIC_STELLAR_NETWORK`.
  4. `QRCode` fallback for desktop-to-mobile hand-off.
  5. A reassurance `Alert`: no private keys ever reach the server (`PRD.md` §6).
- **CTAs & navigation targets:** "Install Freighter" `▲ https://freighter.app`; **Connect Freighter** `→ connect()`; "Not on testnet?" `▲` Horizon docs; "Skip for now" `→ #three-pillar-value-proposition`.
- **Colour treatment:** 🟣 `--gradient-token` tinted card on a `--gradient-aurora` backdrop; connected-state badges use `--color-success` / `--color-info`; borders via the gradient-border helper.

#### 8. Built-on-Stellar Trust Bar

- **Type:** Landing Section
- **Anchor:** `#built-on-stellar-trust-bar`
- **Description:** A short credibility strip naming the exact technology the project is built on, so a technical visitor can immediately place CoopLumen in the ecosystem. It replaces the usual "as seen in" logo parade with the actual stack, all of which is verifiable in `README.md` and `docs/architecture.md`.
- **Content blocks:**
  1. Six marks: Stellar Network, Horizon API, Soroban, Freighter, `@stellar/stellar-sdk` 12.3, PostgreSQL 16.
  2. One-line caption per mark (e.g. "Horizon — the REST API every balance on this page came from").
  3. Optional "Next.js 15 + Express 4" line for the application layer.
- **CTAs & navigation targets:** Stellar `▲ https://stellar.org`; Horizon `▲` testnet docs; Soroban `▲` Soroban docs; Freighter `▲ https://freighter.app`; SDK `▲` npm; architecture `→ #architecture-transparency`.
- **Colour treatment:** 🧊 `--color-surface` with `--color-border` separators; marks sit at `--color-text-muted` and lift to `--color-text` on hover; a `--gradient-seam` tops and tails the strip.

#### 9. Three-Pillar Value Proposition

- **Type:** Landing Section
- **Anchor:** `#three-pillar-value-proposition`
- **Description:** The map of the product in one screen: **Issue** (community tokens), **Lend** (peer-to-peer pools) and **Govern** (shared treasuries). This mirrors the three claims in the README's opening sentence and gives the eight feature sections that follow a clear parent. Each pillar card links down to its feature cluster.
- **Content blocks:**
  1. Three pillar cards, each with icon, one-line promise and two supporting bullets.
  2. **Issue** — custom asset `<CODE>/<ISSUER>`, trustlines, batch airdrop.
  3. **Lend** — micro-loans in community tokens or XLM, on-chain repayment, reputation.
  4. **Govern** — multi-sig treasury now, Soroban proposals next.
  5. A footer line linking the pillars to the roadmap phases.
- **CTAs & navigation targets:** Issue `→ #community-token-issuance`; Lend `→ #p2p-micro-lending-pools`; Govern `→ #dao-governance-soroban`; "See the full roadmap" `→ #public-roadmap-timeline`.
- **Colour treatment:** three cards using `--gradient-token` for Issue, `--gradient-treasury` at low opacity for Lend, `--gradient-governance` at low opacity for Govern — the only place where three gradients appear side by side, kept subtle so they read as one family.

### Group C — Problem → Solution Narrative

#### 10. The Financial Exclusion Problem

- **Type:** Landing Section
- **Anchor:** `#financial-exclusion-problem`
- **Description:** The "why" of the project, lifted from `PRD.md` §2: 1.4 billion unbanked adults, informal savings circles managing billions with no audit trail, NGOs paying 3–7% in wire fees with 3–5 day settlement, and general-purpose DeFi that is too expensive or too volatile for a village savings group. It earns the right to the solution sections that follow.
- **Content blocks:**
  1. Three large statistics with sources (World Bank 2022 for the 1.4bn figure).
  2. A four-row problem table: local savings circles, diaspora communities, NGOs, co-ops — with their specific pain (no digital record, remittance fees, slow grant disbursement, manual governance).
  3. A short narrative paragraph naming ROSCAs, SACCOs and tontines explicitly.
  4. A transition line into section 11.
- **CTAs & navigation targets:** "Read the problem statement" `→ /about`; "See how it changes" `→ #how-cooplumen-works`; World Bank citation `▲`.
- **Colour treatment:** 🧊 `--gradient-aurora` over `--color-bg`; statistics in `--color-text`; the problem table uses `--color-error-subtle` accents (not saturated red) to signal friction without alarm.

#### 11. How CoopLumen Works

- **Type:** Landing Section
- **Anchor:** `#how-cooplumen-works`
- **Description:** The four-step journey from "no wallet" to "community treasury running", written so a non-technical treasurer can follow it. Each step names the on-chain artefact it produces, because that is the difference between CoopLumen and a spreadsheet.
- **Content blocks:**
  1. Step 1 — **Connect**: install Freighter `▲` and link the public key. Produces: a `G…` address.
  2. Step 2 — **Register the community**: name, description, asset code (e.g. `ECOLGS`) and issuer key. Produces: a community record + an issued asset.
  3. Step 3 — **Establish trustlines**: members sign a trustline so they can hold the token. Produces: a `[CODE]/[ISSUER]` balance of `0`.
  4. Step 4 — **Transact & govern**: disburse, lend, repay, propose. Produces: signed transactions on Stellar.
  5. A connected time-line showing "≈4 seconds" beside each on-chain step.
  6. A "what goes on-chain vs what stays off-chain" two-column note (Stellar vs PostgreSQL).
- **CTAs & navigation targets:** "Install Freighter" `▲ https://freighter.app`; "See the API" `→ /developers`; "Understand the trustline" `→ #trustline-onboarding`; "Open the dashboard" `→ /`.
- **Colour treatment:** 🧊 `--gradient-mesh` behind a tokenised step rail; step numbers on `--gradient-cta` pills; connector lines use `--gradient-seam`.

#### 12. Why Stellar

- **Type:** Landing Section
- **Anchor:** `#why-stellar`
- **Description:** Justifies the blockchain choice with the four reasons already stated in `README.md`: 3–5 second finality, fees of fractions of a cent, native asset primitives (custom assets, trustlines, multi-sig), and a Soroban path to on-chain governance — plus a mature browser wallet in Freighter. This is where a sceptical visitor stops wondering "why not Ethereum?".
- **Content blocks:**
  1. Four reason cards: **Speed & cost**, **Native asset primitives**, **Soroban smart contracts**, **Freighter wallet**.
  2. A side-by-side cost/time comparison strip: Stellar vs a 3–7% wire with 3–5 day settlement.
  3. One honest limitation note: community keys and custodian policy are still an open question (`PRD.md` §11).
- **CTAs & navigation targets:** "Stellar docs" `▲ https://developers.stellar.org`; "Soroban" `▲` Soroban docs; "Horizon API" `▲` Horizon docs; "Read the architecture" `→ #architecture-transparency`; "Estimate your fee" `→ #fee-remittance-calculator`.
- **Colour treatment:** 🧊 `--gradient-surface`; the four cards take `--color-primary`, `--color-secondary`, `--color-info` and `--color-warning` as icon accents on `-subtle` tints; the comparison strip uses `--color-success-subtle`.

#### 13. Comparison Matrix

- **Type:** Landing Section
- **Anchor:** `#comparison-matrix`
- **Description:** A plain, unemotional table letting a visitor compare CoopLumen against the four alternatives a community actually has: cash and paper ledgers, a bank account, an international wire / remittance corridor, and a general-purpose DeFi protocol. It uses the existing `Table` component so the landing page reuses product behaviour rather than inventing a new grid.
- **Content blocks:**
  1. `Table` with sortable columns: Settlement time, Cost, Audit trail, Group governance, Wallet requirement, Volatility of unit.
  2. Rows: Cash/paper ledger, Bank account, Wire/remittance, Generic DeFi, **CoopLumen (highlighted row)**.
  3. Footnote tying the claims to `PRD.md` sources.
- **CTAs & navigation targets:** "How we reach the numbers" `→ /about`; "Try it on testnet" `→ /`; "No crypto experience needed" `→ #faq` (FAQ page P6).
- **Colour treatment:** 🧊 `--gradient-aurora` section background; the CoopLumen row gets a `--gradient-token` wash and a `--color-primary` left border; other rows stay neutral.

#### 14. Architecture Transparency

- **Type:** Landing Section
- **Anchor:** `#architecture-transparency`
- **Description:** The full system diagram from `README.md` rendered as a real part of the page, not buried in docs. Publishing the pipeline — browser/PWA → Express API → Stellar SDK → Horizon → Stellar Network, with PostgreSQL alongside — is itself the trust argument: a visitor can see exactly where their key is and where it is not.
- **Content blocks:**
  1. Four-layer diagram: Browser/PWA (Next.js 15 + Freighter) → Node/Express API (port 4000) → Stellar SDK → Stellar Network (Horizon, assets, trustlines) with PostgreSQL off to the side.
  2. Annotated labels on each connection: "REST/JSON", "Horizon REST", "private keys never leave the browser".
  3. `OpenAPI` spec badge with an endpoint count and a link to the API page.
  4. "What the backend can and cannot do" bullet pair.
- **CTAs & navigation targets:** "Read `docs/architecture.md`" `▲` GitHub; "API reference" `→ /developers`; "OpenAPI spec" `→ /developers#openapi`; "Security policy" `→ /security`.
- **Colour treatment:** 🧊 `--gradient-mesh` on `--color-surface`; nodes as `--color-surface-raised` cards with `--gradient-seam` connectors; the Stellar layer node carries the `--gradient-hero` accent to mark the boundary of the public network.

### Group D — Core Product Features

#### 15. Community Token Issuance

- **Type:** Landing Section
- **Anchor:** `#community-token-issuance`
- **Description:** The flagship capability: any group issues its own Stellar asset — `ECOLGS` for Eco Lagos, `CO1` for a co-op — and the initial supply lands in a community distributor account. This section shows the exact artefact produced, `<CODE>/<ISSUER>`, because that string is what a member sees in their wallet forever after. Backed by `POST /api/v1/tokens/build-issue` and `build-burn`.
- **Content blocks:**
  1. Card mock-up: asset code field, issuer public key, initial supply, distributor address.
  2. The `[CODE]/[ISSUER]` result rendered with `StellarAddress` + `CopyToClipboard` and an `▲` Stellar Expert deep-link.
  3. A "what happens on-chain" trace: build unsigned XDR → Freighter signs → submit to Horizon.
  4. Burn path note (`build-burn`) for supply reduction.
  5. An `EmptyState`-style note for communities with no token yet.
- **CTAs & navigation targets:** "Issue a token" `→ /` (dashboard); "API: build-issue" `→ /developers#tokens`; "View an asset" `▲ stellar.expert`; "Next: trustlines" `→ #trustline-onboarding`.
- **Colour treatment:** `--gradient-token` card on `--color-surface`; asset code in monospace with `--color-primary`; a `--gradient-seam` divider under the on-chain trace.

#### 16. Trustline Onboarding

- **Type:** Landing Section
- **Anchor:** `#trustline-onboarding`
- **Description:** Explains the one concept a newcomer must learn: a member cannot hold a community token until they sign a trustline. This section turns an intimidating Stellar primitive into a friendly step with a visible before/after balance (`0 ECO` once the trustline exists). Backed by `POST /api/v1/tokens/build-trustline` and `GET /api/v1/accounts/{publicKey}/trustlines`.
- **Content blocks:**
  1. Before/after balance card: token missing → `0 ECO` with the asset row added.
  2. A four-step signed-flow diagram (build → sign → submit → confirm).
  3. Configurable trustline limit, described as "community policy".
  4. A `Tooltip` on "trustline" giving the one-sentence definition.
  5. Failure states as `Badge` variants: unsigned, rejected, already exists.
- **CTAs & navigation targets:** "Review my trustlines" `→ /` (`BalancePanel`); "API: build-trustline" `→ /developers#trustlines`; "Look up an account" `▲ horizon-testnet.stellar.org`; "Next: the treasury" `→ #shared-multisig-treasury`.
- **Colour treatment:** `--gradient-token` on `--color-surface`; the "after" state uses `--color-success-subtle`; the "before" state uses `--color-warning-subtle` rather than red, since nothing has failed.

#### 17. Shared Multi-Signature Treasury

- **Type:** Landing Section
- **Anchor:** `#shared-multisig-treasury`
- **Description:** The community's shared money, held in a distributor account no single person can drain. This is the section that answers the most common objection from a savings group: "who can take the money?". Multi-sig is a Phase 2 item in `PRD.md` §5.3, so it is presented as a designed capability with an honest status label.
- **Content blocks:**
  1. A treasury ledger mock-up: balance in XLM and community token, signer list with weights and a threshold (e.g. 2-of-3).
  2. Recent treasury activity (date, counterparty, amount, status).
  3. Status `Badge`: "Phase 2 — designed, not yet shipped".
  4. Key-handling note: distributor keys encrypted server-side, multi-sig in Phase 2 (`PRD.md` §6).
- **CTAs & navigation targets:** "Treasury endpoint" `→ /developers#communities`; "Roadmap Phase 2" `→ #public-roadmap-timeline`; "Security model" `→ #security-transparency-open-source`; "Balance panel" `→ /`.
- **Colour treatment:** 🟢 `--gradient-treasury` across the section band, with `--color-text-inverse` for copy that lands on the teal end; the ledger card itself stays on `--color-surface-raised` for legibility.

#### 18. Batch Disbursement & Airdrop

- **Type:** Landing Section
- **Anchor:** `#batch-disbursement-airdrop`
- **Description:** The section an NGO administrator cares about most: pay every member in one transaction instead of a spreadsheet of individual transfers. It quotes the real acceptance criterion — up to 100 members per transaction — and shows the resulting audit trail against a manual wire run. Backed by `POST /api/v1/tokens/build-airdrop`.
- **Content blocks:**
  1. Two-column contrast: "Wire run" (3–7% fee, 3–5 days, auditor-heavy) vs "Airdrop run" (a fraction of a cent, ≈4s, auditable).
  2. A recipient list mock-up with `Checkbox` rows and a running total.
  3. One-transaction summary: recipients, total amount, estimated fee.
  4. Result panel: transaction hash + `▲` Stellar Expert link.
- **CTAs & navigation targets:** "API: build-airdrop" `→ /developers#tokens`; "Fee estimator" `→ #fee-remittance-calculator`; "Run a disbursement" `→ /`; "See it on-chain" `▲ stellar.expert`.
- **Colour treatment:** 🟢 `--gradient-treasury` accent behind the summary card; recipient rows on `--color-surface`; the contrast columns use `--color-error-subtle` vs `--color-success-subtle`.

#### 19. P2P Micro-Lending Pools

- **Type:** Landing Section
- **Anchor:** `#p2p-micro-lending-pools`
- **Description:** The Phase 2 headline feature and the reason many communities would adopt CoopLumen at all: a member requests a micro-loan, another member funds it, both sides see one shared status, and repayment settles on-chain. The section walks the full lifecycle so the visitor understands there is no hidden step. Backed by `/api/v1/loans`, `/loans/{id}/disburse`, `/repay` and `/default`.
- **Content blocks:**
  1. Lifecycle rail with the five real statuses: `pending` → `active` → `repaid`, plus `defaulted` and `cancelled`.
  2. A `LoanCard`-style example: amount in community token, optional interest, purpose, borrower/lender short addresses, outstanding balance.
  3. Timeline toggle showing loan events (`/loans/{id}/events`) and the resulting on-chain transaction.
  4. `ProgressBar` for repayment progress.
  5. Honest note: interest calculation and risk scoring are explicitly out of scope in Phase 1 (`PRD.md` §10).
- **CTAs & navigation targets:** "Create a loan" `→ /` (`CreateLoanForm`); "Browse loans" `→ /`; "API: loans" `→ /developers#loans`; "Reputation signals" `→ #reputation-credit-signals`.
- **Colour treatment:** 🧊 `--gradient-surface`; status chips follow the semantic tokens (`--color-warning` pending, `--color-success` active/repaid, `--color-error` defaulted, `--color-text-muted` cancelled); `ProgressBar` fills with `--gradient-cta`.

#### 20. Reputation & Credit Signals

- **Type:** Landing Section
- **Anchor:** `#reputation-credit-signals`
- **Description:** Trust without a credit bureau. A member's repayment record becomes a portable score — on-time repayments up, defaults down — readable by any community on the network. The section reproduces the real leaderboard shape from `ReputationPanel` and is explicit that the score is currently written off-chain in PostgreSQL (`PRD.md` §5.5, open question §11).
- **Content blocks:**
  1. A leaderboard table: rank, short Stellar address, "N on time / M defaulted", score.
  2. A "how the score moves" explainer with the three named inputs: repayment history, tenure, participation.
  3. Address privacy note: addresses are shortened (`GABC…WXYZ`) exactly as the product does.
  4. Status `Badge`: "On-chain scoring — Phase 3".
  5. A "your own score" panel mirroring `MyReputationPanel` for a connected wallet.
- **CTAs & navigation targets:** "View the leaderboard" `→ /`; "API: reputation" `→ /developers#reputation`; "Look up an address" `→ #asset-trustline-explorer`; "Governance weighting" `→ #dao-governance-soroban`.
- **Colour treatment:** `--gradient-token` behind the leaderboard card; score values use `--color-success` for strong records and `--color-warning` for thin ones; rank numbers in `--color-text-muted`.

#### 21. DAO Governance & Soroban Proposals

- **Type:** Landing Section
- **Anchor:** `#dao-governance-soroban`
- **Description:** Where the project is going: members vote on treasury disbursements, membership and rule changes through a Soroban smart contract, weighted by token balance captured at a snapshot. This section must be clearly labelled Phase 3 so the roadmap stays credible, while still showing the intended experience. Source: `PRD.md` §3.4 and §5.5.
- **Content blocks:**
  1. A proposal card: title, type (disbursement / membership / rule change), quorum progress, yes/no tally.
  2. Voting-weight explainer: weight = token balance at the proposal-creation snapshot; passes at quorum over 50% of supply.
  3. Snapshot-block display in monospace.
  4. Status `Badge`: "Phase 3 — Soroban contract planned".
  5. A note on the open question: weighted voting vs simple majority (`PRD.md` §11).
- **CTAs & navigation targets:** "Read the governance plan" `→ /about`; "Soroban docs" `▲` Soroban docs; "Roadmap Phase 3" `→ #public-roadmap-timeline`; "Identity gate" `→ #identity-kyc-oracles`.
- **Colour treatment:** `--gradient-governance` band with `--color-text-inverse` on the info end; tally bars use `--color-success-subtle` (yes) and `--color-error-subtle` (no).

#### 22. Identity, KYC & Price Oracles

- **Type:** Landing Section
- **Anchor:** `#identity-kyc-oracles`
- **Description:** The Phase 4 frontier, presented as direction rather than delivery: SEP-12 KYC with anchor providers, optional World ID / Proof of Humanity gating for governance, on-chain attestation for verified communities, and an XLM/USD price feed so loan values can be stated in familiar currency. It also names the open question about which anchor to recommend. Source: `PRD.md` §5.6, §11, `GET /api/v1/prices/xlm`.
- **Content blocks:**
  1. Three cards: **KYC (SEP-12)**, **Decentralized identity (World ID / Proof of Humanity)**, **Price oracle (XLM/USD)**.
  2. A live XLM/USD price chip with a "source + updated at" caption.
  3. A privacy framing note: verification is optional and per-governance only.
  4. Status `Badge`: "Phase 4 — planned".
- **CTAs & navigation targets:** "SEP-12 standard" `▲` Stellar docs; "Price endpoint" `→ /developers#prices`; "Roadmap Phase 4" `→ #public-roadmap-timeline`; "Contact partnerships" `→ /contact`.
- **Colour treatment:** 🧊 `--gradient-surface` with `--gradient-trust` accents inside the three cards; the price chip uses `--color-success-subtle` for an up tick and `--color-error-subtle` for a down tick.

### Group E — Blockchain-Native Experience

#### 23. Transaction Lifecycle Visualizer

- **Type:** Landing Section
- **Anchor:** `#transaction-lifecycle-visualizer`
- **Description:** The section that makes the blockchain _felt_. A visitor presses one button and watches a real transaction travel: the backend builds an unsigned XDR, Freighter signs it client-side, the signed envelope is submitted to Horizon, and the ledger closes. Each stage lights up in turn with a timestamp and a hash — the clearest possible answer to "so what actually happens?".
- **Content blocks:**
  1. Five-stage rail: **Build** (unsigned XDR) → **Sign** (Freighter, in-browser) → **Submit** → **Ledger close** (~3–5s) → **Confirmed**.
  2. Live elapsed timer per stage and a total.
  3. XDR envelope preview in monospace (truncated, with `CopyToClipboard`).
  4. Result card: transaction hash, ledger number, fee paid, `▲` Stellar Expert link.
  5. Failure-path toggle showing a rejected signature as an `Alert` with `role="alert"`.
  6. A "run it yourself" CTA into the real dashboard.
- **CTAs & navigation targets:** "Run it on testnet" `→ /`; "API: transactions" `→ /developers#transactions`; "See it on the explorer" `▲ stellar.expert`; "Why sign client-side?" `→ #security-transparency-open-source`.
- **Colour treatment:** `--gradient-ledger` sweeping along the stage rail; each completed stage lights with `--gradient-cta`; pending stages stay `--color-border`; the animation becomes a static stepped diagram under reduced motion.

#### 24. Live On-Chain Activity Feed

- **Type:** Landing Section
- **Anchor:** `#live-onchain-activity-feed`
- **Description:** A running ticker of what the network is actually doing for CoopLumen right now: a community registered, a token issued, a trustline established, an airdrop sent, a loan repaid. Rows grow in from the top and every hash is a real link out to the explorer, turning the landing page from a static argument into something alive.
- **Content blocks:**
  1. Feed rows: event icon, short address, amount + asset code, relative time ("12s ago").
  2. An event-type `Badge` per row: issued / trustline / transfer / airdrop / loan.
  3. Each hash deep-links `▲` to Stellar Expert.
  4. An offline state using `EmptyState` when the API is unreachable.
  5. A "pause" control for visitors who do not want motion.
- **CTAs & navigation targets:** "Explore in the app" `→ /`; "Look up a hash" `▲ stellar.expert`; "See all communities" `→ /communities`; "API: balances history" `→ /developers#balances`.
- **Colour treatment:** `--color-surface-raised` rows on a `--gradient-surface` band; a `--gradient-ledger` glow under the newest row; event icons borrow the semantic tokens (issue `--color-primary`, transfer `--color-secondary`, loan `--color-info`).

#### 25. Community Discovery Explorer

- **Type:** Landing Section
- **Anchor:** `#community-discovery-explorer`
- **Description:** Proof that this is a network, not a single-tenant app. A live preview of the discovery experience showing real communities, their asset codes, member counts and token counts — the exact shape of `CommunityCard` today (`Eco Lagos`, `CO1`, `member_count`, `token_count`, issuer short key, created date). It gives an NGO administrator the sense of an ecosystem rather than one tool.
- **Content blocks:**
  1. A grid of four to six `CommunityCard` previews with metrics.
  2. A search filter mock-up and a sector `Select`.
  3. A `Pagination` strip showing the list scales past one page.
  4. Join affordances mirroring the real `Join Community` / `Joined` states.
- **CTAs & navigation targets:** "Browse all communities" `→ /communities`; a card's overlay `→ /communities/{id}`; "Create your own" `→ /`; "Search API" `→ /developers#communities`.
- **Colour treatment:** `--gradient-surface`; asset codes on `--gradient-token` chips; the "Joined" state carries `--color-success-subtle`, "Join" keeps the outline style.

#### 26. Asset & Trustline Explorer

- **Type:** Landing Section
- **Anchor:** `#asset-trustline-explorer`
- **Description:** A genuinely useful utility rather than a marketing block: paste a Stellar address or an asset pair and see that account's balances and trustlines. Because it reuses `StellarAddress`, `CopyToClipboard`, `Table` and the balances endpoint, it doubles as a demonstration of the API working in public.
- **Content blocks:**
  1. An input accepting either a `G…` public key or a `CODE/ISSUER` pair, validated the way the product validates addresses.
  2. Results `Table`: asset code, issuer (short), balance, trustline limit.
  3. Native XLM row pinned first, matching `BalancePanel`.
  4. Error state for an unfunded or malformed account.
  5. "Copy address" and "View on explorer" actions.
- **CTAs & navigation targets:** "Balances API" `→ /developers#balances`; "Trustlines API" `→ /developers#trustlines`; "Open on explorer" `▲ stellar.expert`; "Horizon endpoint" `▲ horizon-testnet.stellar.org`.
- **Colour treatment:** `--gradient-token` behind the input card; the results table uses `--color-surface-raised` with `--color-border` rules; the native XLM row gets a `--color-info-subtle` tint.

#### 27. Fee & Remittance Savings Calculator

- **Type:** Landing Section
- **Anchor:** `#fee-remittance-calculator`
- **Description:** The emotional payoff for a diaspora community or an NGO: a plain calculator showing what the same transfer costs today versus on Stellar. The visitor enters an amount, and the section contrasts a 3–7% wire fee with 3–5 day settlement against a Stellar fee of a fraction of a cent settling in ~4 seconds. It uses the real `GET /api/v1/fees/estimate` endpoint.
- **Content blocks:**
  1. `AmountInput` + a currency `Select` for the amount being sent.
  2. Two result columns: **Today** (wire/remittance) vs **On Stellar** (base fee, plus an optional community-token path).
  3. Cost difference in currency and percentage, plus time difference in days vs seconds.
  4. A small chart comparing the two fees across amount bands.
  5. A caveat line: figures are illustrative, based on published averages, not a quote.
- **CTAs & navigation targets:** "Fees API" `→ /developers#fees`; "Send a real payment on testnet" `→ /`; "Compare in the table" `→ #comparison-matrix`; "The unbanked context" `→ #financial-exclusion-problem`.
- **Colour treatment:** `--gradient-cta` for the "On Stellar" column header, `--color-error-subtle` for the "Today" column; the savings figure uses `--gradient-text-hero`.

#### 28. Network Health & XLM/USD Ticker

- **Type:** Landing Section
- **Anchor:** `#network-health-xlm-ticker`
- **Description:** A small, always-on instrument panel for people who think in ledgers: XLM/USD price, 24-hour movement, ledger close interval trend and Horizon latency. It closes the blockchain-native group by leaving the visitor with a live number rather than a slogan. The price data is shared with section 22's chip.
- **Content blocks:**
  1. XLM/USD price with a change indicator and "updated Ns ago".
  2. Ledger close interval sparkline (target ≈4s).
  3. Horizon latency and success-rate tiles.
  4. Current network pill and an explorer link.
  5. A disclosure line naming the price source.
- **CTAs & navigation targets:** "Prices API" `→ /developers#prices`; "Network status page" `→ /status`; "Horizon stats" `▲ horizon-testnet.stellar.org`; "Back to the metrics" `→ #live-network-metrics-strip`.
- **Colour treatment:** `--gradient-surface` with a `--gradient-ledger` sparkline fill; deltas use `--color-success-subtle` / `--color-error-subtle`; neutral values stay `--color-text-muted`.

### Group F — Proof, Trust & Roadmap

#### 29. Built for These Communities

- **Type:** Landing Section
- **Anchor:** `#built-for-these-communities`
- **Description:** Six personas taken directly from `PRD.md` §4, each with the need CoopLumen actually answers. Naming them prevents the landing page from pitching "crypto users" — the audience is treasurers, members and NGO administrators, with developers, auditors and governance participants as the secondary set.
- **Content blocks:**
  1. Primary trio: **Community Treasurer** (issue a token, disburse funds, view the audit trail), **Community Member** (view balance, apply for a loan, repay on-chain), **NGO Administrator** (register communities, batch-disburse tokens).
  2. Secondary trio: **Developer / Integrator**, **Governance Participant**, **Auditor**.
  3. Each card states that persona's key need in their own words.
  4. A "which one are you?" selector that jumps to the most relevant section.
- **CTAs & navigation targets:** Treasurer `→ #community-token-issuance`; Member `→ #p2p-micro-lending-pools`; NGO `→ #batch-disbursement-airdrop`; Developer `→ /developers`; Governance `→ #dao-governance-soroban`; Auditor `→ #live-onchain-activity-feed`.
- **Colour treatment:** `--gradient-surface`; six cards in a two-column (`md`) then three-column (`lg`) grid using the token grid utilities; each persona icon tinted from a different semantic token.

#### 30. Community Stories & Case Studies

- **Type:** Landing Section
- **Anchor:** `#community-stories`
- **Description:** Concrete narratives for the three settings the product is built for — a local savings circle, a diaspora network, an NGO grant programme — each told as a before/after with the on-chain artefact that proves it. Real testimonials replace this section once the Phase 1 pilots complete; until then it reads as an illustrative scenario and is labelled as such.
- **Content blocks:**
  1. Three story cards with a short quote, a community type and an outcome metric.
  2. For each: the asset code used and the transaction volume moved.
  3. A "verified on-chain" link `▲` per story so a sceptic can check.
  4. A disclosure line: illustrative scenario until pilot data is available.
- **CTAs & navigation targets:** "Read the full case study" `→ /blog`; "Register your community" `→ /`; "Talk to the team" `→ /contact`; explorer links `▲`.
- **Colour treatment:** `--gradient-aurora`; quotes in `--color-text` with attribution in `--color-text-muted`; the outcome metric uses `--gradient-text-hero`.

#### 31. Public Roadmap Timeline

- **Type:** Landing Section
- **Anchor:** `#public-roadmap-timeline`
- **Description:** The four phases from `PRD.md` §8 rendered honestly, including what is already done and what is not. Publishing a roadmap with unchecked boxes is itself a trust signal in open source, so completed items carry a success `Badge` and planned items a neutral one.
- **Content blocks:**
  1. Phase 1 — Foundation (current): monorepo scaffold, Stellar SDK integration, community API, balance dashboard with Freighter, lint/type/test setup — done; testnet walkthrough, CI/CD, staging — open.
  2. Phase 2 — P2P Lending: loan request/acceptance, on-chain repayment, reputation scoring, multi-sig distributors, batch disbursement, mobile UI.
  3. Phase 3 — DAO Governance: Soroban voting contract, token-weighted snapshots, proposal types, governance UI.
  4. Phase 4 — Identity, KYC & Oracles: SEP-12, decentralized identity, price oracle, remittance corridors.
  5. A `Badge` state and target quarter per item.
- **CTAs & navigation targets:** "Follow the roadmap on GitHub" `▲` repo issues/projects; "Read the PRD" `→ /about`; "Contribute to an open item" `→ #contribution-bounty-spotlight`; "Governance detail" `→ #dao-governance-soroban`.
- **Colour treatment:** `--gradient-aurora` with a `--gradient-seam` spine down the timeline; the current Phase 1 marker uses `--gradient-cta`, Phase 3 uses `--gradient-governance`.

#### 32. Impact Metrics & Success Targets

- **Type:** Landing Section
- **Anchor:** `#impact-metrics`
- **Description:** The measurable definition of success, taken verbatim from `PRD.md` §9 — registered communities, active wallets, on-chain transaction volume, GitHub contributors, resolved issues and test coverage, each with a Phase 1 and a Phase 2 target. Publishing targets you can miss is what separates a project from a pitch.
- **Content blocks:**
  1. Six metric rows: Communities (10 → 100), Active wallets (50 → 1,000), On-chain transactions (1,000 → 50,000), GitHub contributors (5 → 25), Issues resolved (20 → 100), Test coverage (≥70% → ≥80%).
  2. Each row shows current, Phase 1 target and Phase 2 target.
  3. A `ProgressBar` per metric.
  4. A "last updated" caption plus a link to how each metric is measured.
- **CTAs & navigation targets:** "Live figures" `→ #live-network-metrics-strip`; "How we measure" `→ /about`; "Contribute" `→ #contribution-bounty-spotlight`; "Coverage report" `▲` CI.
- **Colour treatment:** `--gradient-surface`; `ProgressBar` fills from `--gradient-cta`; achieved targets get `--color-success` badges and in-progress ones `--color-info`.

#### 33. Security, Transparency & Open Source

- **Type:** Landing Section
- **Anchor:** `#security-transparency-open-source`
- **Description:** The section a cautious treasurer reads last and decides on. It states the security principles from `PRD.md` §6 and `SECURITY.md`: no private keys server-side for user wallets, all mutations require a signed Stellar transaction with no privileged backend bypass, inputs validated at the API boundary, and Helmet CSP/CORS enforced — plus a real responsible-disclosure process with published timelines.
- **Content blocks:**
  1. Four principle cards: **Client-side signing only**, **No privileged bypass**, **Validated inputs**, **Hardened headers**.
  2. The disclosure SLA table from `SECURITY.md`: 24h acknowledgement, 72h severity assessment, 14-day patch target for critical/high, coordinated public disclosure.
  3. Reporting channel: `security@cooplumen.org` with a `CopyToClipboard` control.
  4. Open-source facts: MIT licence, `main` supported, `CONTRIBUTING.md` and `CODEOWNERS` present, funding via Open Collective.
- **CTAs & navigation targets:** "Read the security policy" `→ /security`; "Read `SECURITY.md`" `▲` repo; "Report a vulnerability" `→ mailto:security@cooplumen.org`; "Contributing guide" `▲` repo; "Support the project" `▲` Open Collective.
- **Colour treatment:** 🟩 `--gradient-trust` (success → info tint); the disclosure table uses `--color-surface-raised`; the email chip gets a `--gradient-seam` underline.

### Group G — Conversion, Community & Content

#### 34. Pricing & Community Plans

- **Type:** Landing Section
- **Anchor:** `#pricing-community-plans`
- **Description:** The honest pricing answer for an open-source, testnet-stage project: the software is free and self-hostable, so the "plans" describe operating models rather than a paywall. Three tiers keep it concrete — Free core, Self-hosted, and NGO & anchor support — with the caveat that no fiat handling exists in Phase 1 (`PRD.md` §10).
- **Content blocks:**
  1. Three plan cards: **Community (free)** — host your own community on the public app; **Self-hosted** — run the Docker Compose stack yourself; **NGO & Anchor** — onboarding, batch tooling, partnership support.
  2. A feature comparison `Table` (communities, tokens, lending, governance, support).
  3. A no-custody note: CoopLumen never holds funds and never stores user private keys.
  4. Testnet-status `Alert` so nobody mistakes this for a production financial service.
- **CTAs & navigation targets:** "Get started free" `→ /`; "Self-hosting guide" `→ /developers#self-hosting`; "Talk to partnerships" `→ /contact`; "Pricing page" `→ /pricing` (reserved route).
- **Colour treatment:** 🟣 `--gradient-cta` on the recommended card's header only; the rest stay `--color-surface-raised`; the comparison table uses `--gradient-token` column headers.

#### 35. Community & Support Hub

- **Type:** Landing Section
- **Anchor:** `#community-support-hub`
- **Description:** Where a stuck visitor goes. It routes people to the right channel instead of a single generic "contact us": GitHub Discussions for design questions, the Stellar Developers Discord for protocol questions, and the repo issue tracker for bugs — with the repository's issue templates named so people file useful reports.
- **Content blocks:**
  1. Four channel cards: **GitHub Discussions**, **Discord** (marked "coming soon", as the README states), **Stellar Developers Discord**, **Issue tracker** (bug report / feature request templates).
  2. Recommended-response expectations.
  3. A "before you ask" mini-checklist (network set to testnet, wallet funded, API running).
  4. `NetworkWarning`-style reminder that unexplained failures are often a network mismatch.
- **CTAs & navigation targets:** GitHub Discussions `▲`; Stellar Developers Discord `▲ discord.gg/stellar`; bug report `▲ .github/ISSUE_TEMPLATE/bug_report.yml`; feature request `▲ .github/ISSUE_TEMPLATE/feature_request.yml`; direct contact `→ /contact`.
- **Colour treatment:** 🟢 `--gradient-treasury` at low opacity behind the channel grid; channel icons on `--color-surface-raised`; the checklist uses `--color-info-subtle`.

#### 36. Contribution & Bounty Spotlight

- **Type:** Landing Section
- **Anchor:** `#contribution-bounty-spotlight`
- **Description:** CoopLumen describes itself as "open source, contributor-first", so the landing page must actively recruit contributors. This section explains how to start — branch model, conventions, review process — and spotlights the work that is genuinely open, including the Phase 1 items still unchecked in the roadmap.
- **Content blocks:**
  1. A three-step on-ramp: pick an issue → read `CONTRIBUTING.md` → open a pull request.
  2. Cards for open work: testnet end-to-end walkthrough, CI/CD pipeline, staging environment, mobile-responsive UI.
  3. Bounty / funding note: Open Collective and GitHub Sponsors.
  4. Contributor count and resolved-issue counter (shared with section 32).
  5. A `CONTRIBUTORS.md` strip acknowledging existing contributors.
- **CTAs & navigation targets:** "Browse good first issues" `▲` repo issues; "Read `CONTRIBUTING.md`" `▲` repo; "Open Collective" `▲`; "GitHub Sponsors" `▲` (`FUNDING.yml`); "Contributor list" `→ /about#contributors`.
- **Colour treatment:** `--gradient-mesh` on `--color-surface`; issue cards use `--gradient-token` accents; bounty badges use `--color-warning-subtle`.

#### 37. Newsletter & Testnet Waitlist Capture

- **Type:** Landing Section
- **Anchor:** `#newsletter-testnet-waitlist`
- **Description:** A single, low-friction conversion point for visitors who are interested but not ready to connect a wallet. It offers two ways in: an email for release notes, or a Stellar public key to be added to the testnet walkthrough list. Including the public-key option keeps the capture blockchain-native instead of generic.
- **Content blocks:**
  1. Email field (validated as `Form`/`Input` already do) and a submit `Button`.
  2. Optional public-key field with a `Tooltip` explaining what will and will not be done with it.
  3. What you get list: Phase 1 testnet walkthrough invitation, release notes, launch announcements.
  4. Success and error states via `Alert`.
  5. Privacy line: only email or public key, never a secret key.
- **CTAs & navigation targets:** "Join the waitlist" `→` submit; "Read the changelog instead" `→ #blog-changelog-preview`; "Get involved now" `→ #contribution-bounty-spotlight`; privacy policy `→ /privacy`.
- **Colour treatment:** `--gradient-cta` accent bar along the card's top edge; inputs on `--color-surface-raised`; success state in `--color-success-subtle`.

#### 38. Blog, Changelog & Release Notes Preview

- **Type:** Landing Section
- **Anchor:** `#blog-changelog-preview`
- **Description:** The "this project is alive" section. It surfaces the three most recent entries from the project's own `CHANGELOG.md` — component additions, claimable-balance support, design-token work — proving that development is continuous and documented, and gives the visitor a reason to come back.
- **Content blocks:**
  1. Three latest entries with date, title and a one-line summary.
  2. A category `Badge` per entry: feature / fix / docs / infrastructure.
  3. A "view all" link to the changelog and blog.
  4. A subscribe hook linking back to section 37.
  5. An `RSS`/feed affordance marker.
- **CTAs & navigation targets:** "Read the changelog" `→ /changelog`; "All posts" `→ /blog`; "View `CHANGELOG.md`" `▲` repo; "Subscribe" `→ #newsletter-testnet-waitlist`.
- **Colour treatment:** `--gradient-aurora` behind the entry list; category badges reuse the `Badge` variants; entry titles in `--color-text` with `--color-text-muted` dates.

#### 39. Final CTA Band

- **Type:** Landing Section
- **Anchor:** `#final-cta-band`
- **Description:** The closing argument, after the visitor has seen live ledgers, a wallet connection, real transactions and an honest roadmap. One band, one sentence, two buttons — start on testnet, or talk to the team — with the network state repeated so the action is never ambiguous about which chain it happens on.
- **Content blocks:**
  1. Headline: "Launch your community treasury on testnet."
  2. One supporting line restating the three pillars.
  3. Two CTAs: **Connect Freighter & Start** (primary) and **Talk to us** (secondary).
  4. `NetworkBadge` and a "no real funds — Phase 1 is testnet only" `Alert`.
  5. Small print: open source, MIT, self-hostable.
- **CTAs & navigation targets:** Primary `→ /` (or `→ #wallet-connect-hero-panel` when unconnected); secondary `→ /contact`; docs `→ /developers`; back to top `→ #hero-section`.
- **Colour treatment:** 🟣 `--gradient-hero` full-bleed with `--gradient-cta` buttons; white heading text on the violet portion only; the teal end carries an illustration or the network pill, never body copy in dark mode.

#### 40. Contact Us

- **Type:** Landing Section
- **Anchor:** `#contact-us`
- **Description:** The landing-page half of the Contact route: a short form plus routing so the message reaches the right place. It names the channels explicitly — general enquiries, partnerships, and the separate security address — because a security report must never go into a general inbox.
- **Content blocks:**
  1. Form: name, optional organisation, optional Stellar public key, topic `Select` (support / partnership / press / bug), message.
  2. Topic routing note: security reports must use `security@cooplumen.org`, not this form.
  3. Direct channel list with `CopyToClipboard` controls.
  4. Response expectation and office-hours note.
  5. Success and validation states via `Alert` and field-level errors.
- **CTAs & navigation targets:** "Send message" `→ /contact` (page P7); "Email security" `→ mailto:security@cooplumen.org`; "Join the community" `→ #community-support-hub`; "File an issue" `▲` repo; "Partnership enquiries" `→ /contact?topic=partnership`.
- **Colour treatment:** `--color-surface` with a `--gradient-token` field focus ring and a `--gradient-cta` submit button; validation errors use `--color-error`/`--color-error-subtle`; a `--gradient-seam` divides the form from the channel list.

---

### Quick Reference — the 40 sections

| #   | Section                                 | Anchors to / links to route                                                        | Gradient                                       |
| --- | --------------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------- |
| 1   | Header Navigation Bar                   | `/`, `/communities`, `/developers`, `#pricing-community-plans`                     | `--gradient-seam`                              |
| 2   | Mobile Navigation Drawer                | header destinations, `#wallet-connect-hero-panel`                                  | overlay + surface-raised                       |
| 3   | Network Status Bar                      | `▲` Stellar Expert, `#fee-remittance-calculator`                                   | `--gradient-ledger`                            |
| 4   | Site Footer                             | every anchor and reserved route                                                    | `--gradient-seam`                              |
| 5   | Hero Section                            | `/`, `/developers`, `#wallet-connect-hero-panel`                                   | `--gradient-hero`                              |
| 6   | Live Network Metrics Strip              | `/communities`, `#impact-metrics`                                                  | `--gradient-ledger`                            |
| 7   | Wallet Connect Hero Panel               | `▲` Freighter, `#three-pillar-value-proposition`                                   | `--gradient-token` + `--gradient-aurora`       |
| 8   | Built-on-Stellar Trust Bar              | `▲` Stellar, Horizon, Soroban, Freighter, npm                                      | `--gradient-seam`                              |
| 9   | Three-Pillar Value Proposition          | `#community-token-issuance`, `#p2p-micro-lending-pools`, `#dao-governance-soroban` | `--gradient-token` / `treasury` / `governance` |
| 10  | The Financial Exclusion Problem         | `/about`, `#how-cooplumen-works`                                                   | `--gradient-aurora`                            |
| 11  | How CoopLumen Works                     | `/developers`, `#trustline-onboarding`, `/`                                        | `--gradient-mesh`                              |
| 12  | Why Stellar                             | `▲ developers.stellar.org`, `#fee-remittance-calculator`                           | `--gradient-surface`                           |
| 13  | Comparison Matrix                       | `/about`, `/`, `/faq`                                                              | `--gradient-aurora`                            |
| 14  | Architecture Transparency               | `/developers#openapi`, `/security`                                                 | `--gradient-mesh`                              |
| 15  | Community Token Issuance                | `/`, `/developers#tokens`                                                          | `--gradient-token`                             |
| 16  | Trustline Onboarding                    | `/`, `/developers#trustlines`                                                      | `--gradient-token`                             |
| 17  | Shared Multi-Signature Treasury         | `/developers#communities`, `#public-roadmap-timeline`                              | `--gradient-treasury`                          |
| 18  | Batch Disbursement & Airdrop            | `/developers#tokens`, `#fee-remittance-calculator`                                 | `--gradient-treasury`                          |
| 19  | P2P Micro-Lending Pools                 | `/`, `/developers#loans`                                                           | `--gradient-surface`                           |
| 20  | Reputation & Credit Signals             | `/`, `/developers#reputation`                                                      | `--gradient-token`                             |
| 21  | DAO Governance & Soroban Proposals      | `/about`, `#public-roadmap-timeline`                                               | `--gradient-governance`                        |
| 22  | Identity, KYC & Price Oracles           | `/developers#prices`, `/contact`                                                   | `--gradient-surface` + `--gradient-trust`      |
| 23  | Transaction Lifecycle Visualizer        | `/`, `/developers#transactions`                                                    | `--gradient-ledger` + `--gradient-cta`         |
| 24  | Live On-Chain Activity Feed             | `/`, `/communities`                                                                | `--gradient-surface` + `--gradient-ledger`     |
| 25  | Community Discovery Explorer            | `/communities`, `/communities/{id}`                                                | `--gradient-surface`                           |
| 26  | Asset & Trustline Explorer              | `/developers#balances`, `▲` Stellar Expert                                         | `--gradient-token`                             |
| 27  | Fee & Remittance Savings Calculator     | `/developers#fees`, `#comparison-matrix`                                           | `--gradient-cta`                               |
| 28  | Network Health & XLM/USD Ticker         | `/developers#prices`, `/status`                                                    | `--gradient-surface`                           |
| 29  | Built for These Communities             | `#community-token-issuance`, `/developers`, `#dao-governance-soroban`              | `--gradient-surface`                           |
| 30  | Community Stories & Case Studies        | `/blog`, `/`, `/contact`                                                           | `--gradient-aurora`                            |
| 31  | Public Roadmap Timeline                 | `▲` GitHub, `/about`, `#contribution-bounty-spotlight`                             | `--gradient-aurora`                            |
| 32  | Impact Metrics & Success Targets        | `#live-network-metrics-strip`, `/about`                                            | `--gradient-surface`                           |
| 33  | Security, Transparency & Open Source    | `/security`, `▲` repo, `mailto:security@cooplumen.org`                             | `--gradient-trust`                             |
| 34  | Pricing & Community Plans               | `/`, `/developers#self-hosting`, `/contact`                                        | `--gradient-cta`                               |
| 35  | Community & Support Hub                 | `▲` Discussions, `▲ discord.gg/stellar`, `/contact`                                | `--gradient-treasury`                          |
| 36  | Contribution & Bounty Spotlight         | `▲` issues, `▲` Open Collective, `/about#contributors`                             | `--gradient-mesh`                              |
| 37  | Newsletter & Testnet Waitlist           | `#blog-changelog-preview`, `/privacy`                                              | `--gradient-cta`                               |
| 38  | Blog, Changelog & Release Notes Preview | `/changelog`, `/blog`, `▲` repo                                                    | `--gradient-aurora`                            |
| 39  | Final CTA Band                          | `/`, `/contact`, `/developers`                                                     | `--gradient-hero`                              |
| 40  | Contact Us                              | `/contact`, `mailto:security@cooplumen.org`                                        | `--gradient-token` + `--gradient-cta`          |

---

## 7. Pages (separate list, outside the 40)

These are the standalone routes the landing page links into. They are **not** counted in the 40 sections above. Every page reuses the same global chrome (sections 1–4) and the same token system.

### P1. Discover Communities

- **Route:** `/communities` — **exists today** at `frontend/src/app/communities/page.tsx`
- **Description:** The public index of every registered community, and the strongest proof that CoopLumen is a network. It already ships with a 15-item mock generator, a `CommunityList` with pagination (`itemsPerPage={6}`) and a `CommunityCard` per community showing name, asset code, description, issuer short key, created date, member count and token count.
- **Rendered sections:** global chrome (1–4), page header, filter/search bar, `CommunityList` grid, `Pagination`, empty and error states, a create-community CTA.
- **Primary CTAs:** a card's link overlay `→ /communities/{id}`; `Join Community` / `Joined`; "Create a community" `→ /`; "Back to home" `→ /`.
- **Review note:** this page currently uses Tailwind-style class names (`bg-gray-50`, `dark:bg-[#0a0a0a]`, `text-3xl font-bold`) that resolve to nothing because Tailwind is not installed. Re-skinning it onto the design tokens is part of bringing the landing page and the app into one visual system.
- **Colour treatment:** `--color-bg` page, `--color-surface` cards, `--gradient-token` asset-code chips.

### P2. Community Detail

- **Route:** `/communities/[id]`
- **Description:** The single-community page the `CommunityCard` overlay currently points at. It is the deepest view of one co-operative: its asset, its members, its treasury activity and its loans. This is where a visitor decides whether to trust a stranger's community.
- **Rendered sections:** global chrome, community header (name, asset code, issuer, created date), member list, treasury summary, token activity, loans for this community (`/balances/community/{id}/loans`), reputation for this community.
- **Primary CTAs:** `Join` / `Leave`; "Establish trustline" `→ /developers#trustlines`; "View asset on explorer" `▲ stellar.expert`; "Request a loan" `→ /`; back `→ /communities`.
- **Colour treatment:** `--gradient-surface` header band with a `--gradient-token` asset chip.

### P3. Dashboard / App

- **Route:** `/` — **exists today** at `frontend/src/app/page.tsx` (renders `Dashboard`)
- **Description:** The application itself, and the destination of nearly every primary CTA on the landing page. Its layout is already established: a sticky header with `ThemeToggle` and `WalletConnect`, a left sidebar (`BalancePanel`, `PortfolioPanel`, `MyReputationPanel`, `ReputationPanel`) and a main column with the communities grid, `CreateLoanForm` and `LoansSection`.
- **Rendered sections:** global chrome, wallet header, sidebar panels, communities grid with `LoadingSkeleton` and `EmptyState` fallbacks, loan creation, loans list with filters.
- **Primary CTAs:** `Connect Freighter`; `Disconnect`; `Retry` on error; "Create the first one" when empty; "View details" per community `→ /communities/{id}`.
- **Colour treatment:** unchanged from the existing `Dashboard.module.css`; the landing page's gradients are reserved for the marketing route so the app stays neutral for long work sessions.

### P4. Developers & API Reference

- **Route:** `/developers`
- **Description:** The page for the secondary persona "Developer / Integrator". It hosts the OpenAPI reference (`docs/openapi.yaml`), grouped by resource, plus code samples and the self-hosting path. Because integration is a first-class feature of an open-source protocol project, this page carries real weight in the header, not just the footer.
- **Rendered sections:** global chrome, quickstart, OpenAPI explorer with anchors `#communities`, `#tokens`, `#trustlines`, `#transactions`, `#balances`, `#loans`, `#reputation`, `#prices`, `#fees`, `#webhooks`; self-hosting guide (`#self-hosting`); SDK version note (`@stellar/stellar-sdk` 12.3); a contribution CTA.
- **Primary CTAs:** "Copy cURL" (`CopyToClipboard`); "Try it on testnet" `→ /`; "Read `docs/architecture.md`" `▲` repo; "Open an issue" `▲` repo; "Start contributing" `→ #contribution-bounty-spotlight`.
- **Colour treatment:** `--gradient-mesh` hero, `--color-surface-raised` code blocks, `--gradient-seam` under the quickstart.

### P5. About

- **Route:** `/about`
- **Description:** The mission, the problem statement, the roadmap and the people. It is the long-form home of everything the landing page summarises, and it carries the `CONTRIBUTORS.md` acknowledgement under an `#contributors` anchor.
- **Rendered sections:** global chrome, mission statement (`PRD.md` §1), problem statement (§2), target users (§4), full roadmap (§8), success metrics (§9), non-goals (§10) published honestly, contributor list, licence.
- **Primary CTAs:** "Read the PRD" `▲` repo; "See the roadmap" `→ /#public-roadmap-timeline`; "Contribute" `▲` GitHub; "Contact" `→ /contact`.
- **Colour treatment:** `--gradient-aurora` hero, `--color-surface` body, `--gradient-seam` dividers.

### P6. FAQ

- **Route:** `/faq`
- **Description:** The page that removes the last objections, grouped so a non-technical visitor and a developer can each find their own answers. Every answer is written to be quotable on its own, since FAQs are often the entry point from a search engine.
- **Rendered sections:** global chrome, grouped question sets — **Getting started** (what is a wallet, why Freighter), **Stellar basics** (what is a trustline, what is XLM, what is an asset code, what is a ledger), **Community finance** (who can issue a token, who controls the treasury, what happens if someone defaults), **Safety** (can CoopLumen take my funds, where are private keys stored, is this audited), **Status** (is this mainnet, is real money involved, what is the licence).
- **Primary CTAs:** "Still stuck? Contact us" `→ /contact`; "Join the community" `→ /#community-support-hub`; "Read the docs" `→ /developers`.
- **Colour treatment:** `--color-surface` with `--color-border` accordion rules; an open answer gets a `--gradient-token` left edge; no saturated gradients on this page, since it is a reference page.

### P7. Contact

- **Route:** `/contact`
- **Description:** The standalone home of the Contact Us section (section 40), so the form is linkable, indexable and reachable from the footer and every CTA that says "talk to us". It routes by topic, with security reports explicitly separated.
- **Rendered sections:** global chrome, the contact form, the direct channel list (general, partnerships, press, `security@cooplumen.org`), response-time expectations, office hours, and a map of who to ask for what.
- **Primary CTAs:** "Send message"; "Email security" `→ mailto:security@cooplumen.org`; "Open a GitHub issue" `▲` repo; "Join Discord" `▲`; back `→ /`.
- **Colour treatment:** `--color-surface` with a `--gradient-token` focus treatment on inputs and a `--gradient-cta` submit button.

### Pages summary

| #   | Page                       | Route               | Exists today | Links in from sections                           |
| --- | -------------------------- | ------------------- | ------------ | ------------------------------------------------ |
| P1  | Discover Communities       | `/communities`      | yes          | 1, 6, 24, 25, 35                                 |
| P2  | Community Detail           | `/communities/[id]` | no           | P1 cards, 25                                     |
| P3  | Dashboard / App            | `/`                 | yes          | 1, 5, 11, 13, 15, 16, 19, 20, 23, 24, 30, 34, 39 |
| P4  | Developers & API Reference | `/developers`       | no           | 1, 5, 11, 14, 31, 34, 36, 39                     |
| P5  | About                      | `/about`            | no           | 4, 10, 13, 21, 30, 31, 32, 36                    |
| P6  | FAQ                        | `/faq`              | no           | 4, 13                                            |
| P7  | Contact                    | `/contact`          | no           | 4, 22, 30, 34, 39, 40                            |

---

## 8. Appendices

### Appendix A — Existing component reuse map

The landing page should be assembled almost entirely from primitives that already exist, so it looks and behaves like the product rather than a separate marketing skin.

| Section(s)     | Reuse from `frontend/src/components`                                                                                                  |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| 1, 2           | `ThemeToggle`, `WalletConnect`, `NetworkBadge`, `Button`, `Modal` (drawer), `Alert`                                                   |
| 3              | `NetworkBadge`, `Spinner`, `Tooltip`                                                                                                  |
| 4              | `Badge`, `NetworkBadge`, `ThemeToggle`                                                                                                |
| 5, 7           | `WalletConnect`, `BalancePanel`, `QRCode`, `StellarAddress`, `CopyToClipboard`, `Button`, `Alert`, `NetworkWarning`                   |
| 6, 32          | `Card`, `ProgressBar`, `LoadingSkeleton`, `Table`                                                                                     |
| 8, 14          | `Card`, `Badge`, `Tooltip`                                                                                                            |
| 9, 29          | `Card`, `Badge`, `Tooltip`                                                                                                            |
| 10, 13         | `Table`, `Alert`, `Badge`                                                                                                             |
| 11, 23         | `Card`, `ProgressBar`, `Alert`, `Spinner`, `CopyToClipboard`                                                                          |
| 15, 16, 17, 18 | `Form`, `Input`, `AmountInput`, `Select`, `Checkbox`, `StellarAddress`, `CopyToClipboard`, `Table`, `Modal`, `ConfirmDialog`, `Badge` |
| 19, 20         | `LoanCard`, `LoanHistory`, `LoanActions`, `ReputationPanel`, `MyReputationPanel`, `ProgressBar`, `Pagination`                         |
| 21             | `Card`, `ProgressBar`, `Badge`, `Table`                                                                                               |
| 22, 28         | `Card`, `Badge`, `Alert`, `Tooltip`                                                                                                   |
| 24             | `Table`, `Badge`, `EmptyState`, `LoadingSkeleton`, `StellarAddress`                                                                   |
| 25             | `CommunityCard`, `CommunityList`, `Pagination`, `EmptyState`, `LoadingSkeleton`, `Select`                                             |
| 26             | `Table`, `Input`, `StellarAddress`, `CopyToClipboard`, `EmptyState`, `Alert`                                                          |
| 27             | `AmountInput`, `Select`, `Card`, `Alert`                                                                                              |
| 30             | `Card`, `Avatar`, `Badge`, `Tooltip`                                                                                                  |
| 31, 36         | `Badge`, `Card`, `ProgressBar`, `Avatar`                                                                                              |
| 33             | `Card`, `Table`, `Alert`, `CopyToClipboard`, `Badge`                                                                                  |
| 34             | `Card`, `Table`, `Badge`, `Alert`, `Button`                                                                                           |
| 35, 40         | `Card`, `Form`, `Input`, `Textarea`, `Select`, `Alert`, `CopyToClipboard`, `Button`                                                   |
| 37, 38         | `Form`, `Input`, `Button`, `Alert`, `Card`, `Badge`                                                                                   |
| 39             | `Button`, `NetworkBadge`, `Alert`                                                                                                     |
| All            | `ErrorBoundary`, `ToastProvider` / `useToast`, `ThemeProvider`, `useBreakpoint`                                                       |

### Appendix B — Reserved routes beyond the 40

These routes are referenced by the footer, the CTA banners and the reserved-route links, but they are **not** counted in the 40 sections or the 7 pages. Each is a small, mostly-static page that can reuse the global chrome and the token system.

| Route           | Purpose                                                                | Referenced from |
| --------------- | ---------------------------------------------------------------------- | --------------- |
| `/pricing`      | Long-form version of section 34 with an expanded comparison table      | 1, 4, 34        |
| `/changelog`    | Full release history, rendered from `CHANGELOG.md`                     | 4, 38           |
| `/blog`         | Case studies, engineering notes, announcements                         | 4, 30, 38       |
| `/status`       | Live network + API status, ledger close history, incident log          | 4, 28           |
| `/security`     | The long-form version of section 33, mirroring `SECURITY.md`           | 4, 14, 33       |
| `/privacy`      | Data handling: emails, public keys, no secret-key collection           | 4, 37           |
| `/terms`        | Terms of use and the testnet / no-real-funds disclaimer                | 4, 34           |
| `/contributors` | Standalone `CONTRIBUTORS.md` view, linked as `/about#contributors` too | 4, 36           |

### Appendix C — External platform inventory

Every outbound link the landing page needs, grouped by destination type.

**Stellar infrastructure**

| Destination                | URL                                                   | Used by sections      |
| -------------------------- | ----------------------------------------------------- | --------------------- |
| Stellar home               | `https://stellar.org`                                 | 8, 12, 14             |
| Stellar developer docs     | `https://developers.stellar.org`                      | 2, 8, 12, 22, 36      |
| Horizon (testnet)          | `https://horizon-testnet.stellar.org`                 | 3, 14, 26, 28         |
| Horizon (mainnet)          | `https://horizon.stellar.org`                         | 3, 28                 |
| Stellar Expert — testnet   | `https://stellar.expert/explorer/testnet`             | 3, 15, 18, 23, 24, 26 |
| Stellar Expert — mainnet   | `https://stellar.expert/explorer/public`              | 3, 26                 |
| Soroban documentation      | `https://developers.stellar.org/docs/smart-contracts` | 8, 12, 21             |
| SEP-12 (KYC) standard      | `https://stellar.org/protocol/sep-12`                 | 22                    |
| Stellar Developers Discord | `https://discord.gg/stellar`                          | 35, 40                |

**Wallet, SDK and funding**

| Destination                   | URL                                                  | Used by sections |
| ----------------------------- | ---------------------------------------------------- | ---------------- |
| Freighter                     | `https://freighter.app`                              | 2, 7, 12, 35     |
| `@stellar/stellar-sdk` on npm | `https://www.npmjs.com/package/@stellar/stellar-sdk` | 8, 14, 37        |
| Open Collective               | `https://opencollective.com/cooplumen`               | 4, 36            |
| GitHub Sponsors               | via `.github/FUNDING.yml`                            | 36               |

**Project**

| Destination                | URL                                               | Used by sections |
| -------------------------- | ------------------------------------------------- | ---------------- |
| Repository                 | `https://github.com/Tybravo/CoopLumen-Index`      | 2, 4, 31, 36, 38 |
| Issues (good first issues) | repo `/issues`                                    | 31, 36           |
| Discussions                | repo `/discussions`                               | 4, 35, 40        |
| Bug report template        | repo `.github/ISSUE_TEMPLATE/bug_report.yml`      | 35               |
| Feature request template   | repo `.github/ISSUE_TEMPLATE/feature_request.yml` | 35               |
| `CONTRIBUTING.md`          | repo root                                         | 36               |
| `SECURITY.md`              | repo root                                         | 33               |
| `CONTRIBUTORS.md`          | repo root                                         | 36               |
| Security email             | `security@cooplumen.org`                          | 33, 40           |

**Third-party references**

| Destination                       | Purpose                                  | Used by sections |
| --------------------------------- | ---------------------------------------- | ---------------- |
| World Bank (2022) unbanked figure | Citation for 1.4 billion unbanked adults | 10               |
| Proof of Humanity / World ID      | Optional governance identity gate        | 22               |
| Docker / Docker Compose docs      | Self-hosting instructions                | 34, P4           |

---

## Closing note

**What this document is:** a reviewed inventory of **40 landing-page sections** plus **7 pages**, with the colour system, gradients, anchors, CTAs and reuse map needed to build them.

**What this document is not:** a landing page. No components, pages, or stylesheets were created. The only CSS shown is the proposed `--gradient-*` token block in [4.3](#43-gradient-tokens), included so the colour direction can be reviewed precisely.

**Before implementation:** confirm the 40-section list, approve the `--gradient-*` token names and values in 4.3, and decide whether the dark-theme teal rule in 4.5 is accepted as written.
