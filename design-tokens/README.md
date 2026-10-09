# Portfolio design system — Figma ↔ code handoff

This folder is the code-side mirror of the Figma design system built for the live portfolio
(`michelleangelaa/webportfolio`, Vite + React 19 + Tailwind v4). The site's `src/styles/app.css`
`@theme` block and `src/data/tints.ts` remain the source of truth for shipped values; the Figma file
is where new work is designed.

| What | Where |
| --- | --- |
| Figma file | `https://www.figma.com/design/9GnxHhVR7PP7xBpyDRhGgu/?node-id=935-244` |
| Page | `[Portfolio] Website` (node `935:244`, formerly "Page 32") |
| Sections | `00 · Read me`, `01 · Foundations`, `02 · Components`, `03 · Live pages · Desktop 1440`, `04 · Live pages · Mobile 390`, `99 · Image assets` |
| Token export | `portfolio.tokens.json` (W3C Design Tokens format, Figma extensions) |
| Node map | `figma-map.json` (section, component-set, page-frame and image-hash IDs) |

## What is in the Figma file

**Variables — 134 across 5 collections**

| Collection | Modes | Count | Purpose |
| --- | --- | --- | --- |
| `Portfolio · Primitives` | Value | 46 | Raw palette: `neutral/0…950`, five tint ramps (`cyan`, `amber`, `pink`, `indigo`, `green` at 50/300/700/900), `cream/100`, hero gradient stops (`lime/50`, `peach/50`, `rose/50`), `alpha/black/*` for shadows. Hidden from pickers (no scopes). |
| `Portfolio · Color` | Light, Dark | 31 | Semantic roles, every value an alias to a primitive: `color/bg/*`, `color/text/*`, `color/border/*`, `color/icon/*`, `color/tint/<tint>/bg|fg`. Pick from this collection, never from Primitives. |
| `Portfolio · Spacing` | Value | 32 | `space/*` on the Tailwind 4px scale (`space/0-5` = 2px … `space/14` = 56px) plus layout sizes (`size/shell-max` 1180, `size/rail` 344, `size/content-desktop` 698, avatar / icon / hero-band / shot heights, viewports). |
| `Portfolio · Shape` | Value | 9 | `radius/shell` 28, `radius/card` 18, `radius/frame` 14, `radius/shot` 10, `radius/focus` 6, `radius/pill` 999; `border/width/default|focus|avatar`. |
| `Portfolio · Typography` | Value | 16 | `font/family/sans` (Inter), `font/weight/regular|medium|semibold`, `font/size/13…30`. Bound into the text styles. |

**Styles** — 17 text styles (`display/lg`, `heading/lg…xs`, `title/md|sm`, `stat/md`, `body/lg|md|sm`,
`label/xl…xs`, `meta/md`), 4 effect styles (`shadow/shell`, `shadow/card-hover`, `shadow/shot`,
`shadow/avatar`), 6 gradient paint styles (`gradient/hero-band`, `gradient/bed/<tint>`).

**Components — 29 main components / sets**, one per source component or repeated page block:

| Figma | Source | Props / variants |
| --- | --- | --- |
| `Icon / Mail · LinkedIn · Dribbble · GitHub` | `components/icons.tsx` | — |
| `Tag` | `components/Tag.tsx` | `Tint` × `Size` (md / sm), `Label` |
| `Button` | header CTA, 404 | `State`, `Label` |
| `NavLink` | `components/Shell.tsx` | `State` (default / hover / active), `Label` |
| `Link / CTA`, `Link / External`, `Link / Quiet` | card CTA, rail links, back / footer / inline / list links | `Tint` or `Style`, `Label` |
| `SocialButton` | Work page socials | `State`, `Icon` (instance swap) |
| `Avatar` | Work / About | `Size` (lg 84 / md 64) |
| `SectionHeading` | `.section-heading` | `Size` (desktop / mobile), `Label` |
| `Card` | `.card` | `Padding` × `State`, `Content` slot |
| `Header`, `Footer` | `components/Shell.tsx` | `Active` × `Device` / `Device` |
| `Figure` | `components/Figure.tsx` | `Treatment` (fill / inset / inset-flat) × `Device`, `Caption`, `Show caption` |
| `Metric` | case-study outcomes | `Tint`, `Value`, `Label` |
| `ProcessStep` | case-study "How it went" | `Device`, `Number`, `Title`, `Body` |
| `ExperienceItem`, `StackItem` | `pages/About.tsx` | text props |
| `AwardCard`, `InitiativeCard` | `pages/Other.tsx` | `Device`, text props, metric toggles |
| `NextLink` | case-study footer link | `Device`, `Title` |
| `ProjectCard` | `components/ProjectCard.tsx` | `Tint` × `Device`, `Org`, `Title`, `Blurb`, tag / figure toggles; nested Tag, Figure and CTA instances are exposed |
| `ProfileRail`, `CaseStudyRail`, `OtherRail` | left rails of Work / case study / Other | `Device`, text props |
| `PageShell` | `components/Shell.tsx` | `Device`; dashed `Content` frame to replace |

**Live pages — 13 frames** built from those instances with the real copy from `src/data`:
Work, About, Other, `/work/tiket-ticket-content`, `/work/playful-voice-studios` and 404 at 1440 and
at 390, plus `Work · 1440 · Dark preview` (the same frame with the Color collection switched to Dark).

## How to design with it

1. Colours: use `Portfolio · Color` only. Tint pairs are `color/tint/<tint>/bg` + `/fg`; the five
   tints map 1:1 to `tints.ts`. Switch a frame to Dark via the variable-mode control to preview.
2. Spacing, radius and border widths: bind to `Portfolio · Spacing` / `Portfolio · Shape`.
   `space/N` is Tailwind's `N` (`space/4` = `gap-4` = 16px).
3. Type: apply a text style; never set size / weight by hand. Sizes are rounded to whole pixels
   (`0.95rem` → `label/md` 15px, `1.35rem` → `heading/sm` 21px).
4. New screens: start from `PageShell`, swap the Header's `Active` variant, replace the dashed
   `Content` frame. Desktop rail + column = `size/rail` + `space/10` gap + fill.
5. Dev Mode shows each variable's code syntax (`var(--color-ink)`, `calc(var(--spacing) * 4)`,
   `var(--radius-card)`) so a token can be found in `app.css` directly.

## Mapping to code

| Figma | `app.css` / Tailwind |
| --- | --- |
| `color/bg/page · shell · card` | `--color-page`, `--color-shell`, `--color-card` |
| `color/text/primary · secondary · muted` | `--color-ink`, `--color-ink-soft`, `--color-ink-faint` |
| `color/border/default · subtle` | `--color-line`, `--color-line-soft` |
| `color/bg/inverse`, `color/text/on-inverse-*` | `--color-dark`, `#F3EAC8`, `#D8D8D8` (award cards) |
| `color/tint/<t>/bg · fg` | `--tag-bg`, `--tag-fg` via `tintVars()` |
| `radius/shell · card · frame` | `--radius-shell`, `--radius-card`, `--radius-frame` |
| `space/*` | Tailwind spacing scale |
| text styles | `text-[…rem]` + `font-*` utilities, `.meta`, `.section-heading` |
| `shadow/*`, `gradient/*` | inline `box-shadow` / `background` in `.frame*`, `.shell`, Work hero |

## Tokens that exist in Figma but not (yet) in code — proposals

- **Dark mode**: every `Portfolio · Color` variable has a Dark value. Supporting primitives
  `neutral/400 · 700 · 900 · 950` and the tint `*/300` and `*/900` steps were added for it. To ship
  it, add a `.dark` / `prefers-color-scheme` block in `app.css` that overrides the `--color-*` and
  `--tag-*` custom properties with the values in `$extensions.figma.modes.Dark`.
- `color/border/strong` — names the existing `hover:border-ink` behaviour as a token.
- `space/1-25` (5px) — the `.tag` vertical padding (`0.3rem`), rounded.
- `size/*` tokens — layout measurements that are literal values in CSS today (1180, 344, 84, 64, 40, 144, shot heights).
- `color/bg/frame-fill`, `color/bg/selection`, `color/border/hover`, `color/border/avatar`, `color/border/focus`,
  `color/icon/*` — hard-coded hex values in `app.css` that now have a name.
- Gradients are paint styles, not variables, so they stay light in Dark mode. A dark variant of
  `gradient/hero-band` and `gradient/bed/*` is still to be designed.

## Quality bar (checked on the exported values)

Contrast, WCAG 2.1 AA, computed from the resolved variables in both modes:

| Pair | Light | Dark |
| --- | --- | --- |
| text/primary on bg/card | 15.1 | 13.7 |
| text/secondary on bg/card | 6.4 | 10.7 |
| text/muted on bg/card | 4.5 | 6.1 |
| text/on-accent on bg/accent | 15.1 | 16.4 |
| on-inverse-title / body on bg/inverse | 12.7 / 10.7 | 15.1 / 6.4 |
| tint fg on tint bg (worst of five) | 6.2 (pink) | 7.6 (indigo) |
| tint fg on bg/card (worst of five) | 6.7 (pink) | 7.2 (indigo) |
| border/default on bg/card | 1.2 | 1.4 |

Every text pair passes 4.5:1 in both modes; `text/muted` on white is the tightest at 4.54:1.
Card borders are decorative (1.2:1) and rely on the card fill difference, as in the live site.
Keyboard focus: `radius/focus` and `border/width/focus` + `color/border/focus` encode the
`:focus-visible` ring; interactive components carry `State` variants. Both themes are defined at the
variable level; both breakpoints (1440, 390) are built for every component and page.

Audit of the Figma file: 937 nodes in components and pages, 0 unbound solid fills or strokes,
0 default-named layers, 1 intentional unstyled text (the NextLink `→` glyph, 21px Regular).

## Caveats

- Image fills are reduced-quality JPEG stand-ins embedded through the plugin API (the sandbox could
  not upload originals). Swap them for `public/media/*.webp` before exporting from Figma.
- The Figma file uses the static `Inter` family; the site uses `Inter Variable`. Metrics match at the
  weights used (400 / 500 / 600).
- Page frames are approximations of the live render: text widths and image crops match the
  Playwright reference within a few pixels; the live site was not reachable from the build sandbox.

## Updating

Code stays the source of truth. When a token changes in Figma, change `app.css` (or `tints.ts`) in
the same PR and regenerate this folder: re-export the variable collections and styles and rebuild
`portfolio.tokens.json` (values, scopes and code syntax come straight from the Figma variables).
