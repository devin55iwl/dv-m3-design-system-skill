# Material 3 Design System With Google-Inspired Engineering Guidance

Sources:

- Local engineering source: `styleguide-gh-pages/`, a full public `gh-pages`
  snapshot of `google/styleguide`
- Visual design source: Material Design 3, https://m3.material.io/
- Figma source: Material 3 Design Kit Community file,
  `rlipYt5ccndwzYHQLe53PZ`, node `11:1833`

## Table Of Contents

- [Source Classification](#source-classification)
- [Figma Kit Inspection](#figma-kit-inspection)
- [1. Design Principles](#1-design-principles)
- [2. Tokens: Color, Typography, Spacing, Radius, Elevation](#2-tokens-color-typography-spacing-radius-elevation)
- [3. Components and Usage Guidelines](#3-components-and-usage-guidelines)
- [4. Component States and Properties](#4-component-states-and-properties)
- [5. Interaction Patterns](#5-interaction-patterns)
- [6. Layout Rules](#6-layout-rules)
- [7. Accessibility Rules](#7-accessibility-rules)
- [8. Code Implementation Mapping](#8-code-implementation-mapping)
- [9. QA Checklist for Future AI-Generated UI](#9-qa-checklist-for-future-ai-generated-ui)
- [Reference URLs](#reference-urls)

## Task Routing

Read only the sections needed for the task, then return here if another decision
depends on them.

| Task | Read First |
|---|---|
| Rebuild or redesign an existing UI | Sections 1, 2, 3, 4, 6, 7, and 9 |
| `all-pages` migration | Coverage Scope and App-Wide Audit in section 1; sections 6, 7, and 9 |
| Create or repair theme tokens | Section 2; Figma Kit Inspection when a Figma source is supplied |
| Add or refactor a component | Sections 3, 4, 7, and 8 |
| Implement a known user flow | Sections 3, 4, 5, 7, and 8 |
| Verify responsive behavior | Section 6 and Responsive QA in section 9 |
| Translate a Material 3 Figma design to code | Figma Kit Inspection; sections 2, 3, 4, 6, 7, and 8 |
| Write or change code/docs | `google-styleguide-reference.md` plus section 8 |

## Source Classification

This file combines two different public sources:

1. The public `google/styleguide` repository provides an **engineering style
   guide collection**: code style,
   naming, formatting, comments, documentation, language rules, testing, and
   maintainability.
2. Material Design 3 provides a **visual and interaction design system**:
   design tokens, color roles, typography, shape, elevation, motion, layout,
   components, states, accessibility, and implementation guidance.
3. The Figma Material 3 Design Kit provides the **design-library form** of the
   system: pages, variables, paint styles, text styles, effect styles, component
   sets, variants, icons, examples, and platform guidance.

Future AI agents should use Material 3 for how UI looks, behaves, adapts, and
maps to components. Use public `google/styleguide` as Google-inspired,
language-specific guidance for how code and docs are written. It is not a
complete statement of Google's internal engineering practices and must not
override explicit local repository conventions.

### Source Authority And Conflict Resolution

Apply sources in this order:

1. Explicit product requirements, legal/security requirements, and platform
   constraints.
2. Existing repository conventions, component APIs, tests, and formatting
   tools when using `preserve-local`.
3. The selected rebuild mode for visual decisions.
4. Material 3 for visual tokens, components, interaction, adaptive layout, and
   accessibility.
5. The relevant public `google/styleguide` language or documentation guide for
   engineering style.

Do not infer a universal rule from one language guide. For exact engineering
rules, read `google-styleguide-reference.md`, then the matching original source
file. The cross-language rules in this document are intentionally conservative.

## Figma Kit Inspection

The provided Figma file is the Material 3 Design Kit, version `V 1.24`.

Observed kit structure:

| Area | Observed Content |
|---|---|
| Foundation pages | Getting started, Table of contents, Avatars, Icons, Examples, Shape, Styles, Utilities |
| Component pages | App bars, Badges, Buttons, Cards, Carousel, Checkboxes, Chips, Date & time pickers, Dialogs, Dividers, Lists, Loading & progress, Menu, Navigation, Radio button, Search, Sheets, Sliders, Snackbar, Switch, Tabs, Text fields, Toolbars, Tooltips |
| Styles | 727 paint styles, 30 text styles, 10 effect styles |
| Variables | M3, Font theme, Typescale, Shape collections |
| Components | 171 component sets, including many variant-rich sets |

Important kit guidance read from the `Getting started` page:

- The kit is a multi-platform design library for Android, Flutter, Jetpack
  Compose, and Web.
- Material Android is Compose-first.
- Material Views / MDC-Android is in maintenance mode.
- Use the Material Theme Builder to explore dynamic color, build custom themes,
  export code through Material Design tokens, swap selected components to a
  theme, and turn on state layers in settings.
- Use Google Fonts for type and Material Symbols for icons.
- Material Symbols consolidate 2,500+ glyphs and support Fill, Weight, Grade,
  and Optical size axes.

## 1. Design Principles

### Reader and User First

For code, optimize for the future maintainer. For UI, optimize for the person
trying to complete a task. Prefer clear structure, visible affordances, and
predictable behavior over clever implementation or decorative complexity.

### Clarity Over Cleverness

Use simple, understandable code and visible UI states. A generated interface
should make its purpose, available actions, current state, and next step obvious.

### Material as a System

Use Material 3 as a tokenized system, not a set of isolated visual tricks. Map
colors, type, elevation, shape, and state through semantic roles instead of
hardcoded values.

### Consistency Before Novelty

Follow the selected rebuild mode. If the user chooses `preserve-local`, local
product consistency wins and Material 3 repairs or strengthens the existing
system. If the user chooses `material3-overwrite`, Material 3 becomes the target
visual system and local UI styling becomes functional reference only.

### Explicit Mode Selection

When rebuilding, redesigning, refactoring, or visually transforming an existing
page, the agent must use one of two modes:

| Mode | Best For | Rule |
|---|---|---|
| `preserve-local` | Real project maintenance | Keep existing component library, brand tokens, and interaction patterns. Use Material 3 only to fix inconsistencies, add states, improve accessibility, and tighten layout. |
| `material3-overwrite` | Full Material 3 takeover | Replace local visual design with Material 3 tokens, components, spacing, shape, elevation, states, and layout. Preserve only business structure, content hierarchy, and user flow. |

Before implementation, collect both visual mode and coverage scope. If either
is absent, ask the user to type the missing value or values:

```text
Before I start, choose the rebuild mode and coverage scope.

Type one value for each:
- Mode: preserve-local | material3-overwrite
- Scope: current-page | all-pages
```

Do not continue an existing-page rebuild until the user chooses both values.

### Coverage Scope: Current Page Or Whole Application

Visual mode answers *how* to transform the UI. Coverage scope answers *how much*
of the product must be audited and migrated.

| Scope | Use When | Completion Rule |
|---|---|---|
| `all-pages` | User explicitly requests an app-wide audit and migration | Inventory all relevant routes/views and shared UI. Report `completed` only when every route is migrated or explicitly excluded. Any blocked route makes the result `partial`. `unverified` is a temporary audit label, not a completion state. |
| `current-page` | User explicitly limits work to one named route, page, screen, or Figma frame | Deliver only that target and state that no app-wide audit was performed. |

Do not treat a supplied screenshot, route, or Figma frame as permission to skip
the rest of a code project. Ask the user to choose `current-page` or
`all-pages` before implementation. If `all-pages` is selected but no project,
repository, or route inventory is supplied, ask for the project source instead
of silently changing scope.

#### App-Wide Audit And Reuse Procedure

1. Discover route definitions, page/view directories, shells/layouts, shared
   component directories, theme/token sources, UI stories, and test fixtures.
2. Create or update a version-controlled audit file. Prefer the repository's
   existing migration/docs location; otherwise use `docs/ui-migration-audit.md`.
   Verify it is not ignored. Use it as the persistent inventory and include
   route/page, page family, shared dependencies, migration status, verification
   evidence, blockers, and a reason for every exclusion.
3. Group pages by reusable UI family, such as authenticated shell, dashboard,
   list-detail, form flow, settings, data table, modal flow, and marketing
   surface.
4. Upgrade tokens and shared foundations before individual pages: color/type/
   shape/elevation tokens, layout primitives, navigation, buttons, inputs,
   feedback, and state behavior.
5. Migrate one representative page per family to expose missing component
   variants; fix those variants in the shared layer, then migrate the remaining
   pages through reuse.
6. Before testing an authenticated route, inspect the project for an authorized
   test account, local mock or seed flow, saved test session, or documented E2E
   authentication setup. If none is available, ask the user:

   ```text
   This route needs login verification. Please provide an authorized
   test-environment login method: a test account, temporary password, existing
   browser session, or local mock/seed setup. Do not provide production
   credentials, real-user accounts, or secret keys.
   ```

   Do not store credentials in source code, the audit file, Git, screenshots,
   logs, or the final response. Mark the route `blocked` only if authorized test
   access is unavailable.
7. Inspect available scripts and run the application when possible. Use browser
   automation to check every affected route at the required breakpoint matrix;
   capture screenshots and overflow results as evidence. If a route cannot be
   run or verified, mark it as `blocked` with its reason. Do not leave a route as
   `unverified` in a completion report.
8. Report `completed` only when every discovered relevant page is migrated or
   intentionally excluded. If any route is `blocked`, report `partial` and list
   every blocker.

Update the audit file whenever the audit, migration status, verification
evidence, or blocker changes. Keep it version-controlled for cross-machine
handoff. Use `.codex/` only when the user explicitly requests a local-only
audit. Summarize the audit in the final response so the current user sees the
outcome and a later agent can resume from the file.

Use this minimal structure:

```markdown
# UI Migration Audit

| Route or page | UI family | Shared dependencies | Status | Evidence or blocker |
|---|---|---|---|---|
| `/settings` | Settings form | AppShell, TextField, Button | migrated | Browser matrix 599/600/839/840/1199/1200/1599/1600 passed |
```

Allowed route statuses are `migrated`, `excluded`, and `blocked`. Use
`unverified` only while work is in progress; convert it before reporting the
task outcome. The task outcome is `completed` only with no blocked routes;
otherwise it is `partial`.

### Adaptive and Accessible

Design across compact, medium, expanded, large, and extra-large layouts. Preserve
keyboard, screen reader, contrast, text resizing, and target-size accessibility.

### Maintainable Implementation

Keep UI code readable, typed, and componentized only where useful. Avoid
unnecessary wrappers, magic numbers, and one-off visual styles that bypass the
theme.

## 2. Tokens: Color, Typography, Spacing, Radius, Elevation

Use tokens instead of hardcoded visual values. Material 3 design tokens represent
reusable decisions for colors, fonts, measurements, elevation, and shape.

The Figma kit represents these decisions through variables and styles:

| Figma Collection / Style | Observed Details | AI Rule |
|---|---|---|
| `M3` variable collection | 197 variables across Light, Dark, High Contrast, Medium Contrast, Monochrome, and multiple hue themes | Bind colors to `M3/sys/*` roles, not raw fills. |
| `Font theme` variable collection | Baseline and Wireframe modes | Use for swapping font treatment without rewriting components. |
| `Typescale` variable collection | 90 variables | Use type variables/styles for all Material text. |
| `Shape` variable collection | 10 variables | Use shape variables for corner radius and component shape. |
| Paint styles | 727 local paint styles | Prefer style names such as `M3/sys/light/primary`. |
| Text styles | 30 local text styles | Prefer `M3/display/*`, `M3/headline/*`, `M3/title/*`, `M3/body/*`, `M3/label/*`. |
| Effect styles | 10 local effects | Prefer `M3/Elevation Light/1-5` and `M3/Elevation Dark/1-5`. |

### Color Tokens

Use Material 3 semantic color roles. Do not create component-specific colors
until the system roles are insufficient.

| Role Group | Use For |
|---|---|
| Primary | Highest-emphasis actions, active states, key CTAs, prominent controls. |
| Secondary | Important but less dominant actions and supporting emphasis. |
| Tertiary | Contrasting accents, expressive moments, secondary highlights. |
| Error | Destructive, invalid, failed, or dangerous states. |
| Surface | App backgrounds, containers, cards, sheets, dialogs, bars. |
| Outline | Borders, dividers, disabled outlines, low-emphasis separation. |

Use paired `on-*` roles for text and icons placed on a role:

- `primary` with `on-primary`
- `primary-container` with `on-primary-container`
- `surface` with `on-surface`
- `surface-variant` with `on-surface-variant`
- `error` with `on-error`

Prefer role pairs over arbitrary contrast fixes.

Figma kit color style examples:

- `M3/sys/light/primary`
- `M3/sys/light/on-primary`
- `M3/sys/light/primary-container`
- `M3/sys/light/on-primary-container`
- `M3/sys/light/surface`
- `M3/sys/light/on-surface`
- `M3/sys/light/surface-container-lowest`
- `M3/sys/light/surface-container-low`
- `M3/sys/light/surface-container`
- `M3/sys/light/surface-container-high`
- `M3/sys/light/surface-container-highest`
- Matching `M3/sys/dark/*` roles
- `M3/ref/primary/primary0-100`, `M3/ref/secondary/*`,
  `M3/ref/tertiary/*`, `M3/ref/error/*`, `M3/ref/neutral/*`

### Baseline Material 3 System Colors

These baseline colors were extracted from the Material 3 Figma Design Kit paint
styles. Use these as default Material 3 color values when no product-specific
theme is supplied. If the user provides a brand theme or generated dynamic color
scheme, prefer that theme while preserving the same semantic role names.

#### Light Scheme

| Role | Figma Style | Hex |
|---|---|---|
| primary | `M3/sys/light/primary` | `#6750A4` |
| on-primary | `M3/sys/light/on-primary` | `#FFFFFF` |
| primary-container | `M3/sys/light/primary-container` | `#EADDFF` |
| on-primary-container | `M3/sys/light/on-primary-container` | `#4F378A` |
| primary-fixed | `M3/sys/light/primary-fixed` | `#EADDFF` |
| on-primary-fixed | `M3/sys/light/on-primary-fixed` | `#21005D` |
| primary-fixed-dim | `M3/sys/light/primary-fixed-dim` | `#D0BCFF` |
| on-primary-fixed-variant | `M3/sys/light/on-primary-fixed-variant` | `#4F378B` |
| secondary | `M3/sys/light/secondary` | `#625B71` |
| on-secondary | `M3/sys/light/on-secondary` | `#FFFFFF` |
| secondary-container | `M3/sys/light/secondary-container` | `#E8DEF8` |
| on-secondary-container | `M3/sys/light/on-secondary-container` | `#4A4459` |
| secondary-fixed | `M3/sys/light/secondary-fixed` | `#E8DEF8` |
| on-secondary-fixed | `M3/sys/light/on-secondary-fixed` | `#1D192B` |
| secondary-fixed-dim | `M3/sys/light/secondary-fixed-dim` | `#CCC2DC` |
| on-secondary-fixed-variant | `M3/sys/light/on-secondary-fixed-variant` | `#4A4458` |
| tertiary | `M3/sys/light/tertiary` | `#7D5260` |
| on-tertiary | `M3/sys/light/on-tertiary` | `#FFFFFF` |
| tertiary-container | `M3/sys/light/tertiary-container` | `#FFD8E4` |
| on-tertiary-container | `M3/sys/light/on-tertiary-container` | `#633B48` |
| tertiary-fixed | `M3/sys/light/tertiary-fixed` | `#FFD8E4` |
| on-tertiary-fixed | `M3/sys/light/on-tertiary-fixed` | `#31111D` |
| tertiary-fixed-dim | `M3/sys/light/tertiary-fixed-dim` | `#EFB8C8` |
| on-tertiary-fixed-variant | `M3/sys/light/on-tertiary-fixed-variant` | `#633B48` |
| error | `M3/sys/light/error` | `#B3261E` |
| on-error | `M3/sys/light/on-error` | `#FFFFFF` |
| error-container | `M3/sys/light/error-container` | `#F9DEDC` |
| on-error-container | `M3/sys/light/on-error-container` | `#852221` |
| outline | `M3/sys/light/outline` | `#79747E` |
| outline-variant | `M3/sys/light/outline-variant` | `#CAC4D0` |
| background | `M3/sys/light/background` | `#FEF7FF` |
| on-background | `M3/sys/light/on-background` | `#1D1B20` |
| surface | `M3/sys/light/surface` | `#FEF7FF` |
| on-surface | `M3/sys/light/on-surface` | `#1D1B20` |
| surface-variant | `M3/sys/light/surface-variant` | `#E7E0EC` |
| on-surface-variant | `M3/sys/light/on-surface-variant` | `#49454F` |
| inverse-surface | `M3/sys/light/inverse-surface` | `#322F35` |
| inverse-on-surface | `M3/sys/light/inverse-on-surface` | `#F5EFF7` |
| inverse-primary | `M3/sys/light/inverse-primary` | `#D0BCFF` |
| shadow | `M3/sys/light/shadow` | `#000000` |
| surface-tint | `M3/sys/light/surface-tint` | `#6750A4` |
| scrim | `M3/sys/light/scrim` | `#000000` |
| surface-container-highest | `M3/sys/light/surface-container-highest` | `#E6E0E9` |
| surface-container-high | `M3/sys/light/surface-container-high` | `#ECE6F0` |
| surface-container | `M3/sys/light/surface-container` | `#F3EDF7` |
| surface-container-low | `M3/sys/light/surface-container-low` | `#F7F2FA` |
| surface-container-lowest | `M3/sys/light/surface-container-lowest` | `#FFFFFF` |
| surface-bright | `M3/sys/light/surface-bright` | `#FEF7FF` |
| surface-dim | `M3/sys/light/surface-dim` | `#DED8E1` |

#### Dark Scheme

| Role | Figma Style | Hex |
|---|---|---|
| primary | `M3/sys/dark/primary` | `#D0BCFE` |
| on-primary | `M3/sys/dark/on-primary` | `#381E72` |
| primary-container | `M3/sys/dark/primary-container` | `#4F378B` |
| on-primary-container | `M3/sys/dark/on-primary-container` | `#EADDFF` |
| primary-fixed | `M3/sys/dark/primary-fixed` | `#EADDFF` |
| on-primary-fixed | `M3/sys/dark/on-primary-fixed` | `#21005D` |
| primary-fixed-dim | `M3/sys/dark/primary-fixed-dim` | `#D0BCFF` |
| on-primary-fixed-variant | `M3/sys/dark/on-primary-fixed-variant` | `#4F378B` |
| secondary | `M3/sys/dark/secondary` | `#CCC2DC` |
| on-secondary | `M3/sys/dark/on-secondary` | `#332D41` |
| secondary-container | `M3/sys/dark/secondary-container` | `#4A4458` |
| on-secondary-container | `M3/sys/dark/on-secondary-container` | `#E8DEF8` |
| secondary-fixed | `M3/sys/dark/secondary-fixed` | `#E8DEF8` |
| on-secondary-fixed | `M3/sys/dark/on-secondary-fixed` | `#1D192B` |
| secondary-fixed-dim | `M3/sys/dark/secondary-fixed-dim` | `#CCC2DC` |
| on-secondary-fixed-variant | `M3/sys/dark/on-secondary-fixed-variant` | `#4A4458` |
| tertiary | `M3/sys/dark/tertiary` | `#EFB8C8` |
| on-tertiary | `M3/sys/dark/on-tertiary` | `#492532` |
| tertiary-container | `M3/sys/dark/tertiary-container` | `#633B48` |
| on-tertiary-container | `M3/sys/dark/on-tertiary-container` | `#FFD8E4` |
| tertiary-fixed | `M3/sys/dark/tertiary-fixed` | `#FFD8E4` |
| on-tertiary-fixed | `M3/sys/dark/on-tertiary-fixed` | `#31111D` |
| tertiary-fixed-dim | `M3/sys/dark/tertiary-fixed-dim` | `#EFB8C8` |
| on-tertiary-fixed-variant | `M3/sys/dark/on-tertiary-fixed-variant` | `#633B48` |
| error | `M3/sys/dark/error` | `#F2B8B5` |
| on-error | `M3/sys/dark/on-error` | `#601410` |
| error-container | `M3/sys/dark/error-container` | `#8C1D18` |
| on-error-container | `M3/sys/dark/on-error-container` | `#F9DEDC` |
| outline | `M3/sys/dark/outline` | `#938F99` |
| outline-variant | `M3/sys/dark/outline-variant` | `#49454F` |
| background | `M3/sys/dark/background` | `#141218` |
| on-background | `M3/sys/dark/on-background` | `#E6E0E9` |
| surface | `M3/sys/dark/surface` | `#141218` |
| on-surface | `M3/sys/dark/on-surface` | `#E6E0E9` |
| surface-variant | `M3/sys/dark/surface-variant` | `#49454F` |
| on-surface-variant | `M3/sys/dark/on-surface-variant` | `#CAC4D0` |
| inverse-surface | `M3/sys/dark/inverse-surface` | `#E6E0E9` |
| inverse-on-surface | `M3/sys/dark/inverse-on-surface` | `#322F35` |
| inverse-primary | `M3/sys/dark/inverse-primary` | `#6750A4` |
| shadow | `M3/sys/dark/shadow` | `#000000` |
| surface-tint | `M3/sys/dark/surface-tint` | `#D0BCFF` |
| scrim | `M3/sys/dark/scrim` | `#000000` |
| surface-container-highest | `M3/sys/dark/surface-container-highest` | `#36343B` |
| surface-container-high | `M3/sys/dark/surface-container-high` | `#2B2930` |
| surface-container | `M3/sys/dark/surface-container` | `#211F26` |
| surface-container-low | `M3/sys/dark/surface-container-low` | `#1D1B20` |
| surface-container-lowest | `M3/sys/dark/surface-container-lowest` | `#0F0D13` |
| surface-bright | `M3/sys/dark/surface-bright` | `#3B383E` |
| surface-dim | `M3/sys/dark/surface-dim` | `#141218` |

### Typography Tokens

Use the Material 3 type scale roles:

| Role | Use For |
|---|---|
| Display | Rare, large expressive text. |
| Headline | Screen and section headlines. |
| Title | Component titles, app bars, dialogs, compact section labels. |
| Body | Reading text, descriptions, paragraphs, form helper text. |
| Label | Buttons, tabs, chips, badges, field labels, small action text. |

Each role can have large, medium, and small variants. Choose by hierarchy and
container size, not by viewport width alone.

Figma kit text style names:

- `M3/display/large`, `M3/display/medium`, `M3/display/small`
- `M3/headline/large`, `M3/headline/medium`, `M3/headline/small`
- `M3/title/large`, `M3/title/medium`, `M3/title/small`
- `M3/label/large`, `M3/label/medium`, `M3/label/small`
- `M3/body/large`, `M3/body/medium`, `M3/body/small`
- Emphasized variants exist for each role and size, for example
  `M3/title/medium-emphasized`

#### Baseline Material 3 Typography Values

These values were extracted from the Material 3 Figma Design Kit text styles.
Use them as the default type scale when no product-specific typography theme is
provided.

| Style | Font | Weight | Size | Line Height | Tracking |
|---|---|---:|---:|---:|---:|
| `M3/display/large` | Roboto Regular | 400 | 57px | 64px | -0.25px |
| `M3/display/large-emphasized` | Roboto Medium | 500 | 57px | 64px | -0.25px |
| `M3/display/medium` | Roboto Regular | 400 | 45px | 52px | 0px |
| `M3/display/medium-emphasized` | Roboto Medium | 500 | 45px | 52px | 0px |
| `M3/display/small` | Roboto Regular | 400 | 36px | 44px | 0px |
| `M3/display/small-emphasized` | Roboto Medium | 500 | 36px | 44px | 0px |
| `M3/headline/large` | Roboto Regular | 400 | 32px | 40px | 0px |
| `M3/headline/large-emphasized` | Roboto Medium | 500 | 32px | 40px | 0px |
| `M3/headline/medium` | Roboto Regular | 400 | 28px | 36px | 0px |
| `M3/headline/medium-emphasized` | Roboto Medium | 500 | 28px | 36px | 0px |
| `M3/headline/small` | Roboto Regular | 400 | 24px | 32px | 0px |
| `M3/headline/small-emphasized` | Roboto Medium | 500 | 24px | 32px | 0px |
| `M3/title/large` | Roboto Regular | 400 | 22px | 28px | 0px |
| `M3/title/large-emphasized` | Roboto Medium | 500 | 22px | 28px | 0px |
| `M3/title/medium` | Roboto Medium | 500 | 16px | 24px | 0.15px |
| `M3/title/medium-emphasized` | Roboto SemiBold | 600 | 16px | 24px | 0.15px |
| `M3/title/small` | Roboto Medium | 500 | 14px | 20px | 0.10px |
| `M3/title/small-emphasized` | Roboto SemiBold | 600 | 14px | 20px | 0.10px |
| `M3/body/large` | Roboto Regular | 400 | 16px | 24px | 0.5px |
| `M3/body/large-emphasized` | Roboto Medium | 500 | 16px | 24px | 0.5px |
| `M3/body/medium` | Roboto Regular | 400 | 14px | 20px | 0.25px |
| `M3/body/medium-emphasized` | Roboto Medium | 500 | 14px | 20px | 0.25px |
| `M3/body/small` | Roboto Regular | 400 | 12px | 16px | 0.4px |
| `M3/body/small-emphasized` | Roboto Medium | 500 | 12px | 16px | 0.4px |
| `M3/label/large` | Roboto Medium | 500 | 14px | 20px | 0.10px |
| `M3/label/large-emphasized` | Roboto SemiBold | 600 | 14px | 20px | 0.10px |
| `M3/label/medium` | Roboto Medium | 500 | 12px | 16px | 0.5px |
| `M3/label/medium-emphasized` | Roboto SemiBold | 600 | 12px | 16px | 0.5px |
| `M3/label/small` | Roboto Medium | 500 | 11px | 16px | 0.5px |
| `M3/label/small-emphasized` | Roboto SemiBold | 600 | 11px | 16px | 0.5px |

### Spacing Tokens

Material 3 uses adaptive layout and spacing rather than a single fixed spacing
scale in this file.

The inspected Figma kit did not expose a dedicated `Spacing` variable collection
alongside `M3`, `Font theme`, `Typescale`, and `Shape`.

AI rule:

- Use existing project spacing tokens first.
- If none exist, use an 8dp-compatible rhythm for layout spacing.
- Use tighter spacing on compact screens and more generous spacing on desktop.
- Keep related controls visually grouped.
- Do not use spacing only as decoration; spacing should clarify structure.

### Radius / Shape Tokens

Use Material shape as a semantic signal. Shape can communicate emphasis, state,
brand expression, and grouping.

Recommended token names for AI output:

| Token | Use For |
|---|---|
| `shape-none` | Flush or full-bleed surfaces. |
| `shape-extra-small` | Dense controls, small containers. |
| `shape-small` | Inputs, chips, compact controls. |
| `shape-medium` | Cards, menus, standard containers. |
| `shape-large` | Dialogs, sheets, large containers. |
| `shape-extra-large` | Expressive hero or prominent surfaces. |
| `shape-full` | Pills, FABs, circular controls, avatars. |

Follow component defaults when using a Material implementation library.

The Figma kit includes a `Shape` variable collection and a `Shape Set` component
set. When translating from Figma, preserve bound shape variables and shape-set
intent rather than flattening every radius to a number.

#### Baseline Material 3 Shape Values

These values were extracted from the Figma kit `Shape` variable collection.

| Figma Variable | Suggested Token | Radius |
|---|---|---:|
| `Corner/None` | `shape-none` | 0px |
| `Corner/Extra-small` | `shape-extra-small` | 4px |
| `Corner/Small` | `shape-small` | 8px |
| `Corner/Medium` | `shape-medium` | 12px |
| `Corner/Large` | `shape-large` | 16px |
| `Corner/Large-increased` | `shape-large-increased` | 20px |
| `Corner/Extra-large` | `shape-extra-large` | 28px |
| `Corner/Extra-large-increased` | `shape-extra-large-increased` | 32px |
| `Corner/Extra-extra-large` | `shape-extra-extra-large` | 48px |
| `Corner/Full` | `shape-full` | 1000px |

### Elevation Tokens

Use Material elevation levels `0` through `5`. Elevation represents relative
distance between surfaces and may be shown through tonal color, shadow, or both.

| Level | Use For |
|---|---|
| `level0` | Base surfaces and flat content. |
| `level1` | Slightly raised containers, low-emphasis cards. |
| `level2` | Raised controls, hover/interactive containers. |
| `level3` | Prominent cards, menus, app bars, active surfaces. |
| `level4` | High-emphasis overlays or raised transient surfaces. |
| `level5` | Highest overlay separation when needed. |

Avoid stacking many elevated surfaces. Elevation should clarify hierarchy, not
create visual noise.

The Figma kit includes elevation effect styles:

- `M3/Elevation Light/1` through `M3/Elevation Light/5`
- `M3/Elevation Dark/1` through `M3/Elevation Dark/5`

#### Baseline Material 3 Elevation Effects

These values were extracted from the Figma kit effect styles. Each elevation is
represented by two drop shadows. Use `level0` as no shadow.

| Style | Shadow 1 | Shadow 2 |
|---|---|---|
| `M3/Elevation Light/1` | `0 1px 3px 1px rgba(0,0,0,0.15)` | `0 1px 2px 0 rgba(0,0,0,0.30)` |
| `M3/Elevation Light/2` | `0 2px 6px 2px rgba(0,0,0,0.15)` | `0 1px 2px 0 rgba(0,0,0,0.30)` |
| `M3/Elevation Light/3` | `0 1px 3px 0 rgba(0,0,0,0.30)` | `0 4px 8px 3px rgba(0,0,0,0.15)` |
| `M3/Elevation Light/4` | `0 2px 3px 0 rgba(0,0,0,0.30)` | `0 6px 10px 4px rgba(0,0,0,0.15)` |
| `M3/Elevation Light/5` | `0 4px 4px 0 rgba(0,0,0,0.30)` | `0 8px 12px 6px rgba(0,0,0,0.15)` |
| `M3/Elevation Dark/1` | `0 1px 2px 0 rgba(0,0,0,0.30)` | `0 1px 3px 1px rgba(0,0,0,0.15)` |
| `M3/Elevation Dark/2` | `0 1px 2px 0 rgba(0,0,0,0.30)` | `0 2px 6px 2px rgba(0,0,0,0.15)` |
| `M3/Elevation Dark/3` | `0 1px 3px 0 rgba(0,0,0,0.30)` | `0 4px 8px 3px rgba(0,0,0,0.15)` |
| `M3/Elevation Dark/4` | `0 2px 3px 0 rgba(0,0,0,0.30)` | `0 6px 10px 4px rgba(0,0,0,0.15)` |
| `M3/Elevation Dark/5` | `0 4px 4px 0 rgba(0,0,0,0.30)` | `0 8px 12px 6px rgba(0,0,0,0.15)` |

### Motion Tokens

Use Material motion to communicate continuity, feedback, and spatial change.

AI rule:

- Use motion for state change, navigation, container transform, expansion,
  selection, and feedback.
- Respect reduced-motion preferences.
- Keep transitions fast enough to preserve task flow.
- Do not animate layout in a way that blocks interaction or causes disorientation.

## 3. Components and Usage Guidelines

Material 3 includes 30+ UI components. Use native Material implementations where
available before hand-building.

The provided Figma kit should be treated as the component-library source of
truth when available. Prefer instances from its component sets over drawing
custom approximations.

### Action Components

| Component | Usage Guideline |
|---|---|
| Button | Use for actions. Choose filled for primary action, tonal for medium emphasis, outlined for secondary, text for low emphasis, elevated when separation is needed. |
| Floating action button | Use for the most important screen-level action. Avoid multiple FABs on one screen. |
| Icon button | Use for compact actions with recognizable icons. Provide accessible labels. |
| Segmented button | Use for mutually exclusive choices or multi-select controls in compact horizontal groups. |
| Split button | Use when one action has a primary default plus related alternatives. |

Observed Figma component families include `Button`, `Button - text`,
`Button - elevated`, `Button - outline`, `Button - tonal`, `Toggle button`,
`Icon button`, `Icon button togglable`, `Split button`, `FAB`,
`Extended FAB`, `FAB menu`, `Connected button group`, and `Standard button group`.

### Input and Selection Components

| Component | Usage Guideline |
|---|---|
| Text field | Use for typed input in forms, dialogs, search, and settings. Always provide label and error behavior. |
| Checkbox | Use for independent boolean or multi-select choices. |
| Radio button | Use for one choice from a visible set. |
| Switch | Use for immediate on/off settings. |
| Slider | Use for selecting from a numeric range. |
| Chips | Use for filters, input tokens, suggestions, and compact actions. |
| Date/time picker | Use for constrained date or time selection. |

Observed Figma component families include `Text field`, `Checkboxes`, `Radio
buttons`, `Switch`, `Standard slider`, `Centered slider`, `Range slider`,
`Suggestion chip`, `Filter chip`, `Assistive chip`, `Input chip`, `Chip groups`,
`Input date picker`, `Modal date picker`, `Docked input date picker [desktop]`,
`Dial picker`, and `Keyboard picker`.

### Container and Feedback Components

| Component | Usage Guideline |
|---|---|
| Card | Use to group related content and actions. Avoid putting page sections in decorative cards by default. |
| Dialog | Use for important prompts requiring user decision or acknowledgement. Keep focused. |
| Bottom sheet | Use for contextual actions, details, or workflows emerging from the bottom edge. |
| Snackbar | Use for brief feedback and optional undo. Do not use for critical blocking information. |
| Tooltip | Use for short explanation of icon-only or unfamiliar controls. |
| Progress indicator | Use for loading or ongoing work. Prefer determinate progress when possible. |

Observed Figma component families include `Stacked card`, `Horizontal card`,
`Basic dialog`, `List dialog`, `Scrollable list dialog`, `Bottom sheet`,
`Side Sheet`, `Snackbar`, `Plain Tooltip`, `Carousel`, `Carousel - Full screen`,
`Linear-determinate progress indicator`, `Linear-indeterminate progress indicator`,
`Circular-determinate progress indicator`, `Circular-indeterminate progress indicator`,
and `Loading indicator`.

### Navigation and Structure Components

| Component | Usage Guideline |
|---|---|
| Top app bar | Use for screen identity, navigation, and high-level actions. |
| Navigation bar | Use on compact screens for 3-5 top-level destinations. |
| Navigation rail | Use on medium/expanded layouts for persistent primary navigation. |
| Navigation drawer | Use for broader destination sets or app-level navigation. |
| Tabs | Use to switch between peer content views. |
| Lists | Use for continuous vertical indexes of text and media. |
| Divider | Use for separation only when spacing and hierarchy are insufficient. |
| Menu | Use for contextual actions or compact option sets. |
| Search | Use search bar/view patterns for finding content or commands. |

Observed Figma component families include `App bar`, `Bottom app bar`,
`Navigation Bar`, `Navigation Rail`, `Navigation Rail: Expanded`, `Tabs`,
`Primary tabs`, `Secondary tabs`, `Search bar`, `Search full-screen layout`,
`Search docked layout`, `Menu`, `Menu item`, `List`, `List item`, `Toolbar`,
and `Tooltips`.

### Icons and Symbols

Use Material Symbols for icons. The Figma kit includes common icon components
such as `menu`, `arrow_back`, `arrow_forward`, `check`, `close`, `add`,
`more_vert`, `search`, `delete`, `settings`, `edit`, `share`, `download`,
`upload`, and many others. When translating from Figma to code, preserve the
symbol name and map it to the platform's Material Symbols implementation.

## 4. Component States and Properties

Use explicit states. Do not rely on color alone.

### Required Interactive States

| State | AI Rule |
|---|---|
| Enabled | Default interactive state. |
| Hover | Desktop pointer feedback using a state layer. |
| Focus | Keyboard and assistive-tech focus must be visible. |
| Pressed | Immediate touch/click feedback. |
| Dragged | Used for drag surfaces and reorderable items. |
| Disabled | Non-interactive; cannot be focused, dragged, pressed, tapped, or hovered. |
| Selected | Use for chosen navigation, filters, options, chips, tabs, rows. |
| Error | Use visible message, color role, and recovery guidance. |
| Loading | Preserve layout and communicate whether action is in progress. |
| Empty | Explain absence of data and offer next action when useful. |

### Material State Layers

Use state layers for interactive feedback:

| State Layer | Opacity |
|---|---:|
| Hover | 8% |
| Focus | 10% |
| Pressed | 10% |
| Dragged | 16% |

State layers should use the relevant content color role, usually an `on-*` role
or component foreground color.

The Figma kit includes state-layer paint styles using names like
`M3/state-layers/light/primary/opacity-0.08` and
`M3/state-layers/light/primary/opacity-0.10`. If the kit state layer is present,
use it directly instead of manually mixing colors.

### Component Properties

AI-generated component APIs should expose:

- `variant`: visual configuration, such as filled, outlined, tonal, text.
- `size`: compact, default, large, if the component supports it.
- `state`: disabled, loading, selected, error.
- `leadingIcon` / `trailingIcon`: where relevant.
- `label` or accessible name.
- `supportingText` or helper/error text for form components.
- `onClick`, `onChange`, `onDismiss`, or semantic event handlers.

## 5. Interaction Patterns

### Search

Use search bars or search views. Provide placeholder text, keyboard submit,
clear action, loading state, empty state, and result count when useful.

### Forms

Use labeled fields, helper text, validation text, clear grouping, and predictable
submit placement. Disable submit only when the reason is obvious, or show inline
validation guidance.

### Approval / Confirmation

Use dialogs for decisions that block progress or carry risk. Keep title,
supporting text, and actions clear. Primary action should match the user's
intended outcome, not a vague "OK".

### Delete / Destructive Flow

Use `error` roles for destructive emphasis. Prefer undo for low-risk deletes and
confirmation for high-risk or irreversible deletes.

### Navigation

Use navigation bar for compact top-level destinations, navigation rail for
medium/expanded layouts, and drawer for larger destination sets. Keep selected
destination visible.

### Feedback

Use snackbars for brief feedback, progress indicators for work in progress,
inline errors for form correction, and dialogs only for blocking or important
interruptions.

### Engineering Pattern Overlay

When implementing any UI pattern, apply Google-inspired, public-styleguide
principles:

- Use meaningful component and prop names.
- Keep logic readable.
- Keep state explicit.
- Avoid unrelated refactors.
- Add or update tests based on risk.

## 6. Layout Rules

### Adaptive Breakpoints

Use Material 3 adaptive breakpoints:

| Breakpoint | Width |
|---|---:|
| Compact | `< 600dp` |
| Medium | `600-839dp` |
| Expanded | `840-1199dp` |
| Large | `1200-1599dp` |
| Extra large | `>= 1600dp` |

### Canonical Layouts

Use Material canonical layouts when appropriate:

| Layout | Use For |
|---|---|
| Feed | Browsing repeated content or cards. |
| List-detail | Selecting an item and viewing details. |
| Supporting pane | Main task plus contextual supporting content. |

### Scaffold

Structure adaptive layouts with bars, rails, drawers, panes, and content areas.
Navigation should adapt across breakpoints rather than simply scaling down.

The Figma kit includes an `Examples/Layout grid` component set with 11 variants.
Use kit layout-grid examples to align adaptive layouts before inventing custom
grid behavior.

### Grid and Spacing

Use responsive grids that adapt across breakpoints. Increase whitespace and pane
structure on larger screens. On compact screens, prioritize task order and
single-column readability.

### AI Layout Checks

- Text must not overflow or overlap.
- Controls must preserve stable dimensions across states.
- Primary action must be easy to find.
- Content hierarchy must be scannable.
- Dense operational tools should stay compact and work-focused.

### Breakpoint Matrix Validation

Do not validate only a compact/mobile endpoint and a wide desktop endpoint.
For every responsive page, validate the full Material 3 breakpoint matrix. For
web, use these exact CSS-pixel viewport widths; for native platforms, use the
equivalent `dp` window sizes:

| Layout class | Exact web widths | Boundary emphasis |
|---|---:|---|
| Compact | `599` | Just below `600` |
| Medium | `600`, `839` | At `600` and just below `840` |
| Expanded | `840`, `1199` | At `840` and just below `1200` |
| Large | `1200`, `1599` | At `1200` and just below `1600` |
| Extra large | `1600` | At `1600` |

At each class, validate the default page state. Also validate every meaningful
anchor or state for pages with sticky navigation, anchored sections, horizontal
workflow diagrams, tables, navigation rails, bottom navigation, drawers, or
other breakpoint-dependent controls.

For runnable web projects, use browser automation for the matrix. Capture a
screenshot and record document overflow results for each route, viewport, and
relevant state in the migration audit. If the application cannot run, mark the
affected route as `blocked`; do not replace browser evidence with a CSS-only
claim.

Fail responsive validation when any of these are true:

- `document.documentElement.scrollWidth > window.innerWidth` because of
  unintended document-level horizontal overflow.
- `window.scrollX !== 0` after resetting horizontal position for the check.
- Visible content is clipped outside the viewport.
- A navigation, sticky region, anchor target, or interactive control becomes
  unreachable, overlapped, or changes meaning at a breakpoint.

An intentional nested horizontal scroller, such as a data table or diagram, is
permitted only when it does not create page-level horizontal overflow and has an
accessible, discoverable scrolling path.

## 7. Accessibility Rules

Use Material accessibility guidance plus platform standards.

### Structure and Navigation

- Use semantic HTML or native platform semantics.
- Provide screen-reader structure matching visual structure.
- Make keyboard goals achievable through Tab, arrow keys, and common shortcuts.
- Keep focus order logical.
- Ensure all actionable elements receive keyboard and screen-reader focus.

### Labels and Text

- Provide accessible labels for icon-only controls.
- Labels should describe the action or value, not the visual appearance.
- Support text resizing; when OS text scaling is unavailable, offer multipliers
  such as 1.5x or 2x.
- Keep essential information in text, not image-only or color-only form.

### Contrast and Targets

- Use Material role pairs for readable foreground/background combinations.
- Non-text UI elements such as button containers should maintain at least 3:1
  contrast against their background where applicable.
- Use recommended touch target sizes of roughly 7-10mm or larger.
- Do not rely only on hover, color, or animation to communicate state.

### Motion

- Respect reduced-motion settings.
- Avoid excessive parallax, flashing, or motion that blocks task completion.

## 8. Code Implementation Mapping

### Web

Preferred options:

- Use the host project's existing Material-compatible library when present.
- For Web Components, `@material/web` is in maintenance mode. Verify the
  installed version and official documentation for every required component or
  Material 3 Expressive feature; do not assume complete Expressive parity.
- For React, use the project's existing component system or a Material-compatible
  React library if already adopted.

AI implementation rules:

- Map tokens to CSS custom properties or theme objects.
- Use semantic token names, not raw hex values in components.
- Keep components accessible by default.
- Keep variants explicit in props.

### Figma-to-Code Mapping

When a design is sourced from the Material 3 Figma Kit:

| Figma Source | Code Target |
|---|---|
| `M3/sys/light/*`, `M3/sys/dark/*` paint styles | Theme color roles / CSS variables / `ColorScheme` |
| `M3/ref/*` paint styles | Reference palettes, not direct component colors |
| `M3/state-layers/*` paint styles | State-layer overlays or component state tokens |
| `M3/display/*`, `M3/headline/*`, `M3/title/*`, `M3/body/*`, `M3/label/*` | Typography theme / text style tokens |
| `M3/Elevation Light/*`, `M3/Elevation Dark/*` | Elevation shadow/effect tokens |
| `Shape` variables and `Shape Set` | Shape scale / component radius tokens |
| Component set variant properties | Component props such as variant, state, size, icon, selected |
| Material Symbol component names | Material Symbols icon names in code |

Do not copy Figma-only layer names such as `.Building Blocks/*` into public code
APIs. Treat those as internal composition details unless the production design
system intentionally exposes them.

Example CSS token shape:

```css
:root {
  --md-sys-color-primary: /* theme value */;
  --md-sys-color-on-primary: /* theme value */;
  --md-sys-color-surface: /* theme value */;
  --md-sys-color-on-surface: /* theme value */;
  --md-sys-shape-medium: 12px;
  --md-sys-elevation-level1: /* tonal/shadow value */;
}
```

### Android Jetpack Compose

Use Material 3 through `MaterialTheme`. The inspected Figma kit explicitly says
Material Android is Compose-first and that the Material Views / MDC-Android
library is in maintenance mode.

```kotlin
MaterialTheme(
    colorScheme = appColorScheme,
    typography = appTypography,
    shapes = appShapes,
) {
    AppContent()
}
```

Map UI to Material 3 composables such as `Button`, `TextField`, `Card`,
`Dialog`, `NavigationBar`, `NavigationRail`, `Scaffold`, and `SnackbarHost`
where appropriate.

### Flutter

Use Flutter's Material 3 support through theme configuration:

```dart
MaterialApp(
  theme: ThemeData(
    useMaterial3: true,
    colorScheme: colorScheme,
  ),
  home: const App(),
)
```

Map design tokens through `ColorScheme`, `TextTheme`, `ShapeBorder`, and
component themes.

### Google-Inspired Engineering Mapping

Use `google-styleguide-reference.md` to select the matching public source. Apply
local repository conventions first, then the exact language guide where it is
relevant:

| Source Area | Implementation Mapping |
|---|---|
| Python | Imports, docstrings, type annotations, resource handling, naming. |
| Go | Clarity, simplicity, explicit errors, interfaces, tests. |
| Java | File structure, Javadoc, exceptions, `@Override`. |
| JavaScript | Modules, naming, JSDoc, readable language features. |
| TypeScript | Strong types, explicit props, interfaces, avoid unjustified `any`. |
| C++ | Header organization, ownership clarity, include discipline. |
| Shell | Quoting, arrays, locals, return checks, ShellCheck. |
| Markdown | Clear headings, informative links, concise docs. |

## 9. QA Checklist for Future AI-Generated UI

### Source Integrity

- Did the agent use Material 3 for UI design and public `google/styleguide`
  only as language-specific engineering guidance?
- Did the agent avoid pretending `google/styleguide` itself contains UI tokens?
- Did the agent avoid claiming this public snapshot represents every internal
  Google engineering standard?
- Are Material 3 roles used semantically rather than hardcoded by component?

### Token QA

- Are colors mapped to role tokens such as `primary`, `surface`, `error`, and
  paired `on-*` roles?
- Are typography choices mapped to display, headline, title, body, or label?
- Are shape and elevation tokens used consistently?
- Are state layers applied for hover, focus, pressed, and dragged states?
- If the design came from Figma, are styles/variables preserved from `M3`,
  `Typescale`, `Shape`, `M3/sys/*`, `M3/ref/*`, and `M3/state-layers/*`?

### Component QA

- Are Material components used where they fit?
- Are component variants chosen by emphasis and task importance?
- Do inputs include labels, helper/error text, and validation states?
- Do dialogs, sheets, menus, and snackbars match their intended use?
- Does navigation adapt between compact and larger layouts?
- If using the Figma kit, did the agent prefer kit component instances and
  variant properties over manually drawing approximations?

### Accessibility QA

- Is keyboard focus visible and logical?
- Do icon-only controls have accessible labels?
- Can screen readers understand structure, state, and actions?
- Does text resize without breaking layout?
- Do non-text UI elements meet relevant contrast expectations?
- Are target sizes large enough for touch?
- Is state communicated by more than color alone?

### Responsive QA

- Was the compact, medium, expanded, large, and extra-large breakpoint matrix
  run rather than only mobile and desktop endpoints?
- Were exact web widths `599`, `600`, `839`, `840`, `1199`, `1200`, `1599`, and
  `1600` checked, or were the native-platform `dp` equivalents checked?
- Were anchors, sticky navigation, tables, rails, bottom navigation, and
  horizontal diagrams checked in every relevant breakpoint state?
- Does the document avoid horizontal overflow, unexpected horizontal scroll,
  and clipped visible content?
- For runnable web projects, is there browser-automation evidence for each
  required route, viewport, and relevant state rather than a CSS-only claim?
- For authenticated routes, did the agent reuse only an authorized test account,
  local mock or seed flow, saved test session, or documented E2E authentication
  setup, without recording credentials in source, audit artifacts, Git,
  screenshots, logs, or the final response?

### Engineering QA

- Is the code readable and locally consistent?
- Are prop and component names meaningful?
- Is state explicit and testable?
- Are errors, loading, empty, disabled, selected, and success states handled?
- Did the agent run available formatters, linters, type checks, tests, or visual
  checks?
- Are docs updated when behavior or usage changes?
- Does code avoid leaking Figma-internal `.Building Blocks/*` naming into public
  production APIs?

### App-Wide Coverage QA

Apply this section only when scope is `all-pages`:

- Did the agent inspect route/view discovery rather than assuming the supplied
  page is the full application?
- Is there a reported inventory of pages, UI families, shared dependencies, and
  migration status?
- Is a version-controlled migration audit present and current enough for another
  agent on another machine to resume the app-wide migration?
- Were common fixes made in tokens, layout primitives, or shared components
  instead of copied into individual pages?
- Was at least one representative page checked for every affected UI family?
- Was every discovered relevant page migrated, intentionally excluded, or
  reported as `blocked` with a reason?
- Did the final report distinguish route statuses from the task outcome? Report
  `completed` only with no blocked or unverified route; otherwise report
  `partial` and list each blocker.

### Final Rule

Use this file as an AI-readable bridge:

- Material 3 defines the UI system.
- Public `google/styleguide` informs engineering discipline; local project
  conventions remain the final implementation authority.
- The host product defines final local constraints.
- The Figma Material 3 Design Kit defines concrete component sets, styles,
  variables, and variants when a design is linked from that file.

When sources disagree, prefer the selected mode:

- In `preserve-local`, prefer local product consistency.
- In `material3-overwrite`, prefer Material 3 visual rules while preserving
  business logic, content hierarchy, and user flow.

Document the decision when the tradeoff matters.

## Reference URLs

- https://m3.material.io/
- https://m3.material.io/foundations/design-tokens
- https://m3.material.io/styles/color/roles
- https://m3.material.io/styles/typography
- https://m3.material.io/styles/elevation
- https://m3.material.io/styles/shape/overview-principles
- https://m3.material.io/foundations/interaction/states/state-layers
- https://m3.material.io/components
- https://m3.material.io/foundations/layout/breakpoints
- https://m3.material.io/foundations/adaptive-design/canonical-layouts
- https://m3.material.io/foundations/designing
- https://m3.material.io/develop
- https://www.figma.com/design/rlipYt5ccndwzYHQLe53PZ/Material-3-Design-Kit--Community-?node-id=11-1833
- https://github.com/google/styleguide
