---
title: Transfer Settlements Blueprint Toolkit
subtitle: Visualise, organise, edit
---

# Transfer Settlements Blueprint Toolkit

A browser-based toolkit for working with [Transfer Settlements]([https://www.nexusmods.com/fallout4/mods/34032) blueprints. Load, visualise, analyse, and edit your blueprints entirely in your browser. Nothing leaves your computer — all processing happens locally.

**Access it here:** https://viabledahlia.github.io/commonwealth-tools/transfer-settlements-blueprint-toolkit/

## Features

### Editor Tab

- **Load blueprints** via drag-and-drop or file picker
- **View statistics** at a glance: total objects, mods used, marked for removal, remaining count
- **Filter by mod or object name** to focus on what you want to change
- **Group by mod or object type** with expandable/collapsible sections
- **Protect core DLC** with one toggle—exclude official Fallout 4 DLC from removal
- **Exclude mods** from removal to hide their objects whilst protecting dependencies
- **Sort** by object count or name, ascending or descending
- **Bulk actions**: tick/untick all visible items at once
- **Download filtered blueprint** with marked items removed
- **Automatic cleanup**: plugins with no remaining objects are removed from `plugins_required`, and `item_count` is recalculated

### Analyser Tab (Experimental)

- **Spatial visualisation**: plot all settlement objects in 2D space
- **Two view modes**: top-down (X/Y) or elevation (X/Z) with colour-coded height
- **Category breakdown**: see what proportion of your settlement is structures, decorations, power, or placed items
- **Mod dependency analysis**: list all DLC and mods in use with object counts per mod
- **Filter and isolate**: drill down into individual mods to see exactly which objects they contain
- **Interactive exploration**: click objects on the chart or mods in the sidebar to filter and explore

## How to Use

### Editor Workflow

1. **Load a blueprint**: Drag a `.json` file onto the dropzone or click to browse
2. **Review statistics**: Check total objects, mods, and what you're about to remove
3. **Protect what matters**: Tick the "Exclude official DLC" button or click individual mods to lock them
4. **Find what to remove**: Use search to filter, or browse grouped by mod or object name
5. **Mark for removal**: Tick checkboxes next to items you want to delete
6. **Download**: Click "Download filtered blueprint" to save your edited version

### Analyser Workflow

1. **Load the same blueprint**: Use either tab's file picker (data persists across tabs)
2. **Visualise placement**: See a 2D chart of all objects with elevation colour-coded
3. **Understand dependencies**: Check which mods are used and how many objects each one has
4. **Drill into a mod**: Click a mod name to see only that mod's objects
5. **Switch views**: Use the dropdown to toggle between top-down and elevation view

### Switching Between Tabs

Once you've loaded a blueprint, switch between the **EDIT** and **ANALYSE** tabs freely. Your data stays in memory—you won't need to reload the file.

## Known Limitations

- **Analyser is experimental**: Visualisation works but may not be representative of actual in-game placement (especially elevation)
- **No undo in editor**: Marked items stay marked until you clear them manually or reload
- **Object collision not shown**: The visualiser shows point placement only; it doesn't account for object size or collision
- **Limited mod icon/metadata**: The toolkit doesn't fetch additional mod information; it works only with what's in the blueprint JSON
- **Single blueprint at a time**: Load one file per session; starting a new file clears the previous one
- **No backup creation**: Always keep a backup of your original blueprint before downloading filtered versions

## Roadmap

### Near term
- Undo/redo stack for editor
- Comparison view (side-by-side before/after statistics)
- Export statistics as CSV or JSON report
- Search history and saved filters

### Longer term
- Multi-file merge (combine two settlements)
- Object search by form ID or reference ID
- Mod compatibility checker (flag conflicts or missing dependencies)
- Elevation-aware collision detection in visualiser
- Keyboard shortcuts for power users
- Dark mode for analyser (analyser is currently always dark; editor is themed per user preference)

## Troubleshooting

**Q: File won't load**
A: Check that your JSON is valid. Use a JSON validator if unsure. The blueprint export format must include `header` and `items` keys.

**Q: Settlement name shows as "Unknown Settlement"**
A: Your export file may not include a `settlement_name` field in the header. The toolkit will fall back to other location fields if available.

**Q: Plugins still appear after I removed all their objects**
A: Download the blueprint using the green button. The toolkit automatically cleans `plugins_required` when you export.

**Q: Changes didn't save**
A: The toolkit works in-browser only. Click "Download filtered blueprint" to save your changes as a new file. The original is never modified.

## Technical Details

- **No server calls**: Runs entirely client-side using vanilla JavaScript
- **File format**: Works with Transfer Settlements v2.2 JSON exports
- **Browser support**: Modern browsers (Chrome, Firefox, Safari, Edge)
- **File size**: Single ~30KB HTML file; no external dependencies

## Credits

- **[Transfer Settlements](https://www.nexusmods.com/fallout4/mods/22442)** is by [CDante](https://www.nexusmods.com/profile/CDante)
- **[Transfer Settlements Blueprint Editor](https://www.nexusmods.com/fallout4/mods/34032)** by [DMKI](https://www.nexusmods.com/fallout4/users/40407985) was the inspiration and is a full-featured tool - go check it out!
- **[Fliteska's Community Discord](https://discord.gg/tGmQHEy3E)** for the nudge to publish my "failed experiment" 😄

Editor theme inspired by retro terminal aesthetics; analyser theme uses GitHub's dark colour palette for clarity.

## License

MIT. Use freely, modify, distribute. Attribution appreciated but not required.

---

**Feedback or bugs?** Open an issue on GitHub or message Viable Dahlia on Nexus Mods.
