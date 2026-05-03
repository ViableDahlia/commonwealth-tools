# Nexus Tracker Enhanced

A lightweight browser-based tool for searching and exploring your **NexusMods tracked mod list**. No install, no account setup, no data stored anywhere.

Built for Fallout 4 modders who want a fast, readable view of everything they're tracking on NexusMods across all their games, with a quick filter for Fallout 4.

---

## What it does

- **Connects directly to the NexusMods API** using your personal API key
- Fetches your full tracked mod list and displays each mod with its **name, author, version, and game**
- **Search** by mod name or author name with live highlighting
- **Filter** by game using the dropdown, or jump straight to **Fallout 4 only** with one click
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

**Step 5**: Search, filter and sort your tracked mods

---

## Removed mods

Mods that have been taken down or hidden by their author on NexusMods are detected automatically using the `available` field in the NexusMods API response.

- Removed mods are **hidden by default** so they don't clutter the main list
- A **REMOVED** count appears in the stats bar when any are found
- Click **Show Removed** to reveal them, they sort to the bottom of the list and display with a distinct **REMOVED** badge
- Click **Hide Removed** to hide them again
- Each removed mod links to its NexusMods page so you can untrack it and clean up your list

---

## Security

This tool is designed to keep your API key safe:

- 🔒 Served over **HTTPS**
- Your key is sent **directly to `api.nexusmods.com` only** — no third parties, no proxy
- Your key is **not saved** to storage, cookies, or anywhere else
- A Content Security Policy restricts the page to only communicate with NexusMods

---

## Compatibility

- Works with any NexusMods account that has tracked mods
- Displays mods across all games in your tracked list
- Handles all NexusMods game domain formats — camelCase, hyphens, underscores, and plain names
- Includes display names for common Bethesda titles: Fallout 4, Fallout 3, New Vegas, Fallout 76, Skyrim LE, Skyrim SE, Oblivion, Morrowind, and Starfield
- Tested with Fallout 4 Anniversary Edition mod lists

---

## About

Made by **Viable Dahlia**, a Fallout 4 settlement builder focused on architectural approaches to in-game construction.

- [Nexus Mods Profile](https://www.nexusmods.com/profile/viabledahlia)

---

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)

You are free to share and adapt this tool for any purpose, including commercially, as long as you give appropriate credit to **Viable Dahlia**.
