---
name: dv-m3-design-system
description: Build, refactor, review, or translate UI using a reusable Material 3 design system with Google-inspired engineering guidance from the public google/styleguide repository. Use when the task mentions Material 3, M3, Material You, design tokens, component states, Figma Material 3 Design Kit, UI design system skill, accessibility, adaptive layouts, single-page rebuilds, app-wide UI audits, or AI-generated UI that should follow Material 3 components, tokens, and readable implementation conventions.
---

# Material 3 Design System

## Purpose

Use this skill to design, build, review, or translate UI with Material 3 visual
rules and Google-inspired engineering guidance distilled from the public
`google/styleguide` repository.

This skill combines:

- Material 3 for UI tokens, components, states, layout, accessibility, and
  platform mapping.
- The Material 3 Figma Design Kit for concrete styles, variables, component
  families, variants, and Figma-to-code naming.
- the public `google/styleguide` repository for language-specific, readable,
  maintainable implementation and documentation guidance.

For full design-system details, read `references/design-system.md`. When a task
changes code or documentation, also read
`references/google-styleguide-reference.md` and route to the relevant original
language guide when it is available.
It includes baseline Material 3 color hex values, typography metrics, shape
radius values, elevation effects, and state-layer values extracted from the
Figma Material 3 Design Kit.

## Quick Rules

1. Use Material 3 semantic tokens, not hardcoded visual values.
2. Use `primary`, `secondary`, `tertiary`, `error`, `surface`, and `outline`
   roles with matching `on-*` foreground roles.
3. Use Material type roles: `display`, `headline`, `title`, `body`, and `label`,
   with large, medium, small, and emphasized variants where available.
4. Use shape, elevation, and state-layer tokens instead of one-off radii,
   shadows, and hover colors.
5. Prefer Material component variants over custom approximations.
6. Preserve accessibility: semantic structure, keyboard focus, labels, contrast,
   text resizing, target size, and reduced motion.
7. Before rebuilding or redesigning an existing page, collect both the visual
   rebuild mode and the coverage scope below. Do not begin implementation until
   the user explicitly selects both.
8. Keep implementation readable and locally consistent.
9. When the user selects `all-pages`, audit all relevant pages and shared UI
   before declaring the rebuild complete.

## Rebuild Modes

When using this skill to rebuild, redesign, refactor, or visually transform an
existing page, collect both a visual mode and coverage scope before work begins.

If either value is absent from the user's prompt, stop and ask for the missing
value or values in one preflight question:

```text
Before I start, choose the rebuild mode and coverage scope.

Type one value for each:
- Mode: preserve-local | material3-overwrite
- Scope: current-page | all-pages

preserve-local keeps the existing component library, brand tokens, and
  interaction patterns; use Material 3 only to fix inconsistencies, add states,
  improve accessibility, and tighten layout.
material3-overwrite replaces local visual design with Material 3 tokens,
  components, spacing, shape, elevation, states, and layout; preserve only the
  business structure, content hierarchy, and user flow.
current-page changes only the named page, route, screen, or Figma frame.
all-pages audits and migrates every relevant page in the supplied project.
```

Do not proceed with a page rebuild until the user explicitly supplies both
values. If the user supplies one value, retain it and ask only for the other.

## Coverage Scope

Choose the coverage scope independently from the visual rebuild mode.

- `current-page`: change only the named page, route, screen, or Figma frame.
  State that no app-wide audit was performed.
- `all-pages`: audit all relevant routes/views and migrate every affected UI
  family through shared foundations. Report `completed` only when no route is
  `blocked` or `unverified`; otherwise report a `partial` result with the
  remaining blockers.

Always ask the user to select one of these before implementation. Do not infer
scope from the fact that the user supplied a single example. If the user selects
`all-pages` but has not supplied a project, repository, or route inventory, ask
for the project source rather than silently falling back to `current-page`.

### `all-pages` Procedure

1. Discover routes, pages/views, app shells/layouts, shared components, token
   definitions, and UI documentation or stories.
2. Create or update a version-controlled audit file. Prefer the repository's
   existing migration/docs location; otherwise use `docs/ui-migration-audit.md`.
   Track page/route, UI family, shared dependencies, migration status,
   verification evidence, blockers, and explicit exclusions. Verify the file is
   not ignored and summarize it in the response.
3. Establish or repair the shared layer first: theme tokens, typography, layout
   primitives, navigation, form controls, feedback, and component states.
4. Migrate every affected page by reusing the shared layer. Do not duplicate a
   page-local fix that belongs in a token, primitive, or shared component.
5. Before testing an authenticated route, inspect the project for an authorized
   test account, local mock or seed flow, saved test session, or documented E2E
   authentication setup. If none is available, ask the user for an authorized
   test-environment login method. Never request or use production credentials,
   real-user accounts, or secret keys. Never write credentials to code, the
   audit file, Git, screenshots, or the final response. Mark the route `blocked`
   only when no authorized test access is available.
6. Inspect available scripts and start the app when it is runnable. Use browser
   automation to check every affected route/view at the required breakpoint
   matrix, capture screenshots and overflow results, and record evidence in the
   audit file. Mark a route that cannot be run or verified as `blocked` with its
   reason; do not leave it as `unverified` when reporting completion.
7. Report `completed` only after every discovered relevant page is migrated or
   intentionally excluded with a reason, with no route `blocked` or
   `unverified`. Otherwise report a `partial` result and list every blocker.

Keep the audit file current after every material migration or verification step
so another agent can resume the same `all-pages` task without rediscovering
scope or status. Do not use `.codex/` as the default audit location because it
may not be version-controlled; use it only when the user explicitly requests a
local-only audit.

Never report an app-wide Material 3 rebuild as complete after changing only the
page supplied as the first example.

### Mode 1: `preserve-local`

Use for real project maintenance.

- Keep the existing component library.
- Keep brand tokens.
- Keep existing interaction patterns.
- Use Material 3 to repair inconsistencies, add missing states, improve
  accessibility, and clarify responsive layout.
- Avoid large-scale visual replacement.

### Mode 2: `material3-overwrite`

Use when the user explicitly wants a full Material 3 takeover.

- Replace local colors, typography, radius, elevation, spacing, and visual
  states with Material 3 tokens.
- Replace local UI components with Material 3 component semantics.
- Preserve only business logic, content hierarchy, and user flow.
- Treat old UI styling as functional reference only, not as a constraint.
- Output a new M3-first page.

## Workflow

1. For an existing-page rebuild, collect the rebuild mode and coverage scope.
   Do not begin implementation until the user explicitly provides both.
2. Inspect the target project or design source before choosing tokens or
   components. For `all-pages`, inventory routes/views and shared UI first.
3. Determine whether the task is design creation, code implementation, review,
   Figma translation, or design-system documentation.
4. In `preserve-local`, use existing project components first. If none exist,
   use Material 3 component semantics.
5. In `material3-overwrite`, use Material 3 tokens and component semantics as
   the target system; keep local UI only as a functional reference.
6. Establish or repair shared tokens, primitives, and component families before
   applying page-level changes. In `all-pages`, reuse them across every affected
   route/view.
7. Map visual decisions through tokens:
   - Color: `M3/sys/*` roles.
     Use `references/design-system.md` when exact baseline light/dark hex values
     are needed.
   - Typography: `M3/display/*`, `M3/headline/*`, `M3/title/*`, `M3/body/*`,
     `M3/label/*`.
     Use `references/design-system.md` when exact font size, line height,
     tracking, and weight values are needed.
   - State layers: hover 8%, focus 10%, pressed 10%, dragged 16%.
   - Elevation: levels 0-5, with light/dark effect styles where available.
   - Shape: Material shape scale or Figma `Shape` variables.
     Use `references/design-system.md` for concrete radius and shadow values.
8. Map components through Material families:
   - Actions: buttons, FABs, icon buttons, segmented buttons, split buttons.
   - Inputs: text fields, checkboxes, radio buttons, switches, sliders, chips,
     date/time pickers.
   - Containers/feedback: cards, dialogs, sheets, snackbars, tooltips, progress.
   - Navigation: app bars, navigation bar, navigation rail, drawer, tabs, lists,
     menus, search.
9. Validate with design QA, accessibility QA, available code checks, and a
   Material 3 breakpoint matrix. For runnable web projects, run browser
   automation at every required CSS-pixel viewport and record screenshot and
   overflow evidence. Include anchor/state validation for sticky navigation,
   horizontal workflow diagrams, tables, rails, and responsive navigation; then
   apply the `all-pages` completion rules when that scope is active.

## Figma Kit Mapping

When a task references the Material 3 Design Kit Community file
`rlipYt5ccndwzYHQLe53PZ`, treat it as the concrete design library.

Observed kit facts:

- Version: `V 1.24`.
- Variables: `M3`, `Font theme`, `Typescale`, `Shape`.
- Styles: 727 paint styles, 30 text styles, 10 elevation effect styles.
- Components: 171 component sets.
- Platform guidance: Android is Compose-first; MDC-Android is in maintenance
  mode.

Use this mapping:

| Figma Source | Code Target |
|---|---|
| `M3/sys/light/*`, `M3/sys/dark/*` | Theme color roles / CSS variables / `ColorScheme` |
| `M3/ref/*` | Reference palettes, not direct component colors |
| `M3/state-layers/*` | State-layer overlays or state tokens |
| `M3/display/*`, `M3/headline/*`, `M3/title/*`, `M3/body/*`, `M3/label/*` | Typography tokens |
| `M3/Elevation Light/*`, `M3/Elevation Dark/*` | Elevation effect tokens |
| `Shape` variables and `Shape Set` | Shape scale / radius tokens |
| Component set variant properties | Component props such as variant, state, size, icon, selected |
| Material Symbol component names | Material Symbols icon names in code |

Do not expose Figma-internal `.Building Blocks/*` names as public production APIs.

## Implementation Notes

For Web:

- Use existing project components first.
- Map tokens to CSS custom properties, theme objects, or framework tokens.
- If using `@material/web`, remember it implements Material 3 components but is
  in maintenance mode. Verify the installed version and official documentation
  for every required component or Material 3 Expressive feature; do not assume
  complete Expressive parity.

For Jetpack Compose:

- Use `MaterialTheme(colorScheme, typography, shapes)` and Material 3
  composables.
- Prefer Compose-first mappings for Android.

For Flutter:

- Use `ThemeData(useMaterial3: true)` and map through `ColorScheme`,
  `TextTheme`, shapes, and component themes.

For React or other frontend stacks:

- Use the project's adopted Material-compatible library if one exists.
- Keep component props explicit and typed.
- Preserve Material semantics even when rendering with custom components.

## QA Checklist

Before finishing, check:

- Are visual values tokenized?
- Are component variants chosen by task importance and emphasis?
- Are hover, focus, pressed, disabled, selected, loading, error, and empty states
  handled where relevant?
- Are keyboard and screen-reader paths usable?
- Does layout adapt across compact, medium, expanded, large, and extra-large
  breakpoints?
- For responsive web pages, run the CSS-pixel matrix `599`, `600`, `839`, `840`,
  `1199`, `1200`, `1599`, and `1600` before finishing. For native platforms,
  run the equivalent `dp` window sizes.
- For pages with anchors, sticky navigation, horizontal diagrams, tables, rails,
  or bottom navigation, validate each major anchor/state at those breakpoints.
- Fail responsive QA if `scrollWidth > viewportWidth`, `scrollX !== 0` after
  resetting horizontal position, or visible content is clipped outside the
  viewport. An intentional nested horizontal scroller is allowed only when it
  does not create document-level overflow and remains accessible.
- Does text fit without overlap or overflow?
- For runnable web projects, use browser automation for the matrix and record
  screenshots plus overflow results in the audit file or final evidence.
- For authenticated routes, use only an authorized test account, local mock or
  seed flow, saved test session, or documented E2E authentication setup. Ask
  the user for an authorized test-environment login method when none exists;
  never request, use, or record production credentials, real-user accounts, or
  secret keys.
- Are icons mapped to Material Symbols or verified project icons?
- Is code readable, locally consistent, and tested or checked proportionally to
  risk?
- For `all-pages`, was every discovered relevant route/view migrated, excluded,
  or reported as `blocked` with an explicit reason? Report `completed` only when
  no route is blocked or unverified; otherwise report `partial`.

## Reference

Read `references/design-system.md` for the full AI-readable design system,
including token tables, component guidance, Figma kit inspection, code mappings,
and detailed QA rules.
