# Plugin Viewer

A browser-based inspector for Fallout 4 ESP/ESM/ESL files. Upload one or two plugins to see what they do, what they break, and whether they share conflicts.

**Live version:** https://viabledahlia.github.io/commonwealth-tools/plugin-viewer/

## What it does

Plugin Viewer reads your plugins and shows you:

- **What's new**: Records the plugin creates (STAT objects, quests, NPCs, etc.)
- **What's overridden**: Records it changes (items, settings, locations, etc.)
- **Conflicts with other mods**: Load two plugins to see overlapping edits, side-by-side
- **Record composition**: Breakdown by type (how many REFR vs STAT vs QUST etc.)
- **Localised names**: Where available, shows actual in-game display names for localised records
- **Placement context**: For REFR/ACHR records, shows what objects are placed and how many of each

## How to use

### Single plugin

1. Open [Plugin Viewer](https://viabledahlia.github.io/commonwealth-tools/plugin-viewer/)
2. Drag a .esp/.esm/.esl file onto the drop zone, or click to browse
3. Read the summary under "WHAT THIS MOD DOES"

### Two plugins (conflict comparison)

1. Drag or select **two** files
2. Plugin Viewer compares them side-by-side
3. Overlapping edits appear with both versions listed

### Localised names

If the plugin is flagged as localised (most official DLC content), Plugin Viewer attempts to resolve display names from the built-in string lookup. If a name can't be resolved, it shows "(localised - unresolved)".

To resolve strings locally without a web server, see [Extending Plugin Viewer](#extending-plugin-viewer).

## Features

### Record type labels

Every record signature has a plain-English label. `STAT (Static Object)` instead of just `STAT`. 87+ record types covered.

### Smart grouping

For records with many unnamed/unresolved entries, Plugin Viewer groups them:

```
REFR (Placed Object) · 1244528 records
8972x 0x002CE2, 6884x 0x1ADB1D, ... + 21873 more objects

INFO (Dialogue Response) · 78087 records
PiperDiamondCityHellos, NickDiamondCityHellos, ... + 4661 more named (+ 73416 unnamed/localised)

LAND (Landscape) · 37020 records
(37020 unnamed/localised landscape)
```

### Base object resolution

REFR and ACHR placements show what they place, aggregated by count. "8972x 0x002CE2" is useless; "8972x Sandbag01" is actionable.

### Comparison mode

Upload two plugins to see:

- Which records changed between them
- Which stayed the same
- What new records were added
- How the composition shifted (bytes, counts, by type)

Click any row to see both versions side-by-side.

### Treemap visualization

Click "Record Composition" to see a proportional breakdown by record type. Hover for counts and byte size. Click to sort by count or bytes.

## Interpreting the output

### "New Content Created"

Records the plugin adds that don't exist in the base game or master plugins. These are not conflicts (unless they clash with your other mods).

### "Overrides Existing Content"

Records the plugin changes. If two mods both override the same record, you have a conflict.

### "(unnamed)" entries

Records with no EditorID and no display name. Common for:

- Interior cells (coordinate-based)
- Dialogue responses (thousands of voice lines)
- Navigation meshes (internal data)
- Generated or procedural content

These usually don't need names because builders don't edit them directly.

### "(localised)" entries

Records with display names stored in .STRINGS files (not in the plugin itself). The display name is there in-game, but the browser can't read it directly from the .esp file.

Plugin Viewer attempts to resolve these using a built-in string lookup table. If it succeeds, you'll see the actual name. If not, it shows "(localised - unresolved)".

## Technical notes

### Parsing

Plugin Viewer reads the binary .esp/.esm/.esl format directly in the browser using DataView. It does not run any plugin code; it's purely structural analysis.

- Handles compressed records (zlib decompression via pako library)
- Captures EDID (EditorID), FULL (display name), NAME (base object references), XCLC (cell coordinates)
- Tallies all records by signature for composition analysis
- Classifies records as new vs override based on FormID high byte

### Localization

When a plugin is flagged as localised, display names live in Bethesda's proprietary .STRINGS format inside BA2 archives. Plugin Viewer can't read those archives directly.

Instead, it loads a precomputed JSON lookup table from GitHub (Fallout4.strings.lookup.json) that maps string IDs to text. This lookup is built once and reused.

**Running offline**: If you're using a local file:// URL or don't have network access, the string lookup won't load. Plugin Viewer continues to work normally, but shows "(localised - unresolved)" for those entries.

**Running locally with a web server**: String lookup will load and strings will resolve. Quick server: `python -m http.server 8000`, then open `http://localhost:8000/index.html`.

### Performance

Plugin Viewer is fast even for large plugins (330MB+ Fallout4.esm). Most time is spent on:

1. Decompressing records (pako handles this efficiently)
2. Rendering the record list (limited to first 10 entries per group, with expandable "+X more")
3. Loading the master index files (optional, for base object resolution)

The parsed data lives in memory for the duration of the session. Closing the browser tab clears everything.

## Extending Plugin Viewer

### Adding more record type labels

Open `index.html` and find the `RECORD_LABELS` dictionary (~line 720). Add entries:

```javascript
const RECORD_LABELS = {
  MYXX: 'My Custom Record Type',
  // ... existing entries ...
};
```

Run locally to test, then commit.

### Resolving localised strings without GitHub

If you want to embed the string lookup directly in the HTML (no network fetch needed):

1. Generate `Fallout4.strings.lookup.json` from your extracted .STRINGS files (see Tools below)
2. Open `index.html` and find the fetch call (~line 318)
3. Replace the async fetch with a hardcoded object:

```javascript
// Instead of:
// const response = await fetch(...);
// stringsLookup = await response.json();

// Use:
stringsLookup = {
  "1": "Text of string 1",
  "2": "Text of string 2",
  // ... millions of entries ...
};
```

Trade-off: file size increases from ~100KB to ~2-3MB. Works offline, no web server needed.

### Adding new comparison metrics

The comparison table is built in `renderComparison()` (~line 1050). To add a new column:

1. Find where the row data is built
2. Add your metric calculation
3. Add a table cell in the render function
4. Update the header row

### Debugging

Open the browser console (F12) to see:

- Parse errors (malformed records, unsupported formats)
- String lookup status (loaded count, fetch failures)
- Record tallies (raw counts before grouping)
- Any warnings about unrecognised record types

For detailed parsing logs, add `console.log()` statements in `processRecord()` or `extractNames()`.

## Tools

### xml_strings_to_json.py

Convert Bethesda String Extractor XML output to JSON lookup table.

```bash
python xml_strings_to_json.py Fallout4_en_en.xml Fallout4.strings.lookup.json
```

Input: XML file from [Bethesda String Extractor](https://www.nexusmods.com/fallout4/mods/215)
Output: `{ "1": "String text", "2": "Another string", ... }`

### fo4_strings_extractor.py

Standalone tool to extract localised strings directly from plugins and .STRINGS files.

```bash
python fo4_strings_extractor.py Fallout4.esm --strings-dir "C:\path\to\Strings" --out Fallout4.esm.strings.json
```

Supports loose .STRINGS files or BA2 archive extraction. Note: ESM record iteration incomplete; use XML converter method for now.

## Troubleshooting

### "Not a recognised plugin (missing TES4 signature)"

The file you uploaded isn't a valid .esp/.esm/.esl. Check that:
- File wasn't corrupted during download
- You're uploading a plugin, not a resource file (.bsa, .ba2)
- File extension is correct (.esp, .esm, or .esl)

### Parser returns 0 records

If a plugin that should have content shows 0 records:

1. Check browser console (F12) for error messages
2. Verify the plugin isn't resource-only (meshes, textures, sounds with no records)
3. Try uploading a known good plugin to confirm Plugin Viewer works

### Localised names show "(localised - unresolved)"

String lookup isn't loading. This is normal when:
- Running from file:// (use a local web server instead)
- Network is down
- GitHub is unreachable

Strings will still resolve in-game; they're just not shown in the browser.

### Two plugins don't show a "Conflicts" section

Plugins don't share any overlapping records. Click the treemap to see what they do touch.

## Contributing

Report bugs or suggest features via GitHub issues. Pull requests welcome for:

- New record type labels
- Improved parsing or conflict detection
- Performance optimizations
- Better documentation

When adding labels or fixing bugs, include a test case (plugin file or description) so the change can be verified.

## License

This tool is provided as-is for community use. It's designed for settlement builders, QoL mod authors, and anyone curious about plugin structure.

## See also

- [Commonwealth Tools](https://github.com/ViableDahlia/commonwealth-tools) repository
- [Fallout 4 Modding](https://moddinglinked.com) (The Method, resources, guides)
- [Nexus Mods](https://nexusmods.com/fallout4)
- [xEdit](https://github.com/TES5Edit/TES5Edit) (deeper record inspection)
