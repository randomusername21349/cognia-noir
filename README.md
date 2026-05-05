# Cognia Noir

A Notion-inspired Obsidian theme. Minimal, calm, dark and light modes. SF Pro typography. True macOS vibrancy when Obsidian's Translucent Window is enabled.

![Cognia Noir](screenshot.png)

## Features

- Dark and light modes, both designed to feel like Notion's surface palette.
- SF Pro for UI; SF Pro Text for note bodies; SF Mono for code.
- Rounded note panel and sidebars with a contrasting tray tone underneath.
- Native macOS vibrancy: enable Settings > Appearance > Translucent Window. The theme paints sidebars and chrome over `NSVisualEffectView` so the desktop wallpaper shows through.
- Plugin-safe vibrancy: Excalidraw and Smart Connections canvas leaves are opted out of the translucent leaf-content force-paint so they render correctly.
- WCAG AA contrast on accent, inline code, secondary text, and faint text in both modes.
- Honors `prefers-reduced-motion: reduce` (all transitions and animations disabled).
- Snappier hover and focus feedback (`100ms ease-out`) on nav titles, tab headers, links, inputs, buttons, and tags. State-change and layout transitions stay on a slower symmetric curve.

## Install

### From the Obsidian community store

1. Open Obsidian.
2. Settings > Appearance > Manage > Browse.
3. Search for "Cognia Noir".
4. Click Install, then Use.

### Manually

1. Download `manifest.json` and `theme.css` from the latest release.
2. Place them in `<your-vault>/.obsidian/themes/Cognia Noir/`.
3. In Obsidian: Settings > Appearance > select "Cognia Noir" under Theme.

## Compatibility

- Minimum Obsidian version: 1.5.0.
- Designed and tested on macOS. Windows and Linux render correctly minus the macOS-specific vibrancy effect.
- Mobile (iPad and phone): supported. iPad gets the full desktop chrome; phone scales appropriately.

## Gallery

### Overview

Lorem-ipsum demo note rendering in both modes: title, body copy, inline formatting (bold, italic, strikethrough, inline code, links, highlight), ordered and unordered lists.

| Dark | Light |
| --- | --- |
| ![Overview, dark](screenshots/01-overview-dark.png) | ![Overview, light](screenshots/01-overview-light.png) |

### Tasks, blockquote, callouts

Task list, blockquote, and the four built-in callout types (Note, Tip, Warning, Quote) with their accent colors.

| Dark | Light |
| --- | --- |
| ![Callouts, dark](screenshots/02-callouts-dark.png) | ![Callouts, light](screenshots/02-callouts-light.png) |

### Code blocks and tables

Fenced code blocks (Python and shell, language pill in the corner) and a Markdown table.

| Dark | Light |
| --- | --- |
| ![Code and tables, dark](screenshots/03-code-table-dark.png) | ![Code and tables, light](screenshots/03-code-table-light.png) |

### Internal links, tags, math

Internal links with hover underline, tag pills with rounded background, inline LaTeX, and a centered display equation.

| Dark | Light |
| --- | --- |
| ![Math and tags, dark](screenshots/04-math-dark.png) | ![Math and tags, light](screenshots/04-math-light.png) |

## Style Settings

If you have the [Style Settings](https://github.com/mgmeyers/obsidian-style-settings) plugin installed, Cognia Noir exposes the following knobs.

### Look and feel

- **Accent color** — Used for links, focus rings, and selection. Defaults clear WCAG AA against each theme's canvas. Separate light and dark defaults.
- **Inline code color** — Color of `inline code` text. Defaults clear WCAG AA against each theme's code background. Separate light and dark defaults.
- **Background tone** — Charcoal (default), Pure Black (OLED-friendly), Warm charcoal (slight warmth), or Zen.
- **Sidebar contrast** — How the sidebars separate from the main editor. Flat (default), Subtle border, or Lifted (slightly darker).
- **Translucency tint** — Strength of the dark wash layered over macOS vibrancy. Pure vibrancy, Light, Medium (default), or Heavy. Pure shows raw vibrancy; Heavy dims it for legibility on bright wallpapers. Requires Settings > Appearance > Translucent Window enabled.
- **Heading scale** — Compact, Comfortable (default), or Large.

![Style Settings: look and feel](screenshots/05-style-settings-look-and-feel.png)

### Typography

- **Editor font size** — Slider, 12 to 20 px (default 16).
- **UI font** — Font for UI chrome (sidebars, tabs, buttons, dialogs). Inter (default), Avenir Next, or System (SF Pro).
- **Note font** — Font for note content (editor and reading view). SF Pro (default), New York (Apple serif), Georgia (classic serif), or Avenir Next.
- **Heading font** — Font for H1-H6. SF Pro Display (default), New York, or Georgia. Pair with the note font for a matched look.
- **Note line height** — Slider, 1.4 to 2.0 (default 1.65).

![Style Settings: typography](screenshots/05-style-settings-fonts.png)

### Layout

- **Line width** — Maximum width of the note content area. Slider, 500 to 1400 px (default 700, matching Obsidian's stock default).
- **Bubble nav buttons** — Collapses the file explorer / bookmarks action row (new note, new folder, sort, expand) into a small pill that expands on hover. Inspired by Velocity.

![Style Settings: layout](screenshots/05-style-settings-layout.png)

## Changelog

See the version history at the top of `theme.css` for a full per-version log. Highlights:

- **1.10.10** — Removed a `display:none` rule that was hiding the sidebar resize handle. Added `--cn-transition-fast` (100 ms ease-out) for hover and focus feedback on nav titles, tab headers, links, inputs, buttons, and tags.
- **1.10.7** — Added `prefers-reduced-motion: reduce` block.
- **1.10.6** — Three Style Settings knobs (heading font, note font, color variants).
- **1.7.x** — Initial Notion-inspired surface palette, rounded note and sidebar corners, macOS vibrancy support.

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built on conventions from the Obsidian theme dev community. Inspired by Notion's surface palette and macOS Sequoia's chrome.
