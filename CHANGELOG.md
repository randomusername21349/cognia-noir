# Changelog

Full version history for the Cognia Noir Obsidian theme. These entries
previously lived in the theme.css header comment and were moved here in
v1.10.24 to keep the stylesheet small (Obsidian flags large theme CSS
files). Recent releases are also on the GitHub releases page. Entries in
the "Earlier history" section are preserved verbatim from the old header,
including their original wording and punctuation.

## v1.10.29

Separate dark / light background pickers.

- Background tone is now two Style Settings controls instead of one: "Dark mode
  background" (Charcoal default, Slate, Graphite, Pure black, True black, Warm, Zen)
  and "Light mode background" (Paper default, Pure white, Mist, Stone, Warm, Zen).
  Each affects only its own mode, so changing the dark surface no longer touches light
  and vice versa. The names are now mode-appropriate rather than dark-centric. (Setting
  IDs changed, so the tone choice resets to the defaults once on update.)
- Verified the scrollbar is consistent across all tones (it uses mode-level overlay
  vars, untouched by tones or the grayscale toggle), and audited plugin compatibility:
  the variable-based theming means all tones and both modes propagate to plugins via
  core surfaces; custom-UI plugins (Excalidraw, Smart Connections, Terminal, Canvas)
  stay opted out of the translucency force-paint and own their surfaces.

## v1.10.28

Settings window polish for Obsidian 1.13.

- Community plugins list: re-assert core 1.13's hide-community-plugin-icons behavior
  (Smart Connections forces its icon back via an injected stylesheet) and collapse the
  leading spaces Smart Connections prepends to its tab names ("  Smart Environment",
  " Connections"), which core's `white-space: pre` was preserving as an indent. The
  section now reads flush and uniform. Tab ordering is set by the plugins/Obsidian and
  is left untouched.

## v1.10.27

Obsidian 1.13 compatibility, new black-and-white appearance options, plus a
documentation and cleanup pass.

- Background tone: three new options in Style Settings. Slate (neutral gray) and
  Graphite (deep neutral) are pure grayscale surfaces with no warm or Zen hue;
  True black is a pure #000000 canvas for maximum OLED contrast (inverts to crisp
  pure white in light mode). All ship with dark, light, and sidebar-lifted values.
- Grayscale UI: new Style Settings toggle (off by default) that desaturates the
  theme's semantic colors (accent, links, callouts, tags, highlight, inline code,
  status) so the interface reads as pure black and white. Implemented as a token
  remap, not a CSS filter, so note images and code syntax highlighting keep their
  color and the translucency/backdrop system is untouched.
- Callouts: migrated `--callout-color` handling to the Obsidian 1.13 contract.
  1.13.0 changed `--callout-color` from an RGB triplet to a valid CSS color (a
  breaking change), so non-note callout types (tip/warning/info and others) and
  Callout Manager custom colors stopped painting their box, border, and icon on
  1.13+. The theme now consumes `--callout-color` directly and uses `color-mix`
  for the translucent tints, the same pattern used elsewhere in the theme.
  Verified rendering with hex, `rgb()`, and `oklch()` colors in dark and light.
- Settings: 1.13.0 moved Settings into a separate window. Verified its root still
  carries `.modal.mod-settings`, so the existing settings selectors still match
  the redesigned window; no selector change was made.
- README: corrected the editor font default (14, not 16), pointed the changelog
  reference to CHANGELOG.md (was "top of theme.css"), and removed a stale
  bubble-nav highlight that no longer matched the code.
- Housekeeping: replaced non-ASCII em dashes (and an arrow in this changelog)
  with ASCII equivalents in comments and docs.

## v1.10.26

Readability and cohesion pass on note content (dark mode).

- Callouts: tip/warning/info now render as boxed cards in translucent dark mode.
  They previously kept only their colored icon and lost the background/border,
  because core's `mix-blend-mode: lighten` over macOS vibrancy washed the
  low-alpha tint out. Callouts now paint an opaque tinted surface with
  `mix-blend-mode: normal`, so every type reads as a solid card while keeping its
  semantic color.
- Live Preview headings: heading sizes were being flattened to the editor font
  size because the editor's `.cm-line` font-size rule out-specified the heading
  rules. Header lines are now excluded, so H1-H6 size correctly in Live Preview
  as well as reading view.
- Headings restyled: tighter, Notion-like scale (H1 is no longer larger than the
  note title), a single uniform weight and one uniform color (white in dark /
  near-black in light) across H1-H6 so hierarchy comes from size, set in SF Pro
  Text to match the body. Heading letter-spacing relaxed for the smaller scale.
- Highlight: brighter `==highlight==` yellow in dark mode.
- Translucency: in dark translucent mode the sidebar and note now share one
  cohesive subtle-glass surface instead of a solid note card against glassy
  chrome. The Translucency tint knob now sets that glass opacity (Pure = most
  vibrancy, Heavy = nearly solid).
- Default note font size is now 14px (was 16px); still adjustable via Style
  Settings.

## v1.10.25

Mobile (iPhone) fix for the left-drawer view-picker popup (the list of
Files / Search / Tags / Bookmarks / All properties / Recent files that
opens from the bottom-of-drawer chevron). The popup previously inherited
only a flat fill matching the drawer background, so it did not read as a
distinct surface: the file tree behind it bled through and the rows looked
like loose items floating over the folders. The popup list
(.workspace-drawer-tab-options-list) now gets opaque floating-menu chrome
(elevated fill, subtle border, radius, and the shared menu shadow) placed
on the floating list itself rather than the wrapping container, so it no
longer frames the whole bottom of the drawer. Stock padding is left intact
so core's reserved strip for the always-on-top select bar is preserved
(an earlier padding shorthand had pushed a row under the select bar). The
redundant active highlight on the select bar is dropped while the picker
is open, so the current view no longer appears highlighted twice. Desktop
and iPad are unaffected (their sidebar-tab styling is guarded
:not(.is-phone)).

## v1.10.24

Obsidian 1.12.x desktop appearance fixes (vault-profile white line,
settings buttons reading as plain text, cramped collapsed ribbon, and
lost squircle corners; see the v1.10.23 entry below for the technical
detail). This release also moved the full changelog out of theme.css into
this file to resolve an Obsidian "theme CSS file is large" performance
notice, and rephrased recently added code comments to ASCII.

## Earlier history (relocated from the theme.css header)

v1.7.2 (light mode opaque Notion-style + force-paint outer shell
        when Translucent Window is enabled in Obsidian settings.
        Obsidian's own internal CSS makes surfaces transparent in
        is-translucent mode regardless of theme; we now explicitly
        paint sidebar/ribbon/tabs/titlebar with --cn-bg-secondary
        and editor area with --cn-bg-primary to override that.)
v1.7.3 (audit pass: WCAG AA fixes for light-mode accent, inline code,
        text-secondary, text-faint; brighter dark inline-code to clear
        AA on bg-tertiary; fixed sidebar-lifted no-op in light
        translucent (force-paint was using --cn-bg-secondary, bypassing
        the variant override of --background-secondary); removed dead
        .cm-tag-pane pill selector that collided with active state;
        tokenized popover shadow and print modal-bg backdrop;
        opted plugin leaves out of translucent leaf-content force-paint;
        scoped .cm-line padding to markdown source view; documented
        the three remaining !important declarations.)
v1.7.4 (perf audit: verified no backdrop-filter on scrolling containers,
        no transition: all, no will-change, no :has(), no @keyframes,
        no brightness/contrast filters, no universal-selector traps.
        Theme is perf-clean. Corrected header comment that misdescribed
        Highlightr as an opted-out leaf - it has no leaf to opt out of.
        When adding a plugin opt-out: update the translucent-dark
        :not() chain (section 4, dark block), the translucent-light
        :not() chain (section 4, light block), and the .mod-root
        :not() chain (section 4b). Search "workspace-leaf-content:not"
        to locate all three. Verify the plugin registers a workspace
        leaf before adding it.)
v1.7.5 (note panel softened: dark Charcoal --cn-bg-primary lifted from
        #191919 to #1d1d1d so the note sits closer to the sidebar tone,
        and added section 4b to round the note's top-left and
        bottom-left corners (left edge against the sidebar). Note
        panel forced solid in dark translucent mode so the color
        difference reads. Variant tones (Pure black, Warm, Zen) keep
        their existing primary values intentionally.)
v1.7.6 (rounded note corners now visible in light mode and in dark
        non-translucent mode: paint the workspace-tab-container behind
        the leaf with --background-secondary so the rounded leaf
        reveals the sidebar tone at the corners, and add overflow:
        hidden to leaf-content so the .view-header above the note
        body clips to the rounded shape instead of painting over it.
        Tab-container stays transparent in dark translucent mode so
        macOS vibrancy still shows through.)
v1.7.7 (rounded note corners were technically rendering but invisible
        because --background-secondary is only ~3-8 units off the note
        color in both modes, so the rounded reveal looked square.
        Introduced --cn-bg-note-tray as a dedicated tray tone with
        real contrast against the note: #131313 in dark (10 units
        darker than the #1d1d1d note), #d8d6d1 in light (~30 units
        darker than the #ffffff note). Section 4b now uses this var.
        Tray is intentionally darker than the note in both modes so
        the note reads as a lifted card on a base, consistent across
        dark and light.)
v1.7.8 (extended section 4b to round the sidebars too. Left sidebar
        rounds top-right and bottom-right; right sidebar rounds
        top-left and bottom-left. Each sidebar's tab-container is
        painted with --cn-bg-note-tray so the rounded inside edges
        reveal the same tray tone the note panel sits on. Outer
        edges stay flat against the window. Tab-container trays
        opt out of opaque paint in dark translucent mode so vibrancy
        still shows through the chrome.)
v1.7.9 (sidebar rounding only worked at the top because the file
        explorer's top action-icons row and bottom vault switcher
        sit outside .workspace-leaf-content, so the leaf-content
        clip didn't include them. Moved sidebar rounding up one
        level to .workspace-tabs so the entire pane (including the
        action row and vault switcher) clips to the rounded shape.
        Tray paint also moved up one level from .workspace-tab-
        container to .workspace-split so the higher-level rounded
        shape reveals the tray. Existing section 4 dark-translucent
        transparency on .workspace-split still wins by specificity.)
v1.7.10 (v1.7.9 broke the note rounding because painting only
         workspace-split with tray meant the note's rounded leaf-
         content revealed the unpainted tab-container in between
         (default white). Now paint both .workspace-split AND its
         .workspace-tab-container with tray so whichever parent is
         "behind" the rounded element shows the right tone. Note
         rounding (on leaf-content) and sidebar rounding (on
         workspace-tabs) both work. Tab-container trays opt out of
         opaque paint in dark translucent mode so vibrancy still
         shows through.)
v1.10.23 (Obsidian 1.12.x desktop appearance fixes, verified
         against the shipped 1.12.7 app.css. (1) Vault profile:
         core reintroduced a top border via
         --tab-outline-width/-color with a (0,4,1) selector that
         outranked the old low-specificity safeguard once 1.10.21
         dropped its !important, producing a white line over the
         vault box. Suppression moved into the (0,4,1) box rule
         (border-top: none) where it wins on specificity + source
         order, no !important. (2) Settings buttons: the broad
         .modal button transparent rule stripped core's fill, so
         Browse / Check for updates / Turn on and reload read as
         plain text on desktop; the non-CTA settings-button fill
         is now unscoped from .is-phone and excludes .clickable-icon
         to match core's own fill boundary. (3) Collapsed left
         ribbon: balanced the cramped 4px right margin to 8px in
         the .is-collapsed state only. (4) Squircle corners:
         core 1.12 dropped -electron-corner-smoothing for the
         standard corner-shape; added corner-shape: squircle
         alongside the legacy property so squircles render on both
         old and new Obsidian.)
v1.10.22 (Republish of 1.10.21 -- no CSS changes; theme.css is
         byte-identical. The developer-portal CSS scan for 1.10.21
         ran before the release file assets were attached and
         linted stale (pre-fix) content; the portal scans once per
         version, so a new version is needed to trigger a fresh
         scan. The 1.10.22 release carries theme.css + manifest.json
         as assets from creation.)
v1.10.21 (Obsidian community theme lint cleanup. Removed all 28
         !important declarations flagged by the linter. Approach
         verified against Obsidian 1.12.7 app.css: rather than
         fight selector specificity, set the CSS variable Obsidian
         already reads. Tree-item active state -> --nav-item-
         background-active / --nav-item-color-active; CM6 and
         ::selection -> --text-selection (rule deleted, theme
         already defines the variable); codeblock background ->
         --code-background; modal backdrop dim -> --background-
         modifier-cover. hide-metadata -> --metadata-display-
         editing/-reading set to none on the opted-in note, so
         core's own rule resolves to display:none. Sync working
         state collapsed to one .sync-status-icon.mod-working
         rule (class-based core rule, theme wins on source
         order; svg inherits via currentColor). The status-bar
         visibility force was deleted -- Obsidian no longer dims
         status-bar items. Section-4 workspace-transparency rules
         are now wrapped in @media screen, so print/PDF export
         inherits opaque surfaces and the print block no longer
         needs to re-paint anything. Reduced-motion redefines
         --cn-transition/-fast to 0s instead of an !important
         universal reset; dead `animation: none` deleted (no
         @keyframes in the theme). Declaration count: 28 -> 0.)
v1.10.20 (docs cleanup of v1.10.19. Rewrote the rationale
         comment on the vault-profile border-top rule so it
         describes the current no-!important state instead of
         the pre-cleanup state, and added retroactive version-
         log entries for 1.10.19 and 1.10.20. No CSS behavior
         change.)
v1.10.19 (Obsidian community theme lint cleanup. Dropped 5
         redundant !important flags whose selector specificity
         already outranks Obsidian core: vault profile
         border-top, and the four resize-handle
         border-inline-end-color declarations (dark/light at
         rest and on :hover). Visually verified in both
         themes; divider remains visible at the documented
         rgba values and hover still brightens. Declaration
         count drops from 33 to 28. Remaining 28 are each
         paired with a rationale comment explaining the
         Obsidian-core override they counter.)
v1.10.18 (mobile drawer fix + audit cleanup. (1) MOBILE BUG:
         on iPhone, tapping the bottom-of-drawer view-picker
         chevron (Files / Search / Tags / Bookmarks / All
         properties / Recent files) opened a transparent popup
         -- the file-explorer leaf behind it bled through and
         both layers were simultaneously legible. Stock
         Obsidian mobile doesn't paint .workspace-drawer-tab-
         options or .workspace-drawer-tab-options-list; it
         expects the theme to. Cognia Noir was missing both
         paints. Added section-21 rule painting both with
         --cn-bg-secondary. Verified live with a red-paint
         diagnostic that CSS updates were reaching the device
         before shipping the dark-gray paint. Velocity's
         additional .workspace-drawer-inner paint was
         considered and rejected: it tints the iOS status bar
         safe-area, unwanted side effect.
         (2) DEAD CSS: .workspace selector removed from the
         section-4 translucent-force-transparent multi-
         selector list -- it was immediately overridden one
         rule below by the workspace-wash that sets it to
         var(--workspace-background-translucent). Including it
         in the transparent list was dead code at equal
         specificity. Comment added in its place to prevent
         re-adding.
         (3) MOBILE GUARD: cn-sidebar-border rules
         (.theme-{dark,light}.cn-sidebar-border .workspace-
         split.mod-{left,right}-split) now scoped with
         body:not(.is-mobile). On mobile, .workspace-split.mod-
         {left,right}-split is the drawer container; a border
         there draws a line on the drawer's inner edge. Same
         scope-decision pattern as v1.8.7 / v1.9.9 for other
         chrome.
         (4) TOKEN EXTRACTION: --cn-amber-warning added to
         both .theme-dark and .theme-light blocks (solid hex
         #ffd54f). Three hardcoded #ffd54f uses on
         .sync-status-icon replaced with var() references.
         Comment on the sync rule rewritten to clarify that
         the sync amber and callout amber (RGB triplet via
         --callout-color) are intentionally different shades
         and serve different purposes (solid status indicator
         vs alpha-composable callout tint).
         (5) TOKEN USE: .modal button.mod-cta hardcoded
         color: #ffffff replaced with var(--text-on-accent),
         matching the rest of the theme's token discipline.
         (6) DOC DRIFT: two stale line-number references in
         historical changelog entries (v1.10.3 and v1.10.11)
         now point to section/comment names instead of
         specific lines -- sections are stable, line numbers
         drift on every edit.
         (7) SECTION HEADER: section 21 renamed from "MOBILE
         SETTINGS -- card contrast + button visibility" to
         "MOBILE -- settings card contrast, button visibility,
         and drawer view-picker opacity" to reflect the new
         drawer block.)
v1.10.16 (audit pass: bugs, dead CSS, and performance. (1) BUG: Pure
         Black dark variant -- --cn-bg-note-tray was inheriting
         #131313 from .theme-dark while --cn-bg-primary is #0d0d0d,
         making the tray LIGHTER than the note and inverting the
         lifted-card hierarchy. Fixed: --cn-bg-note-tray: #050505
         added to .theme-dark.cn-bg-black. (2) BUG: .modal-title
         font-family was hardcoded to "SF Pro Display" -- missed in
         the v1.10.14 pass that fixed inline-title. Changed to
         var(--cn-heading-font) so New York / Georgia Style Settings
         selections apply to modal titles. (3) BUG: Sidebar border
         option (cn-sidebar-border) added border-left AND border-right
         to both sidebars, including the outer edge against the window
         frame. Fixed: left sidebar gets border-right only; right
         sidebar gets border-left only. (4) BUG: Resize divider colors
         were hardcoded hex (#b8b8bb dark / #888888 light) that do not
         adapt to Pure Black, Warm, or Zen variants. Replaced with
         var(--cn-divider) / var(--divider-color-hover). Transition
         changed from raw 0.15s ease to var(--cn-transition-fast) to
         match the theme token system. (5) DEAD CSS: .modal in section
         10 set border-radius: var(--cn-radius-lg) (12px) that was
         always overridden by the section 19b 18px value; .suggestion-
         container, .menu similarly set var(--cn-radius) (8px) always
         overridden by 14px. Consolidated: authoritative values now
         live in section 10; section 19b duplicate overrides removed.
         (6) DEAD CSS: --h1-size through --h6-size, --h1-weight through
         --h6-weight, --h1-line-height through --h6-line-height,
         --cn-editor-font-size, --cn-note-line-height, and
         --cn-heading-font were defined identically in both .theme-dark
         and .theme-light. Moved to :root following the same pattern as
         --inline-title-* vars in v1.10.9. Themed blocks now only
         contain mode-sensitive tokens. (7) DEAD CSS: Two identical
         body:not(.is-phone).mod-root.view-header .view-actions
         selectors (padding and margin-left) merged into one rule.
         (8) DEAD CSS: Light Zen cn-sidebar-lifted tray was #cdcbc6 --
         the same as Charcoal, which lacks Zen's warm hue. Changed to
         #c8c5c0 (one stop darker, warm undertone). (9) PERF: Added
         contain: layout to .nav-buttons-container so the max-height
         hover animation does not cascade layout recalculations to
         parent/sibling elements. clip-path was evaluated as a
         compositor-friendly alternative but rejected: it does not
         collapse layout height, leaving a 40px gap at rest.
         CORRECTIONS (found during post-session review): (a) BUG-1
         left cn-sidebar-lifted.cn-bg-black at #050505, identical to
         the new default Pure Black tray -- Lifted had no visible
         effect. Updated to #000000. (b) BUG-4 used var(--cn-divider)
         (rgba 0.22) for the resize divider default state; on Pure
         Black this renders at ~2:1 contrast, below the 3:1 minimum
         for interactive elements. Replaced with rgba(255,255,255,0.35)
         dark / rgba(55,53,47,0.28) light -- visible and non-stark.)
v1.10.15 (split-pane resize divider visibility. --divider-color was
         not set by the theme, so it resolved through
         --background-modifier-border -> --cn-border-subtle ->
         rgba(255,255,255,0.06) in dark mode, making the center line
         between split windows nearly invisible. Fixed: --divider-color
         now set to --cn-divider (rgba(255,255,255,0.22) dark /
         rgba(55,53,47,0.16) light), matching the value already used
         for hr elements in section 9. --divider-color-hover added for
         a visible affordance on hover. A 0.15s transition on
         .workspace-leaf-resize-divider gives a smooth brighten effect.)
v1.10.14 (three bug fixes. (1) Bubble nav reverted from v1.10.12's
         scale(0.65)/opacity approach back to a height-collapse model:
         .nav-buttons-container collapses to max-height:0 at rest and
         expands to max-height:40px on hover/focus-within. A 28x3px
         pill-dash indicator added via ::after on .nav-header gives a
         visible hover affordance when buttons are folded. (2) Light
         mode missing variables: --cn-note-line-height and
         --cn-heading-font were defined only in .theme-dark; in light
         mode note content fell back to browser-default line height and
         headings fell back to Inter. Both variables now defined in
         .theme-light at the same defaults as dark. (3) Inline title
         font: was hardcoded to "SF Pro Display" regardless of the
         Style Settings heading font knob. Changed to
         var(--cn-heading-font) so New York and Georgia selections now
         apply to the inline title as well as the headings.)
v1.10.13 (post-v1.10.12 audit follow-ups. (1) Default accent and
         inline code now clear WCAG AA on every text-bearing surface
         in every default Background tone variant, not just Zen.
         Earlier audits had only checked canvas, sidebar, and code
         surfaces; bg-elevated (modals, popovers, dropdowns) was
         missed. Fixed: dark Charcoal bg-elevated #2a2a2a -> #282828
         (accent ratio 4.44 -> 4.56). Dark Warm bg-elevated
         #2f2c2a -> #2b2826 (accent 4.29 -> 4.53, inline 4.44 ->
         4.69). Light Charcoal bg-elevated #e3e2e0 -> #e8e7e5
         (accent 4.45 -> 4.67). Light Warm bg-tertiary #e6e0d4 ->
         #ece6d9 (accent 4.39 -> 4.64) and bg-elevated #ddd6c8 ->
         #e9e3d5 (accent 3.99 -> 4.51). Pure Black and Zen variants
         already passed at v1.10.12. Lifted-tray values for non-Zen
         variants are decorative chrome (no text lands on them) and
         are unchanged. (2) Kanban added to the translucent leaf-
         content force-paint opt-out chain in all three locations
         (sections 4 dark, 4 light, and the .mod-root chain).
         obsidian-community/obsidian-kanban registers a full Preact
         leaf with data-type "kanban"; without the opt-out, the
         theme would paint over its internal styling the same way
         it would for Excalidraw or Canvas. (3) Bubble nav comment
         tightened to be honest about which transitions are
         compositor-friendly: transform and opacity are; the
         background-color and border-color hover changes still
         paint, but the size reveal does not reflow. (4) Modal-
         backdrop !important cluster (.modal-bg) now carries an
         inline rationale comment matching the documentation
         pattern used for other !important uses. (5) Empty
         authorUrl field removed from manifest.json; the field is
         optional in Obsidian's manifest spec and an empty string
         is metadata noise.)
v1.10.12 (deferred visual changes from the v1.10.11 review pass.
         (1) Zen Background tone reworked for WCAG AA. Approach
         picked: adjust surface luminance, keep the shared default
         accent and inline code. The Zen identity now carries via
         warm hue (red/orange undertone) instead of mid-gray
         lightness, so every Zen surface clears AA against the
         default accent (#5b8def dark / #3a5fc7 light), default
         inline code (#ee6666 dark / #a8201a light), and primary
         text (#e8e8e8 dark / #37352f light). Dark Zen surfaces
         moved from #5a5755 / #4f4c4a / #656260 / #6e6b68 to
         #1f1c19 / #181613 / #25221f / #2a2724. Light Zen surfaces
         moved from #ebe9e5 / #e0ddd8 / #d6d3ce / #cbc7c1 to
         #f5f3ef / #eeece8 / #ece9e4 / #e9e6e1. Sidebar-lifted
         and lifted-tray overrides retuned for the new scale; the
         old Zen-specific border override (rgba 0.08 / 0.14) is
         dropped because the darker dark-Zen surfaces no longer
         need the bump above the theme-dark default (rgba 0.06 /
         0.10). Verified contrast ratios in order bg-primary /
         bg-secondary / bg-tertiary / bg-elevated:
           Dark Zen primary: 13.84, 14.74, 12.91, 12.12.
           Dark Zen accent:   5.25,  5.59,  4.90,  4.60.
           Dark Zen inline:   5.43,  5.78,  5.07,  4.76.
           Light Zen primary:11.06, 10.39, 10.13,  9.85.
           Light Zen accent:  5.20,  4.89,  4.76,  4.63.
           Light Zen inline:  6.56,  6.17,  6.01,  5.84.
         All ratios clear the 4.5:1 body-text threshold. Charcoal,
         Pure Black, and Warm tone variants are unchanged in
         appearance and contrast. (2) Bubble nav animation moved
         off layout-triggering max-height / max-width to
         compositor-friendly transform: scale and opacity. The
         visual idiom is refined: buttons stay visible at rest,
         scaled to 65 percent with reduced opacity, and spring to
         full size on hover or focus-within. No layout reflow per
         frame. Children no longer toggle opacity 0 to 1; the
         scale-in carries the reveal. The universal section-22
         prefers-reduced-motion block still neutralizes the new
         transitions.)
v1.10.11 (review-driven fixes from outside-eye audit before the
         obsidian-releases submission was reviewed. (1) Excalidraw
         opt-out narrowed: replaced the previous all: unset block on
         .excalidraw form controls with targeted color/font overrides
         and a checkbox appearance restore. all: unset stripped
         inheritance the plugin relies on for layout, accessibility
         affordances, and its own custom controls; the new block
         keeps the theme out of Excalidraw's way without nuking
         anything. (2) Core Canvas added to the translucent leaf-
         content opt-out chain (data-type="canvas") in all three
         opt-out selectors. Defensive: keeps Cognia Noir from force-
         painting or force-transparency on Canvas leaves where
         Canvas's own grid, cards, and edges should win. (3) Smart
         Connections reference comments corrected: the data-type
         "smart-connections-view" is the related-notes view, not a
         graph canvas. The file-header comment and the section-19
         head comment for the smart-connections rule now agree
         about what that view actually is.)
v1.10.10 (resize + transition fixes. (1) Removed the
         .workspace-split.mod-left-split > hr /
         hr.workspace-leaf-resize-handle display:none rule that
         evolved across v1.9.5-v1.9.8. In current Obsidian the
         sidebar-width drag handle IS an hr.workspace-leaf-resize-
         handle that is a direct child of .mod-left-split, so the
         rule killed the resize cursor and drag interaction. The
         original target (decorative hr between file-explorer
         column and vault profile) does not appear in current DOM;
         if it returns, scope by a more specific selector that
         excludes the boundary handle. (2) Snappier hover/focus
         feedback via new --cn-transition-fast (100ms ease-out)
         replacing var(--cn-transition) (150ms symmetric ease-in-
         out) which read as laggy. Applied to nav titles, tab
         headers, links, modal/dropdown inputs, hover-state buttons
         and clickable-icons in tab-header / view-header / nav-
         buttons / status-bar, and tag pills. Deliberately NOT
         applied to checkboxes (state-change, not hover), scrollbar
         thumbs (fade behavior), bubble-nav layout reflow, or
         clickable-icon press transforms. Those benefit from the
         slower symmetric easing.)
v1.10.9 (housekeeping. (1) --inline-title-size, --inline-title-weight,
         --inline-title-line-height moved from both themed blocks to
         :root (only --inline-title-color stays themed, references
         --h1-color). (2) Changelog reordered to strict descending
         version order (1.10.6/5/4 had been listed after 1.10.3,
         making the history hard to follow). (3) Section 19 header
         expanded with a 19a/19b subsection index listing all 14
         subsections in the 500-line quality-of-life block.)
v1.10.8 (follow-up hardening. (1) Shared constants block (section 0):
         --cn-radius, --cn-radius-lg, --cn-transition, --radius-s/m/l
         moved from .theme-dark and .theme-light into :root so each
         is defined once. (2) Section table of contents added to the
         file header for navigation in a 2600-line file. (3) Bubble
         nav transition: removed padding from the animated list (was
         a third layout-triggering property per frame; now only
         max-height and max-width animate). Padding unified to 3px 6px
         in both collapsed and expanded states so there is no jump.)
v1.10.7 (perf audit: added @media (prefers-reduced-motion: reduce)
         block that disables all transitions for users with macOS
         Reduce Motion enabled (accessibility + perf gap). Removed
         redundant border-radius: 10px from .workspace-tab-header
         .is-active rule (already set on base rule; cascade no-op).
         Removed redundant border-radius: 14px / empty .is-active
         rule from .mod-sidedock .workspace-tab-header. Added
         comment on bubble-nav transition noting that max-height,
         max-width, and padding trigger layout reflow on hover.)
v1.10.6 (three new Style Settings knobs. (1) Heading font:
         class-select matching note font options (SF Pro
         Display default, New York, Georgia). Heading font-
         family rule now uses --cn-heading-font variable
         instead of hardcoded SF Pro Display. (2) Note line
         height: variable-number-slider 1.4-2.0 step 0.05,
         default 1.65. Replaces the hardcoded value in the
         markdown view block. (3) Line width: variable-number-
         slider 500-1400px step 50, default 700. Sets
         --file-line-width which Obsidian picks up natively.)
v1.10.5 (note font Style Settings knob. New cn-note-font class-
         select offers SF Pro (default), New York (Apple serif),
         Georgia (classic serif), and Avenir Next. Sets
         --font-text-theme on body; headings keep SF Pro Display
         from section 3 regardless of selection.)
v1.10.4 (extended non-CTA settings button fix from v1.10.3 to
         cover light mode on iPhone. Selector changed from
         body.is-phone.theme-dark to body.is-phone so both
         themes get the tertiary-fill + border treatment.)
v1.10.3 (three regression fixes from user spot-check.
         (1) View-header right-side action icons (+ new note,
         book/reading-mode toggle, ... overflow) had drifted
         flush left on desktop. v1.10.2 hid the redundant
         centered title via display:none on .view-header-
         title-container - but that container carries flex:1
         in Obsidian's stock layout and was the spacer pushing
         .view-actions to the right. Now the inner .view-
         header-title gets visibility:hidden instead, so the
         container stays as a flex spacer and the actions
         stick to the right. Belt-and-suspenders rule pins
         .view-actions with margin-left:auto.
         (2) Mobile + new-note action icon was not visible on
         iPhone - only the ... overflow rendered. Cause: the
         section 19b -electron-corner-smoothing chain and pill-
         shaped clickable-icon rule were missing the :not(.is-
         phone) guard that every other section 19b rule
         carries (see the "Mobile scope decision (locked v1.9.9)"
         block comment in section 19b). 32px min-
         height + 10px horizontal padding in the tight mobile
         view-header was clipping the + icon. Both rules now
         scoped to body:not(.is-phone). iPad keeps the desktop
         chrome.
         (3) Settings cards on iPhone in dark mode had nearly
         zero contrast against the page (Account, Catalyst
         license, Help, etc. blended into the background) and
         Open / Manage / Log out / Upgrade / Purchase buttons
         rendered as bare text. Added a phone+dark-only
         section 20 that paints settings card surfaces with
         --cn-bg-elevated + a subtle border, and gives non-CTA
         buttons a --cn-bg-tertiary fill so they read as
         buttons. Light mode and iPad are unchanged.)
v1.10.2 (two visual cleanups surfaced by user spot-check on
         v1.10.1.
         (1) Active tab pill showed two small detached
         circles at its bottom-left/bottom-right. Cause:
         Obsidian paints decorative ::before/::after pseudo-
         elements on .workspace-tab-header to draw the
         curved "tab ear" connectors that join an active
         tab to the bar below it. With section 19b's
         bordered self-contained pill style those curves
         no longer connect to anything and render as
         floating dots. Hidden via display:none on both
         pseudos, scoped to the same selector chain as the
         pill rules so stock styling stays intact wherever
         pills don't apply.
         (2) Note title appeared three times: tab label,
         small centered .view-header-title text, and the
         large bold .inline-title at the top of the note
         body. The middle one is fully redundant when the
         other two are visible. Hidden via display:none on
         .view-header-title-container, scoped to .mod-root
         so sidebar leaf headers (search, outline, etc.)
         keep their titles. The view-header itself stays in
         the layout - only the title text is suppressed -
         so right-side action icons keep their container.)
v1.10.1 (regression fix on v1.10.0 panel-box selector.
         v1.10.0's broadened sidebar panel-box rule reused
         section 4's translucent-force-paint exclusion list
         verbatim - terminal, excalidraw, smart-connections-
         view. That mechanical reuse was wrong: the two
         exclusion lists serve different purposes. Section 4
         opts out views whose internal styling collides with
         our bg-primary force-paint over macOS vibrancy.
         Section 19b's panel box only needs to opt out views
         whose content fights a margin-bordered container
         (full-canvas painters like terminal and excalidraw).
         Smart Connections' related-notes side panel
         (data-type=smart-connections-view) is a normal
         scrollable list and SHOULD get the box; v1.10.0
         silently suppressed it. Dropped smart-connections-
         view from the section 19b exclusion list and
         rewrote the panel-box rule's head comment to spell
         out the per-list rationale so the next audit
         doesn't repeat the mistake. Verified by Cowork V7
         run that flagged the data-type identity question;
         confirmed against local Smart Connections plugin
         source that smart-connections-view is the related-
         notes panel, not a graph/canvas view.)
v1.10.0 (hardening pass on top of v1.9.9 audit. (1) Ribbon
         top-margin: split into a 36px default that aligns
         with the folder-box top in non-frameless and non-
         macOS layouts, plus the existing 60px override for
         macOS frameless (traffic-light clearance). (2)
         Section 19b folder-box selector broadened: instead
         of enumerating data-types (file-explorer, bookmarks,
         outline, etc.), now matches every sidebar leaf via
         :is(.mod-left-split, .mod-right-split) ... .view-
         content with the same plugin-safe-zone exclusion
         list section 4 uses (terminal, excalidraw, smart-
         connections-view). File-explorer keeps its dedicated
         .nav-files-container selector. Smart Connections
         nav, Dataview, Periodic Notes, Outliner, etc. now
         pick up panel-box chrome automatically. (3) iPad
         scope decision (:not(.is-phone)) documented in a
         block comment above the top-bar tab-pill rule so
         the choice isn't relitigated. (4) Vault-profile
         pseudo-element parent-width dependency documented
         in a defensive comment, and the horizontal inset
         (8px) now derives from a shared --cn-panel-side-
         inset CSS var that the folder box also uses, so
         the two stay edge-locked. (5) !important audit:
         three previously undocumented uses (sync-status-
         icon, hide-metadata, vault-profile border-top) now
         carry one-line "needed because X" comments; print-
         block !important cluster gets a block-level rationale.
         Ribbon align-self/height workaround re-confirmed
         load-bearing per v1.8.6 with a clearer comment.
         Visual regression sweep for light mode, dark
         translucent, multi-pane right sidebar, and print
         preview is the user's responsibility - not testable
         from the editor side.
         CORRECTION (see v1.10.1 entry above): item (2)'s
         exclusion list mechanically copied section 4's
         translucent-force-paint opt-out and incorrectly
         suppressed the panel box on Smart Connections'
         related-notes side panel. Fixed in v1.10.1.)
v1.9.9  (desktop+mobile audit fixes: cn-sidebar-lifted
         now overrides --cn-bg-note-tray (was a no-op in
         opaque mode because section 4b paints tray
         directly, bypassing --background-secondary);
         light-mode --text-selection switched from
         hardcoded blue to color-mix on --cn-accent so it
         follows Style Settings; HR rule in Live Preview
         now centers via translateY(-50%); section 4b
         tray paint, note clip, and sidebar rounding
         scoped to body:not(.is-mobile) so mobile drawers
         are untouched; section 19b chrome (sidebar tab
         pills, view-action FAB, ribbon box, panel boxes,
         vault-profile pseudo) switched from :not(.is-
         mobile) to :not(.is-phone) so iPad gets the full
         desktop look matching the top-bar tabs; click-
         feedback scale on .clickable-icon scoped to
         :not(.is-mobile) so it no longer stacks on top
         of iOS/Android tap-highlight.)
v1.9.8  (audit pass: scoped resize-handle hide to mod-
         left-split only - the v1.9.7 universal selector
         would have hidden inter-pane resize handles in
         the right sidebar too, breaking that feature
         silently for anyone with multiple panes stacked
         on the right.)
v1.9.7  (the hr.workspace-leaf-resize-handle was visible
         even with opacity:0 + transparent border/bg
         (likely a child pseudo-element or stronger
         inline override). Switched to display:none which
         removes it from render entirely. Column resize
         still works because that's a different element
         (.workspace-leaf-resize-divider). Also zeroed any
         top border on the vault profile defensively.)
v1.9.6  (workspace-leaf-resize-handle was still showing
         as a line because Obsidian sets opacity:1 inline
         on the element. Override with !important on the
         hr's border, background, and opacity so the line
         disappears regardless of inline styling.)
v1.9.5  (hide the workspace-leaf-resize-handle <hr> that
         sits between the file explorer column and the
         vault profile. With the new bordered boxes around
         both, the hr's default border-top showed up as an
         awkward horizontal divider. Now transparent - the
         element stays in place so resize drag still works,
         but no visible line.)
v1.9.4  (vault box rebuilt around DevTools data: the
         workspace-sidedock-vault-profile is a flex child
         that's force-stretched to the full sidebar width
         (234.5px in the user's window) regardless of
         margins, which is why every margin attempt drifted
         unpredictably. Now the element keeps its natural
         full-width position and a ::after pseudo-element
         with absolute positioning (top:0 bottom:0 left:8px
         right:8px) draws the visible box. 8px matches the
         folder box's nav-files-container horizontal margin,
         and both elements live in 234.5px-wide parents, so
         the boxes end up at identical x-coordinates.)
v1.9.3  (vault box: shifted left edge right by +15px
         (margin-left -3 -> 12) and right edge right by
         +11px (margin-right -14 -> -25) to bring both
         edges in line with the folder box.)
v1.9.2  (vault box left margin nudged from -18 to -3
         after the right edge aligned but the left edge
         extended ~15px past the folder box's left.)
v1.9.1  (vault box uses negative margins on BOTH sides
         (-18px left, -14px right) - its parent container
         is narrower than the folder-list column, so to
         reach the folder box's left/right x-coordinates
         the box has to extend past its parent's bounds.
         Acceptable since the gaps on either side are
         empty space inside the left split.)
v1.9.0  (vault box uses negative left margin (-18px) to
         counter the ~28px extra left inset coming from the
         vault-profile's parent or internal layout, plus
         38px right margin to bring the right edge in to
         match the folder box. Net effect: box left/right
         edges now align with the folder box.)
v1.8.9  (vault box reverted to margin-based sizing - the
         width: calc approach truncated "Master Vault" to
         "Master V..." because Obsidian's internal layout
         on the vault-profile constrained the inner content
         width below the box width. Now uses margin: 4px
         28px 8px 12px (asymmetric to counter the slight
         leftward content shift), which matched closely in
         v1.8.7 testing.)
v1.8.8  (vault box now uses centered-width sizing
         (margin-inline: auto + width: calc(100% - 36px))
         to match the folder box's horizontal extent. More
         reliable than asymmetric margins because the
         vault-profile's internal layout shifts content
         unpredictably under fixed margins.)
v1.8.7  (mobile audit + box tuning. All three left-column
         box rules (ribbon, sidebar leaves, vault profile)
         now scoped with body:not(.is-mobile) so mobile/
         tablet drawer layouts are untouched. macOS
         frameless ribbon top reduced from 70px to 60px to
         better match folder-box top. Vault box margins
         switched to asymmetric 8px-left / 32px-right to
         counter the vault-profile element's internal
         rightward content offset, so the box now visually
         lines up with the folder box's horizontal extent.)
v1.8.6  (ribbon and vault box tuning. Ribbon: stronger
         border (--cn-border-strong instead of -subtle) so
         it reads against the dark column where there's no
         adjacent content for contrast; align-self stretch
         and height auto force the container to fill the
         workspace height so the bottom edge isn't clipped
         to the icon list; macOS top margin reduced from
         88px to 70px to match folder-box top. Vault box:
         asymmetric horizontal margin (28px left, 40px right)
         so the box width visually matches the folder box,
         since the vault-profile has different parent
         padding than .view-content.)
v1.8.5  (ribbon-box vertical alignment: top now starts at
         88px (matches folder-box top - below the sidebar
         tab-pills + nav-header on macOS frameless). Bottom
         margin reduced to 8px so the ribbon box ends at
         the same y as the vault-profile box. Vault-profile
         horizontal margin bumped to 24px so its width
         visually matches the folder box.)
v1.8.4  (ribbon-box layout polish: bumped macOS frameless
         top margin to 44px for cleaner clearance from the
         traffic lights; added 52px bottom margin so the
         ribbon box ends where the vault-profile box begins
         (left column reads as ribbon-top + vault-bottom
         with a gap between, mirroring the folder side);
         widened vault profile horizontal margin to 16px
         so its box visually matches the folder-box width.)
v1.8.3  (fixed traffic-light overlap on macOS frameless -
         ribbon box now starts below the traffic lights via
         scoped margin-top. Added matching panel box around
         the bottom-left vault profile (vault switcher,
         help, settings gear) so the left column reads as
         three stacked panels: ribbon / folder list / vault
         profile.)
v1.8.2  (ribbon now wears its own bordered rounded box so
         it reads as a sibling panel to the folder box.
         Dropped the right-border separator since the box
         border provides separation. Both columns share
         12px radius, 1px subtle border, and matching
         margins.)
v1.8.1  (added ribbon-to-sidebar separator + bordered box
         around left/right sidebar leaf content (file
         explorer, bookmarks, recent files, outline,
         backlinks, etc.) - folder tree now reads as a
         distinct panel inside the sidebar.)
v1.8.0  (Velocity-inspired chrome shapes - section 19b. Top-bar
         tabs become bordered pill rectangles with min-height 32px
         and radius 10px. Clickable icons in workspace-ribbon, view-
         header, sidebar toggle, vault profile, and tab-list become
         pill-shaped (radius-xl, padding 6px 10px). View-header
         action icons (the right-side + and reading-glasses) become
         circular FAB-style buttons with subtle border. Status bar
         rounded on the left edge. Modal/menu/popover radii bumped
         (modal 18px, menu 14px). Electron corner smoothing applied
         to all rounded chrome surfaces. Colors and translucency
         behaviors are unchanged - shapes only.)

