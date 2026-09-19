# Changelog

## [Version 1.1]

### Added

- **Creation Club plugin support**: 9 Next Gen CC plugins now indexed with RecordType data
  - CC localised strings resolve via new `.strings.lookup.json` files
  - Expandable record lists (click "+ X more" to toggle full content)
- **Localised string resolution**: Plugin Viewer now fetches and resolves localised record names from `Fallout4.strings.lookup.json`, displaying actual in-game names instead of "(localised - unavailable)"
  - Automatically loads `Fallout4.strings.lookup.json` from GitHub on page load
  - Falls back gracefully when running locally (file://) or without network access
  - Shows "(localised - unresolved)" only for strings not found in the lookup
- **String ID extraction**: Records with localised FULL subrecords now capture and store string IDs for resolution
- **Extended record type labels**: Added descriptive labels for 15+ previously unlabeled record types:
  - IMGS (Image Space Modifiers), SNCT (Sound Concurrency), GDRY (Godrays), CAMS (Camera Shot)
  - LAND (Landscape), TACT (Talking Activator), RFCT (Reference Effect), BPTD (Body Part Data)
  - CLAS (Class), FSTS (Footstep Set), MATO (Material Object), AECH (Audio Effect Chain)
  - STAG (Sound Tag Set), SMEN (Story Manager Event Node), ZOOM (Zoom Data), ASTP (Association Type)
  - BNDS (Bend Spring Data), SCSN (Cinematic Scene), DMGT (Damage Type), AORU (Audio Occlusion Rule)
  - CLMT (Climate), INNR (Instance Naming Rules), DEBR (Debris), SPGD (Shader Particle Geometry Data)
  - PMIS (Projectile Missile), DOBJ (Default Object), DLVW (Dialog View), NOCM (Navigation Object Culling Manager)
  - OVIS (Object Visibility Cutoff)

### Fixed

- **GDRY label**: Corrected from "Goodness" to "Godrays"
- **IMGS label**: Updated from "Image Space" to "Image Space Modifiers"
- **Fallout4.esm.strings.lookup.json**: Generated from extracted .STRINGS files using new `xml_strings_to_json.py` converter

### Changed

- `enrichNamesFromMasterIndices()` now loads CC strings lookups alongside master indices
- `extractNames()` now captures `stringId` for localised records instead of boolean `localisedUnavailable` flag
- `displayNameFor()` now attempts string resolution before falling back to EditorID or "(unnamed)"
- `resolveBaseObjects()` now resolves REFR/ACHR base object names using string lookup when available
- Record comparison logic updated to check for `stringId` presence instead of `localisedUnavailable` flag

### Breaking Changes

- **Local webserver now required**: Plugin Viewer no longer works with `file://` protocol due to cross-origin fetch for assets
  - Use: `cd plugin-viewer && python3 -m http.server 8000` then open `http://localhost:8000`

### Technical

- Added `loadStringsLookup()` async function to fetch and cache string resolution table
- Updated `processRecord()` to store `stringId` alongside existing fields
- String lookup is stored in global `stringsLookup` object; graceful no-op if fetch fails
- All changes backward-compatible with multi-file comparison mode

---

## Tools & Resources

### New

- **`xml_strings_to_json.py`**: Converter script to extract localised strings from Bethesda String Extractor XML output
  - Usage: `python xml_strings_to_json.py input.xml output.json`
  - Converts hex string IDs to decimal for easier lookup

- **`fo4_strings_extractor.py`**: Standalone Python tool for extracting localised strings from Fallout 4 plugins
  - Supports loose .STRINGS files or BA2 archive extraction
  - Generates JSON compatible with Plugin Viewer format
  - NOTE: ESM record iteration currently incomplete; use XML converter method instead

### Documentation

- **README.md** (new): Comprehensive guide to Plugin Viewer features, usage, and extending the tool
- **CHANGELOG.md** (this file): Version history and technical notes

---

## Future Enhancements

- Complete ESM record parser in `fo4_strings_extractor.py` for automated string extraction
- Expand record type label dictionary as new signatures encountered
- Optional BA2 previs/precombine analysis overlay
- Export filtered record lists as CSV/JSON for batch processing
