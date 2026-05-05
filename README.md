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
- Three Style Settings knobs for color variants, heading font, and note font.

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

## Style Settings

If you have the [Style Settings](https://github.com/mgmeyers/obsidian-style-settings) plugin installed, this theme exposes:

- Color variant (Charcoal, Pure Black, Warm, Zen).
- Heading font.
- Note body font.

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built on conventions from the Obsidian theme dev community. Inspired by Notion's surface palette and macOS Sequoia's chrome.
