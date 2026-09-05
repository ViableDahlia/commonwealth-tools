# Nexus Tracker Enhanced

A collection of lightweight tools built for Fallout 4 settlement builders and modders. No installs. No accounts. No data sent anywhere except where you choose. Everything runs locally in your browser.

This tool is a lightweight browser-based way to search and explore your **NexusMods tracked mod list**.

---

## What it does

- **Connects directly to the NexusMods API** using your personal API key
- Fetches your full tracked mod list and displays each mod with its **name, author, version, category, and game**
- **Search** by mod title, description, or author, with live highlighting
- **Include or exclude** - every search and filter can either keep matches or hide them
- **Chain multiple conditions together** - lock a search term in as a filter chip and keep adding more; all chips combine with AND for precise, layered searches
- **Filter by game** using the dropdown, or jump straight to **Fallout 4 only** with one click
- **Filter and group by Nexus category** (Fallout 4 only - see [Categories](#categories) below), with collapsible category sections
- **Adult content control** - show everything, hide adult-flagged mods, or show only adult-flagged mods
- **Sort** alphabetically by name or by game
- **Detects removed mods** - mods that have been taken down or hidden on NexusMods are identified automatically, hidden from the main list by default, and shown with a **REMOVED** badge when revealed
- Links directly to each mod's page on NexusMods, with a prompt to untrack removed mods

Your API key is sent directly to NexusMods and nowhere else. It is not saved anywhere and disappears when you close the tab.

---

## How to use it

**Step 1**: Get your personal API key from NexusMods:
> nexusmods.com → Settings → API Keys → Personal API Key

**Step 2**: Open the tool in your browser

**Step 3**: Paste your API key into the input field

**Step 4**: Click **Fetch Mods** and wait for your list to load

**Step 5**: Search, filter, group and sort your tracked mods

---

## Search & Filtering

The search box can match against **Title**, **Description**, and **Author** - toggle any of the three on or off. Clicking one of these buttons searches *only* that field; click it again to go back to searching all three.

Every search has a mode:
- **Include** - keep mods that match
- **Exclude** - hide mods that match

**Chaining filters**: type a term, set its scope and mode, then click **+ Add Filter** (or press Enter) to lock it in as a chip. Keep adding more - every chip combines with AND, so you can build searches like:

> Include "settlement" (title + description) **and** exclude "cheat" (title) **and** include "viabledahlia" (author)

Chips can be removed individually with the ✕ on each one.

**Adult content**: a separate three-way toggle - **All**, **Hide Adult**, or **Adult Only** - sits alongside the quick filters and combines with everything else.

---

## Categories

Fallout 4 mods can be filtered and grouped by their Nexus category (e.g. "Weapons," "Player Settlement," "Overhauls").

- Use the **category dropdown** to filter to a single category
- Click **Group By Category** to reorganise the list into collapsible sections, one per category, sorted alphabetically
- Click a category header to collapse or expand it, or use **Collapse All** / **Expand All**

**Why only Fallout 4?** NexusMods removed the API endpoint that used to turn a category number into a readable name. Category IDs are also per-game, so a lookup table has to be built one game at a time. This tool ships with a hand-maintained Fallout 4 table (sourced from a Mod Organizer 2 `categories.dat` and cross-checked against real API responses). If a mod's category isn't in the table yet, it shows as **"Category N"** instead of guessing - that's a sign the table needs a new entry, not a bug. Tags and language filtering aren't supported at all, since NexusMods only exposes those through a different API that needs a full account login rather than a personal key.

---

## Removed mods

Mods that have been taken down or hidden by their author on NexusMods are detected automatically using the `available` field in the NexusMods API response.

- Removed mods are **hidden by default** so they don't clutter the main list
- A **REMOVED** count appears in the stats bar when any are found
- Click **Show Removed** to reveal them - they sort to the bottom of the list and display with a distinct **REMOVED** badge
- Click **Hide Removed** to hide them again
- Each removed mod links to its NexusMods page so you can untrack it and clean up your list

---

## Security

This tool is designed to keep your API key safe:

- 🔒 Served over **HTTPS**
- Your key is sent **directly to `api.nexusmods.com` only** - no third parties, no proxy
- Your key is **not saved** to storage, cookies, or anywhere else
- A Content Security Policy restricts the page to only communicate with NexusMods

---

## Compatibility

- Works with any NexusMods account that has tracked mods
- Displays mods across all games in your tracked list
- Handles all NexusMods game domain formats - camelCase, hyphens, underscores, and plain names
- Includes display names for common Bethesda titles: Fallout 4, Fallout 3, New Vegas, Fallout 76, Skyrim LE, Skyrim SE, Oblivion, Morrowind, and Starfield
- Tested with Fallout 4 Anniversary Edition mod lists
- Category names, filtering, and grouping are Fallout 4 only for now - other games show a raw category number (see [Categories](#categories))

---

## About

Made by **Viable Dahlia** - a Fallout 4 settlement builder focused on architectural approaches to in-game construction.

- [Nexus Mods Profile](https://www.nexusmods.com/profile/viabledahlia)

---

## License

[MIT License](LICENSE)

You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of this software. Attribution to **Viable Dahlia** is appreciated but not required.

---

See [CHANGELOG.md](CHANGELOG.md) for version history.
