# Firefox + Sidebery Setup

My Firefox vertical-tabs setup: Sidebery as a collapsed icon rail that expands on hover, with sub-tab trees flattened while collapsed and colors pulled from the active Firefox theme.

## What's in this repo

```
firefox-setup/
├── README.md               # this file
├── sidebery-styles.css     # paste into Sidebery > Settings > Styles editor > Sidebar
├── sidebery-export.json    # exported Sidebery settings (re-export after big changes)
└── chrome/
    ├── userChrome.css      # rail width + hover-expand behavior (Firefox side)
    └── userContent.css     # optional
```

## How it fits together

- **Firefox side (`chrome/userChrome.css`)** controls the sidebar's width: the narrow rail and the expand-on-hover behavior. Sidebery doesn't control this.
- **Sidebery side (`sidebery-styles.css`)** controls what's drawn inside the sidebar: background gradient, transparent panels, flattened sub-tab trees, active tab highlight.
- **Theme colors** come from Sidebery's theme adaptation. It exposes the Firefox theme as `--s-*` CSS variables (`--s-frame-bg`, `--s-frame-fg`, `--s-toolbar-bg`, etc.), and the CSS builds everything from those, so it follows whatever theme is active.

## Restoring on a new machine or profile

1. **Install extensions and theme**
   - Install [Sidebery](https://addons.mozilla.org/en-US/firefox/addon/sidebery/).
   - Theme in use: **Pixel-Punk - Dynamic Theme** (any theme should work; see Notes).
   - If Firefox Sync is on, add-ons and the theme may carry over on their own.

2. **Enable userChrome support**
   - Go to `about:config` and set `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`.

3. **Copy the Firefox-side files**
   - Go to `about:support`, click **Open Folder** next to *Profile Folder*.
   - Copy this repo's `chrome/` folder into it.
   - Restart Firefox.

4. **Import Sidebery settings**
   - Open Sidebery's settings and import `sidebery-export.json`
     (look for Export/Import or Backup; the label varies by version).
   - Optionally enable Sidebery's settings sync through your Firefox account.

5. **Apply the styles**
   - In Sidebery, set the theme option to the adaptive one (follows the Firefox theme).
   - Open **Settings > Styles editor > Sidebar** and paste in `sidebery-styles.css`.

6. **Check it**
   - Collapsed: icons only, no tree lines or indent.
   - Hover: sidebar expands and the tree comes back.

## Maintenance

- After changing Sidebery settings or CSS, re-export `sidebery-export.json` and commit it. It's a snapshot and doesn't update on its own.
- If you edit CSS in Sidebery's Styles editor, copy it back into `sidebery-styles.css`.

## Notes and gotchas

- The tree flattening relies on Sidebery's `--tabs-indent` and `--tabs-lvl-opacity` variables, applied when the cursor isn't over the sidebar (`#root:not(:hover)`).
- The active tab highlight uses `.Tab[data-active] .lvl-wrapper:after`. If a Sidebery update changes that markup, inspect an active tab and update the selector.
- The gradient has a faint SVG noise layer on top. It's there to dither out color banding. If it looks grainy, lower the `0.05` alpha in the `feColorMatrix`.
- Sidebery only gets a theme's solid colors, not its images. With an artwork-based theme (like Pixel-Punk), the sidebar matches the palette, not the banner.
- To inspect Sidebery's DOM: `about:debugging` > This Firefox > Sidebery > Inspect.
