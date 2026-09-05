# Changelog

## [1.2.0] - 2026-09-05

### Added
- Search scope toggles - match against Title, Description, or Author individually
- Include/Exclude mode - any search or filter can either keep matches or hide them
- Chainable filter chips - lock in multiple include/exclude conditions that all combine together (AND)
- Adult content control - All / Hide Adult / Adult Only
- Category filter dropdown and Group By Category view (Fallout 4 only)
- Collapse/expand on category groups, plus Collapse All / Expand All
- Footer link back to the Commonwealth Tools homepage

### Changed
- Fetch batch size increased (10 → 20 concurrent) and the artificial delay between batches removed, for faster loading
- The list no longer reshuffles while mods are still loading - it settles into sorted order once the fetch finishes
- README intro description now matches the shared Commonwealth Tools wording

### Fixed
- Search scope buttons (Title/Description/Author) now behave consistently - clicking one always isolates that field, rather than toggling unpredictably depending on what was clicked before it
- Fallout 4 category ID table corrected - the original table (sourced from a community MO2 file, Nexus mod #91100) used a numbering scheme that didn't match what the live API actually returns; replaced with a verified table cross-checked against real tracked mods

### Known limitations
- A handful of Fallout 4 category IDs (49, 54, 60, 64–67, 78, 102+) aren't in the table yet and show as "Category N" until confirmed
- Tags and Language filtering aren't supported - NexusMods only exposes these through a different API requiring full OAuth login rather than a personal API key

## [1.1.0] and earlier

Baseline release, predating this changelog: fetch tracked mods via the NexusMods API, search by mod name or author with live highlighting, filter by game (with a Fallout 4 quick filter), sort by name or game, and automatic detection of removed/hidden mods with a REMOVED badge and untrack link.
