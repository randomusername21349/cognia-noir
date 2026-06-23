# Cognia Noir -- Design Notes

Design rationale extracted from `theme.css` to keep the shipped CSS
lean (Obsidian flags themes over ~100 KB). The CSS keeps one-line
breadcrumbs; the full reasoning for each rule lives here, in source
order, keyed by the selector it precedes. Mirrors the earlier
`CHANGELOG.md` split (v1.10.24).


## 0. SHARED CONSTANTS - theme-independent values referenced in


### `:root {`


0. SHARED CONSTANTS - theme-independent values referenced in
both .theme-dark and .theme-light. Defined once here; do not
duplicate in either themed block.


### `--inline-title-size: 1.6em;`


Inline title - size/weight/line-height are mode-independent.
  --inline-title-color stays in each themed block (references --h1-color).


### `--cn-editor-font-size: 14px;`


Editor font size and line height - driven by Style Settings sliders.
  Previously duplicated identically in both .theme-dark and .theme-light.


### `--cn-heading-font: "SF Pro Text", -apple-system, system-ui, BlinkMacSystemFont, sans-serif;`


Heading font default - SF Pro Text to match body text (tight scale headers
  read below ~20px, where Text is the size-appropriate Apple face and shares
  the body's family for cohesion). Overridden by body.cn-heading-* Style
  Settings classes (New York / Georgia serif alternatives).


### `--h1: 1.4em;`


Heading sizes/weights/line-heights - mode-independent. Colors stay in each
  themed block (--h1-color..--h6-color). The --h1..--h6 token names are read
  by both the theme's heading rules (reading view + Live Preview); keep these
  names (do NOT rename to --h*-size).
  Design: tight, Notion-like scale. The inline title (1.6em/700) is the single
  largest, heaviest anchor; H1 sits AT OR BELOW it so headings read as quiet
  structure cohesive with body, not competing display type. Hierarchy comes
  from size + a subtle color step; weight is UNIFORM (600) across H1-H6.


### `--cn-panel-side-inset: 8px;`


Shared horizontal inset for sidebar panel boxes (folder box,
  plugin-leaf boxes, vault-profile pseudo). Locks all left-column
  panels to the same x-coordinate. Previously a standalone :root
  block in section 19b; moved here per the section 0 pattern.


## 1. CSS VARIABLES - DARK THEME


### `--cn-bg-primary: #1d1d1d;`


Base palette. bg-primary lifted slightly off bg-secondary so the
  note panel reads as a distinct surface against the sidebar without
  the seam feeling stark. Paired with the rounded inside corners in
  section 4b.


### `--cn-bg-note-tray: #131313;`


Tray painted behind the note panel in section 4b. Needs to be
  visibly different from --cn-bg-primary or the rounded corners
  of the note read as the same color and look square. Sits a few
  stops below the note so the note feels lifted on a darker base.


### `--cn-inline-code: #ee6666;`


Used by rgba()-based effects (selection, focus ring, link underline)
  so they follow the user's Style Settings accent automatically.
  color-mix gives us alpha control without a parallel RGB variable.


### `--cn-inline-code: #ee6666;`


Inline code red: brightened from #eb5757 (4.40:1 on bg-tertiary, just
  under WCAG AA) to #ee6666 (~4.6:1) for body-text contrast.


### `--cn-strong-color: #ffffff;`


Theme-aware tokens - flipped in .theme-light so shared rules below
  (bold text, dividers, hover overlays, scrollbar, checkboxes, sidebar
  lifted tint) work correctly in both modes.


### `--cn-modal-backdrop: rgba(0, 0, 0, 0.65);`


Modal chrome - backdrop dim behind dialogs and shadow under the dialog
  itself. Heavy in dark (matches the moody aesthetic), much lighter in
  light (a 0.65 black overlay in light mode darkens the whole UI to
  near-unreadable when Cmd-K or Settings open).


### `--cn-amber-warning: #ffd54f;`


Status / in-progress amber. Used for Obsidian Sync's syncing
  state and reusable for other warning-state indicators. Solid
  hex (not RGB triplet) because it does not need alpha
  composition; for tinted backgrounds use --callout-color
  pattern instead.


### `--cn-tint-alpha: 0.18;`


Translucent surface over macOS vibrancy. Vibrancy comes from Obsidian's
  Translucent Window setting (Electron NSVisualEffectView). Rather than a thin
  dark WASH over mostly-vibrancy chrome (which made the glassy sidebar clash
  with the more-opaque note), the workspace base is a HIGH-OPACITY wash of the
  panel color. Every region (sidebar, note, chrome) shows this one base, so
  the whole window reads as a single cohesive subtle-glass surface with a faint
  vibrancy bleed and legible text. The Translucency tint knob swaps the opacity
  (section 1c). --cn-tint-alpha kept for the light-mode no-op path.


### `--background-primary: var(--cn-bg-primary);`


Default mapping (Translucent Window OFF or fullscreen): solid surfaces.
  Under .is-translucent the workspace layers are forced transparent in
  section 4 so vibrancy shows through.


### `--background-modifier-cover: var(--cn-modal-backdrop);`


Core paints these surfaces from its own variables; setting the
  variable is cleaner than overriding the rule with !important.


### `--h1-color: #ffffff;`


Heading colors - mode-specific. Sizes/weights/line-heights are in :root.
  ALL headings share one color (full white). Hierarchy is carried by size
  alone; no color ramp.


### `--inline-title-color: var(--h1-color);`


size / weight / line-height are in :root (mode-independent)


## 1b. CSS VARIABLES - LIGHT THEME (Notion-warm off-white)


### `.theme-light {`


1b. CSS VARIABLES - LIGHT THEME (Notion-warm off-white)
Mirrors the dark block: same tokens, flipped values. Heading scale,
weight cascade, inline-title sizing, and Style Settings knobs all
carry over so the two modes feel like one design.


### `--cn-bg-primary: #ffffff;`


Notion light-mode palette - pure white canvas, warm near-black text,
  subtle off-white sidebar. Fully opaque (Notion does not use macOS
  vibrancy in light mode). High contrast, no decoration.


### `--cn-bg-note-tray: #d8d6d1;`


Tray painted behind the note panel in section 4b. Sits a few stops
  below the white note so the rounded corners of the leaf read against
  a clearly different tone instead of disappearing into the canvas.


### `--cn-text-secondary: #666564;`


Secondary text: was #787774 (4.48:1 on white, fails AA by 0.02 and drops
  to 4.05:1 on the warm canvas). #666564 clears 4.5:1 on white, both
  sidebars (default + warm + lifted), and bg-tertiary default. Borderline
  4.43:1 on warm bg-tertiary, where text-secondary rarely lands.


### `--cn-text-faint: #7d7c79;`


Faint text: was #9b9a97 (2.81:1 on white, 2.54:1 on warm; below 3:1 UI
  minimum). #7d7c79 clears 4:1 while keeping a "muted" feel.


### `--cn-accent: #3a5fc7;`


Accent darkened for light mode: #5b8def yields 3.23:1 on white (link
  contrast fails AA). #3a5fc7 clears ~4.6:1 on white. Hover stays one
  step darker for visual feedback.


### `--cn-inline-code: #a8201a;`


Inline code: #eb5757 yields 2.92:1 on bg-tertiary in light mode.
  #c0392b only clears 4.14:1 on the warm bg-tertiary; #a8201a holds
  5.5:1+ across both default and warm code backgrounds.


### `--cn-strong-color: #37352f;`


Theme-aware tokens - Notion light values. Bold text uses the same
  color as body and lets font-weight do the work (Notion convention).


### `--cn-modal-backdrop: rgba(15, 15, 15, 0.30);`


Modal chrome - Notion-style: ~30% backdrop dim, soft 10% shadow.


### `--cn-amber-warning: #ffd54f;`


Status / in-progress amber. Same value as dark mode -- status
  indicators should be brand-consistent across modes.


### `--cn-tint-alpha: 0.0;`


Translucency vars set as no-ops - light mode is opaque. The wash on
  .workspace still exists but doesn't show through opaque surfaces.
  Style Settings translucency tint knob has no effect in light mode.


### `--background-modifier-cover: var(--cn-modal-backdrop);`


Core paints these surfaces from its own variables; setting the
  variable is cleaner than overriding the rule with !important.


### `--h1-color: #37352f;`


Heading colors - mode-specific. Sizes/weights/line-heights are in :root.
  ALL headings share one color (near-black). Hierarchy is carried by size
  alone; no color ramp.


### `--inline-title-color: var(--h1-color);`


size / weight / line-height are in :root (mode-independent)


### `.theme-dark.cn-bgd-black {`


Background tone variants - DARK MODE (Style Settings: cn-bg-tone-dark).
Charcoal is the default and lives in the base .theme-dark block above
(no class rule). Only applies in dark mode; the light picker is separate.


### `--cn-bg-note-tray: #050505;`


Tray must be darker than bg-primary so the note card reads as lifted on a
  base. The .theme-dark default tray (#131313) is lighter than #0d0d0d,
  inverting the hierarchy; #050505 restores it for the default Flat sidebar.


### `.theme-dark.cn-bgd-zen {`


Zen: warm-gray identity carried via hue (red/orange undertone) rather than
mid-gray lightness. Surfaces pulled toward charcoal-warm so the default accent
and inline code clear WCAG AA on every surface. Sidebar slightly darker than
canvas to mirror Zen browser's layout. Borders/overlays use rgba so they adapt.


### `.theme-dark.cn-bgd-slate {`


Neutral gray tones - no warm or Zen hue, pure grayscale surfaces.
Slate is a lighter, airier neutral; Graphite is a deeper neutral that sits
between Charcoal and Pure black. The default Flat tray (#131313) stays darker
than both canvases, so the note card still reads as lifted.


### `--cn-bg-primary: #232323;`


Lighter neutral than Charcoal, but the code surface (--cn-bg-tertiary) is
  held at #2a2a2a so the default inline code (#ee6666) keeps WCAG AA (4.6:1);
  a lighter tertiary drops it below 4.5:1.


### `.theme-dark.cn-bgd-trueblack {`


True black: pure #000000 canvas for maximum OLED contrast. The tray cannot go
darker than the canvas, so it matches at #000000 (seamless); the note card
separates via --cn-bg-elevated and its border instead.


### `.theme-light.cn-bgl-white {`


Background tone variants - LIGHT MODE (Style Settings: cn-bg-tone-light).
Paper (warm-ish white) is the default and lives in the base .theme-light block
(no class rule). Only applies in light mode; the dark picker is separate.


### `.theme-light.cn-bgl-zen {`


Zen light parity. Surfaces raised toward near-white-warm so the default light
accent (#3a5fc7) and inline code (#a8201a) clear AA on every surface, including
bg-elevated. Sidebar slightly darker than canvas.


### `.theme-light.cn-bgl-mist {`


Neutral gray tones - light parity (clean grays, no warm tint). Mist is the
lighter gray; Stone is the deeper.


### `body.cn-grayscale.theme-dark {`


Grayscale UI toggle (Style Settings: cn-grayscale)
Desaturate the theme's semantic colors so the interface reads as pure
black and white. Implemented as a token remap rather than a CSS filter
so (a) note images and code syntax highlighting keep their color and
(b) the translucency/backdrop-filter system is left untouched. Most UI
color flows from --cn-accent (links, tags, selection, buttons, focus
rings via --text-accent/--interactive-accent), so remapping it plus the
few standalone color tokens covers the theme. Higher specificity
(body.cn-grayscale.theme-*) outranks the base .theme-* token blocks.


### `body.cn-grayscale .callout { --callout-color: var(--cn-accent); }`


Callouts: drive every type's color from the grayed accent so tip/warning/
info and Callout Manager colors all read monochrome too. Outranks the
per-type and default --callout-color rules in section 8.


### `.theme-dark.cn-translucent-off    { --workspace-background-translucent: color-mix(in srgb, var(--cn-bg-primary) 45%, transparent); }`


Translucency tint intensity = opacity of the unified subtle-glass surface
over macOS vibrancy. Pure = glassiest (most vibrancy shows), Heavy = nearly
solid. Medium (default) is the cohesive subtle-glass level. Only takes effect
when Obsidian's "Translucent window" toggle is on. Literal percentages (not a
var) so color-mix resolves reliably.


### `body:not(.is-mobile).theme-dark.cn-sidebar-border .workspace-split.mod-left-split {`


Light mode is opaque (Notion-style) so the Translucency tint knob has
no effect there. The .theme-light.cn-translucent-* class rules were
removed; --cn-tint-alpha stays at the .theme-light block default (0.0).


### `body:not(.is-mobile).theme-dark.cn-sidebar-border .workspace-split.mod-left-split {`


Sidebar contrast variants - dark


### `body:not(.is-mobile).theme-dark.cn-sidebar-border .workspace-split.mod-left-split {`


Sidebar border on the inner edge only -- the edge that faces the note.
Left sidebar's left edge and right sidebar's right edge sit against
the window frame (no adjacent content), so a border there is noise.
The panel boxes in section 19b provide their own inner border; this
option adds a separator at the split level for users who want it.


### `body:not(.is-mobile).theme-dark.cn-sidebar-border .workspace-split.mod-left-split {`


:not(.is-mobile) guard: on mobile, .workspace-split.mod-{left,right}-split
is the drawer container; a border there would draw a line on the
drawer's inner edge. Desktop split panes only.


### `.theme-dark.cn-sidebar-lifted              { --background-secondary: #1b1b1b; --cn-bg-note-tray: #0d0d0d; }`


Lifted sets both --background-secondary (panel) and --cn-bg-note-tray (workspace-split
tray, which bypasses --background-secondary in opaque mode -- see section 4b).


### `.theme-dark.cn-sidebar-lifted.cn-bgd-black  { --background-secondary: #0a0a0a; --cn-bg-note-tray: #000000; }`


Pure Black tray (#000000): one stop darker than default (#050505).


### `.theme-dark.cn-sidebar-lifted.cn-bgd-trueblack { --background-secondary: #000000; --cn-bg-note-tray: #000000; }`


True black lifted: canvas is already #000, so sidebar/tray stay #000;
the note card still separates via --cn-bg-elevated and its border.


### `body:not(.is-mobile).theme-light.cn-sidebar-border .workspace-split.mod-left-split {`


Sidebar contrast variants - light


### `body:not(.is-mobile).theme-light.cn-sidebar-border .workspace-split.mod-left-split {`


Same inner-edge-only rule for light mode.


### `.theme-light.cn-sidebar-lifted.cn-bgl-zen   { --background-secondary: #e6e3de; --cn-bg-note-tray: #c8c5c0; }`


Zen tray: one stop darker than default (#d8d6d1), with warm undertone to match Zen surfaces.


### `body.is-translucent.theme-dark:not(.is-fullscreen).cn-sidebar-lifted .workspace-split.mod-left-split,`


Sidebar-lifted in dark translucent mode: outer-shell containers are
forced transparent, so paint a subtle dark tint to give Lifted a
visible effect over the vibrancy. Light mode is opaque so this rule
only fires in dark.


### `body.is-translucent.theme-light.cn-sidebar-lifted .workspace-split.mod-left-split,`


Sidebar-lifted in light translucent mode: the section 4 force-paint
block paints sidebar with var(--cn-bg-secondary), but cn-sidebar-lifted
only overrides --background-secondary (Obsidian's token). Without this
higher-specificity rule, the lifted setting is a silent no-op when
Translucent Window is enabled in light mode. We pull from
var(--background-secondary) here because the .cn-sidebar-lifted +
bg-tone variants set that variable to the lifted hex.


## 2. TYPOGRAPHY


### `body {`


2. TYPOGRAPHY
- UI chrome: Inter (Linear/Notion-style, slick + neutral)
- Note content: SF Pro Text / Display (Apple's content-optimized stack)
- Code: SF Mono
The UI font is overridable via the Style Settings "UI font" knob.


### `font-feature-settings: "calt", "ss01", "kern", "liga";`


Inter looks best with calt + ss01 (single-storey 'a'), kern, liga


### `body.cn-ui-inter      { --font-interface-theme: "Inter", -apple-system, system-ui, sans-serif; }`


UI font variants - selected via Style Settings


### `body.cn-ui-avenir     { --font-interface-theme: "Avenir Next", "Avenir", -apple-system, system-ui, sans-serif; letter-spacing: 0; font-feature-settings: "kern", "liga"; }`


Avenir is a touch wider -- bump letter-spacing back to 0


### `body.cn-note-sfpro   { --font-text-theme: "SF Pro Text", -apple-system, system-ui, BlinkMacSystemFont, sans-serif; }`


Note font variants - selected via Style Settings


### `body.cn-heading-sfpro   { --cn-heading-font: "SF Pro Text", -apple-system, system-ui, BlinkMacSystemFont, sans-serif; }`


Heading font variants - selected via Style Settings


### `.markdown-source-view.mod-cm6 .cm-content,`


Editor base font size. Header lines are EXCLUDED: a header line is a
`.cm-line.HyperMD-header-N`, so `.cm-line` here (specificity 0,3,0) otherwise
beat BOTH the theme's and Obsidian core's heading-size rules (`.HyperMD-header-N`,
0,1,0) and flattened every Live Preview heading to the editor font size. The
`:not(.HyperMD-header)` guard lets the heading rules size header lines while
normal lines still get the editor base size (which they also inherit from
.cm-content). Confirmed against the running build via DevTools.


## 3. HEADINGS - tight tracking, dramatic scale


### `.markdown-rendered h1, .markdown-rendered h2, .markdown-rendered h3,`


3. HEADINGS - tight tracking, dramatic scale
Font driven by --cn-heading-font (Style Settings knob, default SF Pro
Display). Sizes driven by --h{n}-size variables (set in section 1).
Both apply in reading mode and Live Preview.


### `.markdown-rendered h1, .markdown-rendered h2, .markdown-rendered h3,`


Tracking stays at 0 for the whole tight scale. The previous negative
display-tracking on H1/H2 was for a 2.1em display face; on SF Pro Text at
1.4em and below it reads cramped, so headers inherit the shared 0 tracking
(H5/H6 open slightly below).


### `.markdown-rendered h5, .HyperMD-header-5 {`


H5/H6 are quiet section labels: at/below body size, muted color, slightly
open tracking. Weight matches H4 (600) so the ramp never inverts.


### `.theme-dark.cn-headings-compact,`


Heading scale variants - relative to the tight comfortable default.
Compact pulls headers nearly flush with body; Large is the opt-in that may
exceed the title.


## 4. WORKSPACE


### `.workspace-tab-header {`


4. WORKSPACE
- Default (Translucent window OFF or fullscreen): solid surfaces.
- body.is-translucent + macOS: every workspace layer is forced transparent
  so Obsidian's NSVisualEffectView (set by Electron when the toggle is
  on) shows through with native macOS vibrancy. A faint dark wash is
  applied via --workspace-background-translucent for legibility.
- Reference: AnuPpuccin translucency.scss; Things obsidian.css; Catppuccin
  _app-variables.scss; Obsidian docs CSS variables.


### `@media screen {`


--- Translucent mode ---


### `@media screen {`


All translucent-mode rules are scoped to .theme-dark. Light mode is
opaque (Notion-style) and ignores the Translucent Window setting -
keeping outer-shell containers transparent in light mode would let
raw macOS vibrancy show through and look gray over dark wallpapers.

The transparency rules below are wrapped in @media screen: macOS
vibrancy is a screen-only effect, and scoping them out of print means
PDF/print export inherits opaque surfaces with no override needed.


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-ribbon.mod-left,`


.workspace itself is NOT in this list -- it gets painted with
the translucent tint wash a few rules below, not forced
transparent. Including it here would be dead code (immediately
overridden by the wash rule at equal specificity).


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-leaf-content:not([data-type="terminal"]):not([data-type="canvas"]):not([data-type="excalidraw"]):not([data-type="kanban"]):not([data-type="smart-connections-view"]),`


Plugin leaves (Terminal, Excalidraw, Smart Connections graph) assume
an opaque container; opt them out of the transparent force so they
render correctly over vibrancy.


### `background-color: transparent;`


.status-bar removed from this transparent-force list. Section 14
  turns the status bar into a floating pill that needs a body of its
  own to stay legible over vibrancy.


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace {`


Workspace wash on top of macOS vibrancy - dark mode only. Controlled
by Style Settings Translucency tint knob via --cn-tint-alpha.


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-tabs.mod-stacked .workspace-leaf,`


end @media screen - translucency transparency


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-tabs.mod-stacked .workspace-leaf,`


Stacked tabs need an opaque fallback so text stays readable


### `body.is-translucent.theme-dark:not(.is-fullscreen) {`


Translucent-mode hover/active states use theme-aware overlay tokens
(rgba whites in dark) so they tint the vibrancy rather than punch
opaque holes in it. Dark-only since light mode is opaque.


### `body.is-translucent.theme-light .workspace-split.mod-left-split,`


Light mode opaque-restore: when Translucent Window is enabled in
Obsidian's Appearance settings, Obsidian's own internal CSS sets
workspace surfaces to transparent so vibrancy can show through.
Light mode is fully opaque (Notion-style), so explicitly paint the
outer-shell containers with our solid colors to override Obsidian's
internal transparency.


### `body.is-translucent.theme-light .workspace-leaf-content:not([data-type="terminal"]):not([data-type="canvas"]):not([data-type="excalidraw"]):not([data-type="kanban"]):not([data-type="smart-connections-view"]) {`


Plugin leaves opted out for the same reason as the dark counterpart
above: their internal styling assumes an opaque container, and our
bg-primary force-paint can collide with their own backgrounds.


## 4b. NOTE PANEL - soft card edge against the sidebar


### `body:not(.is-mobile) .workspace-split.mod-root,`


4b. NOTE PANEL - soft card edge against the sidebar
The note pane in the main split rounds its top-left and
bottom-left corners (where it meets the sidebar) so the
boundary reads as a soft transition rather than a sharp
90-degree seam. Right edges stay flat against the window.

Two requirements for the rounded edge to be visible:
1. The parent area behind the leaf (workspace-tab-container)
   must be painted with a contrasting tone so the rounded
   corners reveal something different from the note. We use
   --background-secondary (sidebar tone) so the rounded
   note reads as a card sitting on the sidebar tray.
2. The leaf-content must clip its children with overflow:
   hidden, otherwise the .view-header above the note body
   paints its own background over the rounded corners.

The note also stays solid in dark translucent mode so the
color difference reads cleanly. The tray behind it stays
transparent in translucent mode so macOS vibrancy still
shows through the chrome.


### `body:not(.is-mobile) .workspace-split.mod-root,`


Tray painted at two levels so the rounded reveal shows the same
tone whether the rounded element is .workspace-tabs (sidebars,
reveals .workspace-split) or .workspace-leaf-content (note,
reveals .workspace-tab-container). Painting both means it
doesn't matter which parent is "behind" each rounded child.
Scoped to non-mobile because mobile uses drawer-overlay sidebars
where tray paint changes drawer color unexpectedly.


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-split.mod-root .workspace-tab-container,`


Translucent dark: keep the tab-containers transparent so vibrancy
shows through the chrome. Workspace-splits are already forced
transparent in section 4.


### `body:not(.is-mobile) .workspace-split.mod-root .workspace-leaf-content {`


Note panel: previously rounded the left edge facing the sidebar.
Removed - the corner clipping interfered with editor padding and read
as visual noise more than chrome. The sidebar pane already has its
own rounded inside edge (below) which is enough separation. Keep
overflow:hidden so any decoratively positioned children still clip
at the leaf boundary. Scoped to non-mobile so mobile drawer chrome
isn't affected.


### `body:not(.is-mobile) .workspace-split.mod-left-split .workspace-tabs {`


Left sidebar: round the entire pane on the inside edge (right side
facing the note). Targets workspace-tabs (one level up from the
leaf) so the action-icons row at top and the vault switcher at
bottom also clip to the rounded shape. Non-mobile only - mobile
sidebars are drawer overlays where rounding clips the drawer
header.


### `body:not(.is-mobile) .workspace-split.mod-right-split .workspace-tabs {`


Right sidebar (when open): mirror - round the inside edge (left).


### `body.is-translucent.theme-dark:not(.is-fullscreen) .workspace-split.mod-root .workspace-leaf-content:not([data-type="terminal"]):not([data-type="canvas"]):not([data-type="excalidraw"]):not([data-type="kanban"]):not([data-type="smart-connections-view"]) {`


Translucent dark: the note leaf body stays TRANSPARENT so it shows the single
unified subtle-glass workspace base (set in section 1 / the tint knob) - the
SAME surface the sidebar shows. Painting the note its own opaque layer here
would compound with the workspace base (~0.98) and re-create the tonal seam
against the sidebar. Keeping it transparent makes note and sidebar identical.


## 5. FILE EXPLORER & SIDEBAR LISTS


### `.markdown-source-view.mod-cm6 .cm-content { caret-color: var(--cn-accent); }`


Tree-item active state (file explorer, tag pane, outline) is painted
by core from --nav-item-background-active / --nav-item-color-active,
set in the .theme-dark / .theme-light blocks.


## 6. EDITOR CONTENT


### `.cm-link, .cm-hmd-internal-link, a.internal-link, a.external-link {`


Accent-tinted selection is painted by core from --text-selection,
set in the .theme-dark / .theme-light blocks.


### `.cm-link, .cm-hmd-internal-link, a.internal-link, a.external-link {`


Links


### `strong, .cm-strong { color: var(--cn-strong-color); }`


Bold - color only; let font-weight cascade so bold inside an H1 (weight
900) renders heavier than the surrounding heading rather than capping
at 700. Body bold gets weight 700 from the user-agent default for <strong>.
Color flips per theme via --cn-strong-color (#ffffff dark, #000 light).


### `.markdown-rendered mark,`


Native ==highlight== - soft amber, readable on dark


## 7. CODE - scoped to markdown content (not plugin UIs)


### `.markdown-rendered pre {`


Codeblock surface is painted by core from --code-background (reading
mode and Live Preview both), set in the .theme-dark / .theme-light
blocks. This rule only adds shape.


## 8. CALLOUTS - boxed, highlight-style, amber default


### `.callout[data-callout="note"],`


8. CALLOUTS - boxed, highlight-style, amber default
Obsidian 1.13.0 changed --callout-color from an RGB triplet to a
valid CSS color (breaking change). Each callout type now sets its
own --callout-color as a color (a hex, rgb(), or color var), so we
consume it directly and use color-mix for the translucent tints.
We override [!note] and the unspecified default to amber so the
default callout reads as a yellow highlight, not a blue note.


### `.callout[data-callout="note"],`


Override default note callout color from Obsidian's blue to amber.
Valid CSS color per the 1.13 contract (#eab308 == old triplet 234, 179, 8).


### `.callout {`


Box every callout as an opaque, theme-tinted card.
Non-default callouts (tip/warning/info) lost their box under this theme's
translucent dark mode because core sets `mix-blend-mode: lighten` on
`.callout`, which over the macOS vibrancy backdrop washes a low-alpha tint
out. Fix: force `mix-blend-mode: normal`, paint an OPAQUE base
(--cn-bg-elevated), then layer the per-type tint with color-mix against the
per-type --callout-color. On Obsidian 1.13+ --callout-color is a valid CSS
color, so color-mix resolves reliably (the earlier collapse-to-transparent
was the pre-1.13 RGB-triplet form nested inside color-mix, not color-mix
itself). Per-type colors and Callout Manager customizations flow through.


## 9. BLOCKQUOTES & DIVIDERS


### `.cm-line.HyperMD-hr-line {`


HR rendered in Live Preview (CodeMirror).
Obsidian applies `.HyperMD-hr-line` to the parent .cm-line in BOTH the
rendered and active-edit states; `.cm-hr` is only present on the inline
span when the line is inactive, so `:has(.cm-hr)` misses the active row.
Targeting the line class draws the rule consistently and hides the
dashes whether the cursor is on the line or not. The caret remains
visible for editing, and source mode still shows the raw markup.


## 10. INPUTS, BUTTONS - scoped to Obsidian containers only


### `.modal select,`


Dropdowns - preserve Obsidian's native chevron icon. Use background-color
only; the `background` shorthand would reset background-repeat to `repeat`
and tile the chevron across the select.


### `.modal,`


Modals - restore opaque variables inside dialogs so any internal
Obsidian rule that paints with --background-primary/-secondary gets a
solid surface. Wins over the .theme-dark/.theme-light variable blocks
by equal specificity + later source order.


### `border-radius: 18px;`


18px radius: chunkier than --cn-radius-lg (12px) without going
  full Velocity (22px). Authoritative value -- the section 19b
  duplicate override has been removed.


### `.modal-content,`


Inner surfaces of the Settings dialog and other multi-pane modals


### `.modal-bg {`


Modal backdrop dim is painted by core from --background-modifier-cover
(set in the .theme-dark / .theme-light blocks). This rule only adds the
blur, which core does not set, so it is uncontested.


### `font-family: var(--cn-heading-font);`


var(--cn-heading-font) follows the Style Settings heading font knob
  (same fix as v1.10.14 applied to inline-title).


### `.suggestion-container, .menu {`


Suggestion menu (cmd-P, file switcher)


### `border-radius: 14px;`


14px radius: matches the section 19b chrome model. Authoritative
  value -- the section 19b duplicate override has been removed.


### `box-shadow: 0 8px 32px var(--cn-modal-shadow);`


Per-theme shadow alpha (heavy in dark, soft in light) so popovers
  match the rest of the modal chrome and don't drop a heavy black
  shadow over a light canvas.


## 12. TABLES


### `border-collapse: collapse;`


Kept on border-collapse: collapse. Obsidian core borders every cell on all
  four sides (app.css: `.markdown-rendered td/th { border: ... }`) and merges
  them via collapse into one faint hairline grid. border-radius is ignored by
  collapsed-border tables, so corners stay square - but switching to
  `separate` un-merges that grid into doubled lines and cleaning it up means
  out-specifying core's :first-child/:last-child rules, which is fragile
  across Obsidian updates. Square corners are the safe trade.


## 13. SCROLLBARS - minimal, scroll-triggered on workspace panels


### `::-webkit-scrollbar { width: 10px; height: 10px; }`


13. SCROLLBARS - minimal, scroll-triggered on workspace panels
Default thumb stays visible (modals, settings, code blocks etc.).
Inside .workspace-leaf-content (file explorer, outline, note view,
side panels) the thumb is transparent and only fades in while the
panel is actively scrolling. Requires the local "Scrollbar on Scroll"
plugin (.obsidian/plugins/scrollbar-on-scroll), which toggles the
.is-scrolling class on scroll events with a ~700ms tail.


## 14. STATUS BAR - floating pill, bottom-right


### `body:not(.is-mobile) .status-bar {`


14. STATUS BAR - floating pill, bottom-right
Detached from the bottom edge so it reads as a free-floating
chip in the same idiom as the file-explorer action-icon row.
Sizes to fit its contents.


### `.sync-status-icon.mod-working {`


(Removed the status-bar visibility force: Obsidian no longer dims
status-bar items, so the rule was a no-op.)


### `.sync-status-icon.mod-working {`


Obsidian Sync - paint the working/in-progress state amber so it reads
distinct from the synced (.mod-success) and error (.mod-error) states,
which keep their defaults. Core colors .sync-status-icon.mod-working
via a class rule of equal specificity, so the theme wins on source
order with no !important. The icon SVG inherits this color through
currentColor, so no separate svg rule is needed. --cn-amber-warning is
a dedicated solid status token, intentionally a different shade from
the callout amber (the --callout-color default).


## 15. PLUGIN SAFE-ZONES


### `.excalidraw {`


15. PLUGIN SAFE-ZONES
Plugin UIs (Excalidraw, Smart Connections graph) should render
with their own styles. Theme rules above are scoped narrowly so
they do not leak in. The block below catches any residual leakage.


### `.excalidraw {`


Excalidraw: plugin owns its full UI surface. Targeted overrides only
so Excalidraw's own CSS cascades unimpeded. The previous `all: unset`
approach was a sledgehammer that stripped layout, accessibility
affordances, and inheritance the plugin relies on; v1.10.11 narrowed
it to color/font reset on form controls and a checkbox appearance
restore so checkboxes inside Excalidraw still look like checkboxes
rather than picking up the theme's custom checkbox styling.
If a future audit finds Cognia Noir styles leaking into Excalidraw
form controls, scope a targeted fix to that exact property rather
than reintroducing all: unset.


### `.smart-connections-view canvas { background: revert; }`


Smart Connections related-notes view: leave any inline canvas
element alone. The data-type "smart-connections-view" is the
related-notes panel, not a graph canvas. The canvas selector here
is defensive in case a future Smart Connections build embeds a
canvas inside that view.


## 16. TAGS - pill style, accent-tinted


### `a.tag,`


Pill styling for tags rendered inline in markdown content (a.tag, .tag,
.cm-hashtag) and in the Live Preview hashtag block. The legacy
.cm-tag-pane .tree-item-self selector was removed: it targeted a
CodeMirror 5 era class that no longer matches modern Obsidian's tag
pane, and it collided with the .tree-item-self.is-active rule,
causing a jarring fill/overlay swap on click.


### `.cm-hashtag-begin, .cm-hashtag-end {`


Live Preview hashtag - keep it inline (no pill) so writing flow isn't disrupted;
accent color only.


## 17. PROPERTIES / FRONTMATTER PANEL


### `.metadata-container {`


17. PROPERTIES / FRONTMATTER PANEL
The metadata editor at the top of notes - used for frontmatter keys
like tags, aliases, dates. Minimal, blends into the translucent surface.


## 18. FOCUS INDICATORS - keyboard accessibility


### `a.internal-link:focus-visible,`


18. FOCUS INDICATORS - keyboard accessibility
:focus-visible only fires on keyboard focus, not mouse click,
so we don't pollute the visual when clicking around.


## 19. QUALITY-OF-LIFE - Velocity-inspired refinements


### `.mod-left-split .workspace-tab-header-container-inner,`


19. QUALITY-OF-LIFE - Velocity-inspired refinements

19a. Functional extras (always-on or toggleable)
     - Centered sidebar tab icons
     - Bubble nav (file-explorer action row collapses to pill)
     - Click feedback (scale-down on :active for all icons)
     - CSS class utilities for per-note overrides

19b. Chrome shapes (desktop + iPad, guarded :not(.is-phone))
     - Apple continuous corners on all rounded surfaces
     - Pill-shaped clickable icons (ribbon, sidebar, FABs)
     - Top-right window controls (chevron + sidebar toggle)
     - Top-bar tab pills (bordered pill, 32px, radius 10px)
     - Sidebar tab chips (larger pill, 34px, radius 14px)
     - Vault profile / vault switcher pill
     - View-header right-side action FABs
     - Status bar items
     - Popover / tooltip corner radius (modal/menu radii moved to section 10)
     - Ribbon box, folder panel box, vault-profile pseudo box


### `.mod-left-split .workspace-tab-header-container-inner,`


--- Centered sidebar tab icons ---
Pushes the tab-header-inner (file explorer / bookmarks /
recent files icons) to the horizontal center of the
sidebar header by giving it auto margins. Avoids flex: 1,
which on macOS would stretch the inner under the
absolutely-positioned traffic-light region and clip the
first tab.


### `body.cn-bubble-nav:not(.is-mobile) .nav-header {`


--- Bubble nav buttons (Velocity pattern) ---
Collapses .nav-buttons-container into a slim pill at rest;
expands on hover/focus to reveal the action icons. Mobile
excluded (touch needs persistent affordances). Toggle:
cn-bubble-nav.


### `body.cn-bubble-nav:not(.is-mobile) .nav-header::after {`


Small pill dash visible at rest as the hover affordance.
Fades out when buttons expand so it doesn't compete with them.


### `contain: layout;`


contain: layout isolates the max-height reflow to this element so the
  animation does not cascade to parent/sibling layout recalculations.
  clip-path was evaluated as an alternative (compositor-friendly, no
  reflow) but rejected: clip-path does not collapse layout height, so
  the container would still occupy 40px of space at rest, creating a
  gap that pushes the search input down. max-height remains the correct
  approach; contain: layout limits its propagation cost.
  Section-22 prefers-reduced-motion block neutralizes every transition.


### `body.cn-bubble-nav:not(.is-mobile) .nav-header > .search-input-container {`


Search input below the bubble shouldn't slide as the bubble
expands - give it a stable margin


### `body:not(.is-mobile) .clickable-icon > .svg-icon {`


--- Click feedback on clickable icons ---
Subtle scale-down on press for tactile feedback. Desktop only -
on mobile the OS provides its own tap-highlight, and stacking
our scale animation on top reads as laggy.


### `.hide-metadata {`


--- CSS class utilities (Velocity-inspired) ---
Apply via frontmatter:
  cssclasses: [hide-metadata, hide-title, style-justify, style-wide, style-margin-top]
Each only fires on the note that opts in.


### `.hide-metadata {`


hide-metadata: core computes the properties-block display from
--metadata-display-editing / --metadata-display-reading. Setting them
to none on the opted-in note makes core's own high-specificity rule
resolve to display:none -- no selector battle, no !important.


## 19b. CHROME SHAPES - Velocity-inspired


### `body:not(.is-phone) :is(`


19b. CHROME SHAPES - Velocity-inspired
Brings Velocity's button/tab/icon geometry to Cognia Noir
without changing colors, transparency behavior, or
typography. Targets: top tab bar, ribbon icons, sidebar
toggles, view-header action icons, vault profile, status
bar, modals/menus.
Notes:
  - All radii are literal pixel values so they don't bleed
    into editor content via the global --radius-* tokens
    (which Cognia Noir keeps at the small Notion scale).
  - Continuous (squircle) corners on rounded surfaces are set
    with two properties paired together: the standard CSS
    `corner-shape: squircle` (Obsidian 1.12+, which dropped the
    non-standard property and adopted corner-shape) and the
    legacy `-electron-corner-smoothing: 60%` (pre-1.12 Electron).
    Each era ignores the other's property, so both ship together
    to keep squircles on old and new Obsidian alike.
  - Translucency-aware: shape changes only - no background
    paint that would fight section 4's transparent chrome.


### `body:not(.is-phone) :is(`


--- Apple-style continuous corners on rounded chrome ---
Scoped to :not(.is-phone) for consistency with the rest of
section 19b. The Electron-only -electron-corner-smoothing
property is a no-op on iOS WebKit anyway, but the modal/
menu/popover entries here would still fire on iPad if not
guarded, and on phones the .view-actions clickable-icon
chrome was leaking pill paint into the mobile view-header
and clipping the new-note + icon (v1.10.3 fix).


### `body:not(.is-phone) :is(.workspace-ribbon,`


--- Pill-shaped clickable icons in chrome ---
Ribbon, vault profile, sidebar toggle, top-bar tab list, and
view-header buttons become Velocity's chunky pill shape.
Padding gives them real body so the rounding reads.

:not(.is-phone) guard is load-bearing: on iPhone, applying
min-height:32px + 10px horizontal padding to every view-header
clickable-icon was clipping the + new-note action so the only
visible action was the ... overflow (v1.10.3 fix). iPad keeps
the desktop pill chrome.


### `.workspace-tabs.mod-top .workspace-tab-header-tab-list,`


--- Top-right window controls (dropdown chevron + sidebar
toggle) appear visually grouped in a rounded container in
the reference design. Round the inner icons consistently.


### `body:not(.is-phone) .mod-root .workspace-tabs:not(.mod-stacked) .workspace-tab-header {`


--- Top-bar tab pills ---
Bordered rounded rectangles, always visible, with min-height
for body. Active and hover states get a brighter overlay.
Cognia Noir's existing transparent-until-active behavior is
replaced with a faint persistent outline so the tabs read as
discrete pills (matches the reference screenshot).

Mobile scope decision (locked v1.9.9, do not relitigate):
guarded with :not(.is-phone), NOT :not(.is-mobile). iPad
carries .is-mobile but not .is-phone in Obsidian's body
classes, so this selector intentionally treats iPad as
desktop and gives it the bordered pill chrome. Rationale:
iPad with an external keyboard / Magic Keyboard runs
Obsidian as a near-desktop experience and the desktop
chrome reads better than the iOS-native compact tabs.
Phones still get the native mobile tab styling. The same
:not(.is-phone) guard is applied to every other section
19b chrome rule (sidebar tab pills, view-action FAB,
ribbon box, panel boxes, vault-profile pseudo) for
consistency - flip them together if this decision is
ever revisited.


### `body:not(.is-phone) .mod-root .workspace-tabs:not(.mod-stacked) .workspace-tab-header::before,`


Hide Obsidian's default tab "ear" pseudo-elements that paint
the curved bottom-corner connectors between an active tab and
the bar below it. With our bordered-pill style the tab is a
self-contained shape, so those pseudos render as detached
small circles at the bottom-left/bottom-right of the active
tab. Scope to the same selector chain as the pill rules so
stock styling stays intact wherever pills don't apply.


### `body:not(.is-phone) .mod-root .workspace-tabs:not(.mod-stacked) .workspace-tab-header-inner {`


Inner of the tab keeps its own rounding so the close button
region clips correctly. Removes the section 4 small radius.


### `body:not(.is-phone) .workspace-tab-header-new-tab {`


New-tab "+" button matches the tab pill height so the row
reads as a single band.


### `body:not(.is-phone) .mod-sidedock .workspace-tab-header {`


--- Sidebar tab headers (file explorer / bookmarks tabs) ---
Larger pill so they read as round chips like Velocity's
sidebar tab strip. iPad gets the desktop look (is-phone guard)
to match the rest of section 19b.


### `.workspace-split.mod-left-split > .workspace-sidedock-vault-profile .workspace-drawer-vault-switcher {`


--- Vault profile / vault switcher (bottom-left) ---
Rounded chunk so the dropdown chevron and vault name read
as a single pill button.


### `body:not(.is-phone) .view-actions .clickable-icon:not(.mod-bookmark) {`


--- View-header right-side action icons ---
In the reference, the "+" and reading-mode toggle (the
round "60"/glasses icon) are circular FAB-style buttons
with a faint border. Apply to non-bookmark view-actions
icons in markdown leaves only - the bookmark star stays a
regular pill so it doesn't visually compete.


### `body:not(.is-phone) .mod-root .view-header .view-actions {`


Breathing room around the FAB cluster. The icons sit too close
to the tab pill above and to the window's right edge by default,
so they read as crammed into the corner. Small top padding pushes
the whole group down a few pixels, and the right padding shifts
them inward. margin-left: auto pins .view-actions to the right
edge so any future stock-layout change in Obsidian cannot drift
the action icons left again. Scoped to non-phone in line with
the FAB rule above -- mobile uses native iOS spacing.


### `body:not(.is-phone) .mod-root .view-header-title-container .view-header-title {`


Hide the centered .view-header-title text. With pill tabs
(which already show the file name) AND the inline-title at
the top of the note body (large bold), the view-header's
small centered title is a third redundant copy of the same
string. The view-header itself stays in the layout so the
right-side action icons (the FAB rule above) keep their
container; only the title text is suppressed. mod-root scope
keeps sidebar leaf headers (search, outline, etc.) untouched
in case any of those rely on a title to identify the leaf.

visibility:hidden (NOT display:none) on the inner title is
load-bearing: the .view-header-title-container element carries
flex:1 in Obsidian's stock layout and acts as the spacer that
pushes .view-actions to the right edge. Collapsing the
container with display:none lets .view-actions slide flush
left, which is the bug v1.10.3 fixed. Keep this rule targeting
the inner .view-header-title only.


### `.status-bar-item.mod-clickable {`


--- Status bar items -
Section 14 handles the parent pill (position, radius, body).
This block only styles the clickable items inside.


### `.popover,`


--- Popover / tooltip corner radius ---
Modal (18px) and suggestion-container/menu (14px) radii are set
directly in section 10 where those elements are first defined.
Popovers and tooltips aren't in section 10 so they stay here.


### `body:not(.is-phone) .workspace-ribbon.mod-left {`


--- Ribbon, folder, and vault-profile boxes ---
The workspace-ribbon, sidebar leaf content, and vault
profile (bottom-left) all become bordered rounded panels
with matching margin and radius so they read as sibling
panels stacked in the left column. Border-only fill so
they work in both opaque and translucent modes without
fighting section 4 paint logic. All scoped to non-mobile -
on mobile (phone or tablet) Obsidian uses drawer layouts
where these box rules would interfere with the native UI.


### `margin: 36px 4px 8px 8px;`


Default top margin (36px) lines the ribbon-box top up with
  the folder-box top in non-frameless / non-macOS layouts:
  sidebar tab-pills (~34px min-height per section 19b) plus a
  small nudge. The macOS frameless override below adds extra
  clearance for traffic lights. Tested on Linux GTK windowed
  and macOS with native title bar; if drift appears on a
  specific platform, override with another body-level rule
  rather than tweaking this default.


### `align-self: stretch;`


Load-bearing per v1.8.6: without these the ribbon container
  sizes to its icon list, which leaves the bottom of the box
  clipped above the vault profile. Confirmed still needed on
  Obsidian 1.5+ - Obsidian's flex layout assigns the ribbon
  intrinsic content height by default.


### `body:not(.is-phone) .workspace-ribbon.mod-left.is-collapsed {`


When the left sidebar is collapsed only the ribbon box shows,
sitting against the workspace. The base rule's right margin (4px)
reads as cramped there, so balance it to match the 8px left margin
in the collapsed state only; expanded layout keeps 4px because the
file-explorer panel box already supplies the gap on that side.


### `body.mod-macos.is-hidden-frameless:not(.is-phone) .workspace-ribbon.mod-left {`


macOS frameless: bump the ribbon top so the box clears the
traffic-light region (which floats over the workspace when
the native title bar is hidden) AND aligns with the folder-
box top in that layout. mod-macos already excludes iOS;
:not(.is-phone) is for consistency with section 19b.


### `body:not(.is-phone) :is(.mod-left-split, .mod-right-split) .workspace-leaf-content:not([data-type="file-explorer"]):not([data-type="terminal"]):not([data-type="excalidraw"]) .view-content,`


Sidebar leaf "panel boxes" - bordered rounded panels around
the content area of every sidebar leaf, so plugin views (Smart
Connections related-notes, Dataview, Periodic Notes, Outliner,
etc.) pick up the same chrome as the core leaves without
needing to be enumerated. File Explorer is special-cased on
its own line because its scroll container is
.nav-files-container, not .view-content.

Exclusion list rationale (panel-box-specific, NOT inherited
from section 4):
  - terminal: xterm canvas needs the full leaf surface; a
    bordered margin would crop the prompt.
  - excalidraw: full-canvas drawing surface, same reason.
Any other plugin leaf gets the box. Don't reflexively copy
section 4's translucent-force-paint exclusion list here -
that list exists to avoid bg-primary collisions over vibrancy
and includes views (e.g. smart-connections-view) that are
normal scrollable lists and benefit from the panel box. The
two concerns are separate; only add a data-type to this list
if its content actively fights a margin-bordered container.

The horizontal inset (--cn-panel-side-inset) is shared with
the vault-profile pseudo box below so the two panels stay
edge-aligned.


### `body:not(.is-phone) .workspace-split.mod-left-split > .workspace-sidedock-vault-profile {`


Vault profile (bottom-left strip with vault switcher,
help, settings gear). The vault-profile is a flex child
whose width is forced to the full sidebar width by the
parent's flex stretch - margins on the element itself
don't change its rendered width or position reliably.
Solution: leave the element at its natural full-sidebar
width and draw the visual box via a pseudo-element with
absolute positioning, anchored by --cn-panel-side-inset
on left and right so the box matches the folder-box's
horizontal extent.

Alignment dependency: this works only because the vault-
profile's parent (.workspace-split.mod-left-split) is the
same width as the file-explorer column. If a workspace
plugin ever relocates the vault profile into a parent of
a different width, this pseudo will drift relative to the
folder box. The shared --cn-panel-side-inset var keeps
the inset values locked together; the parent-width
assumption still has to hold for the alignment to read.
See section 19b head-comment for the full chrome model.


### `border-top: none;`


Suppress the top border core reintroduced in Obsidian 1.12
  (border-top via --tab-outline-width/--tab-outline-color on
  .workspace-sidedock-vault-profile). That core rule is
  specificity (0,4,1); this rule matches it and, loading after
  core, wins by source order, so no !important is needed.


### `.theme-dark .workspace-split.mod-root .workspace-leaf-resize-handle {`


v1.10.10  Removed the rule that hid hr.workspace-leaf-resize-handle
inside .workspace-split.mod-left-split. In current Obsidian the
sidebar-width drag handle IS an hr.workspace-leaf-resize-handle that
is a direct child of .mod-left-split, so the old rule killed the
sidebar resize cursor and drag interaction. The original target (a
decorative hr between file-explorer column and vault profile) does
not appear in the current DOM; if it returns, scope by a more
specific selector that excludes the boundary handle.


### `.theme-dark .workspace-split.mod-root .workspace-leaf-resize-handle {`


v1.10.23  Vault-profile top-border suppression moved into the
(0,4,1) box rule above (border-top: none). Obsidian 1.12
reintroduced this border via --tab-outline-width/-color with a
(0,4,1) selector, which outranked the old low-specificity
safeguard here once v1.10.21 dropped its !important. That is
what produced the visible white line. The standalone block is
removed; the suppression now lives where it can win on
specificity + source order without !important.


### `.theme-dark .workspace-split.mod-root .workspace-leaf-resize-handle {`


v1.10.15  Split-pane resize divider visibility.
DevTools confirmed: the element is hr.workspace-leaf-resize-handle
(not .workspace-leaf-resize-divider), and Obsidian renders the
visible line via border-inline-end (not background-color). Scoped to
.mod-root so the sidebar resize handle is not affected.
Values are fixed rgba overlays calibrated for findability at rest and
a clear brightening on hover, without being as stark as the original
hardcoded #b8b8bb (dark) / #888888 (light). Using pure --cn-divider
(0.22 opacity) was a v1.10.16 regression: on Pure Black it rendered
at ~2:1 contrast, below the 3:1 interactive-element minimum.


## 20. PRINT / PDF EXPORT


### `@media print {`


20. PRINT / PDF EXPORT
The workspace-transparency rules in section 4 are scoped to
@media screen, so print/PDF export inherits opaque surfaces with
no override needed here. This block only strips decorative chrome.


### `.modal, .suggestion-container, .menu { box-shadow: none; }`


Drop shadows and decorative blur in print


## 21. MOBILE - settings card contrast, button visibility,


### `body.is-phone.theme-dark .modal.mod-settings .vertical-tab-content,`


21. MOBILE - settings card contrast, button visibility,
    and drawer view-picker chrome
On iPhone, Obsidian groups the Settings screen content into
stacked cards (Account, Catalyst license, Help, etc.) that
paint with --background-secondary. In dark mode our --cn-bg-
primary (#1d1d1d) and --cn-bg-secondary sit only ~3-8 units
apart (intentional for desktop note-vs-tab-tray tone), which
makes the cards nearly indistinguishable from the page. Light
mode is unaffected because the equivalent values are far
enough apart and stock iOS Obsidian renders them as crisp
white cards.

This block is phone-only. iPad inherits the desktop modal
chrome (section 10) and is fine. Light mode card surfaces
are opaque by default so no fill is needed here; the button
fix below covers both modes.


### `.modal.mod-settings button:not(.mod-cta):not(.clickable-icon) {`


Non-CTA settings buttons (Open, Manage, Log out, Upgrade,
Purchase, plus Turn on and reload / Browse / Check for updates)
are bare text without this, because section 10's broad
`.modal button { background: transparent }` strips the fill core
gives `button:not(.clickable-icon)`. Give them a tertiary fill +
subtle border so they read as buttons, not floating labels. The
blue Activate button (.mod-cta) keeps its accent paint from
section 10. v1.10.23: unscoped from .is-phone so it applies on
desktop too, since Obsidian 1.12's settings layout surfaced these as
plain text on desktop. .modal.mod-settings (0,3,1) outranks the
broad .modal button (0,1,1). :not(.clickable-icon) mirrors core's
own fill boundary (button:not(.clickable-icon)) so settings icon
buttons (hotkey edit/delete, extra-option gears) stay bare and
are not boxed. Dark and light mode both.


### Community-plugin settings-tab icons (two rules)

`.vertical-tab-header-group-items[data-section="community-plugins"] .vertical-tab-nav-item-icon`
and
`.vertical-tab-header-group-items[data-section="community-plugins"] [data-setting-id^="smart-"]:not([data-setting-id^="smart-environment"]) .vertical-tab-nav-item-icon.vertical-tab-nav-item-icon`

Goal: every community plugin's settings-tab icon hidden, so the rows read
uniformly (no icon, flush left). Obsidian 1.13+ core already hides them at
specificity (0,3,0). The complication is Smart Connections.

Smart Connections does NOT use an inline style. It injects a STYLESHEET rule at
runtime (verified in its `main.js`, the `apply_style_sheet(settings_default)`
path) that re-shows its own icons:

    .vertical-tab-header-group-items[data-section="community-plugins"]
      [data-setting-id^="smart-"]:not([data-setting-id^="smart-environment"])
      .vertical-tab-nav-item-icon { display: flex; }

That selector is specificity (0,5,0) -- it out-specifies core's (0,3,0) AND
loads later, so SC's icon wins and reintroduces a margin-inline-end indent on
the Smart Environment / Connections rows.

Fix (v1.10.30, replacing the prior `display: none !important`): two non-!important
rules. The first re-asserts the broad hide at (0,3,0). The second mirrors SC's
exact selector and doubles the trailing class to reach (0,6,0), which beats SC's
(0,5,0) outright on specificity -- no load-order dependence, no !important. This
was the theme's last remaining `!important`; removing it keeps the file lint-clean
(the project has progressively removed all `!important` since v1.10.19/1.10.21).

Known fragility (accepted): if a future Smart Connections release raises its own
selector specificity to >= (0,6,0), the second rule stops winning and needs a
re-tune. The old `!important` was immune to that, but carried the lint warning.


### `.vertical-tab-header-group-items[data-section="community-plugins"] .vertical-tab-nav-item-title {`


Smart Connections also prepends literal leading spaces to its tab names
("  Smart Environment" with two spaces, " Connections" with one), and core's
`.vertical-tab-nav-item-title { white-space: pre }` preserves them - which is why
those two rows indented by different amounts even with the icon hidden. Collapse
leading whitespace on community-plugin titles. nowrap (not normal) keeps core's
single-line + text-overflow: ellipsis behavior intact. No !important needed: this
outranks core's plain `.vertical-tab-nav-item-title` rule on specificity.


### `body.is-mobile .workspace-drawer-tab-options-list {`


Mobile drawer view-picker popup. The dropdown that opens from
the bottom-of-drawer chevron lists Files / Search / Tags /
Bookmarks / All properties / Recent files.

Core DOM (verified in obsidian.asar):
  .workspace-drawer-active-tab-container
    .workspace-drawer-tab-options                      <- container
      .workspace-tab-header.workspace-drawer-tab-select <- always-visible
                                                           "current view" bar
                                                           with the chevron
      .workspace-drawer-tab-options-list               <- the popup list
                                                           that floats open
        .workspace-tab-header (one row per view; active
                               row is .is-active)

The container holds BOTH the always-visible select bar AND the
floating list, so chrome (border/shadow/radius) must go on the
floating LIST, not the container -- a box on the container wraps the
select bar too and frames the whole bottom of the drawer. The list
is what overlays the file tree when open, so it must be fully opaque
or the folder names behind it bleed through. Give the list real
floating-menu chrome (opaque elevated fill + border + radius +
shadow, matching the .menu treatment at line ~1328); leave the
container and the select bar to their stock layout. Scoped to
.is-mobile so iPhone and iPad both pick it up; iPad rarely surfaces
this popup but the chrome is harmless there.
Velocity additionally paints the whole .workspace-drawer-inner
for cohesive chrome; that approach tints the iOS status bar
safe area, which we don't want here, so we stop at the picker.


### `}`


Do NOT set `padding` here. Core gives the list
  `padding-top: var(--touch-size-l)`, which reserves a strip for the
  always-on-top .workspace-drawer-tab-select bar (z-index:
  var(--layer-cover)). A `padding` shorthand wipes that reservation
  and a row slides under the select bar. Recolor only; keep stock
  padding.


### `body.is-mobile .workspace-drawer-tab-options-list .workspace-tab-header {`


Row styling only -- no size/spacing overrides. Core lays the rows
out in its own flow; setting min-height / padding / gap on the
.workspace-tab-header(-inner) made the rows taller than that flow
expects and the first and last collided. Limit ourselves to the
rounded corner (cosmetic, no layout effect) and the active fill.
Scoped inside the list so the always-visible
.workspace-drawer-tab-select bar (a sibling, not a child) is left
alone.


### `body.is-mobile .workspace-drawer-tab-options-list .workspace-tab-header.is-active {`


Clear but restrained active highlight -- tertiary fill reads on the
elevated list surface where the faint --cn-soft-overlay from
section 4 did not.


### `body.is-mobile .workspace-drawer-tab-options:not(.is-collapsed) .workspace-drawer-tab-select .workspace-tab-header-inner,`


Stop the "highlighted twice" look. The always-present
.workspace-drawer-tab-select bar (bottom of the picker, carries the
collapse chevron) mirrors the current view, so core marks it
.is-active and paints its inner with --nav-item-background-hover --
the same highlight the active row in the list already shows. When
the picker is open that bar is redundant, so drop its fill so it
reads as a plain collapse control, not a second highlighted "Files".
Two selector forms cover both DOM shapes (the select bar is a
sibling of the list in the desktop build but appears nested in the
mobile list rules). Specificity (>= 0,5,1) beats core's
.workspace-tab-header.is-active .workspace-tab-header-inner (0,5,0).
Closed state is untouched, so the bar keeps its resting pill.


## 22. REDUCED MOTION -- zero the transition-duration tokens for


### `@media (prefers-reduced-motion: reduce) {`


22. REDUCED MOTION -- zero the transition-duration tokens for
users who enabled Reduce Motion. Every transition in the theme
uses these two variables, so redefining them to 0s disables all
motion without an !important universal reset. The theme has no
@keyframes animations.
