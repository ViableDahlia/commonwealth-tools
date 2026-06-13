# VD Conflict Analyser

A lightweight browser-based tool for visualising and analysing **Fallout 4 plugin conflicts**. No install, no account, no data sent anywhere.

Built for settlement builders and modders who need to understand conflict impact at a glance, without wading through xEdit's interface.

---

## What it does

- **Drag and drop** your xEdit conflict export (TSV) straight into the browser
- See conflicts grouped by **FormID** with full context and override details
- **Identify winning plugins** (loads last, highest priority) vs. losing overrides
- **View load order positions** to understand which plugin takes precedence
- **Search** by FormID to find specific records quickly
- **Filter out harmless conflicts** (`[missing]` entries automatically excluded)
- Displays **statistics** at a glance: total conflicts, affected records, and plugin pairs

Everything runs locally in your browser. Your conflict data never leaves your machine.

---

## How to use it

**Step 1**: Generate conflict data from xEdit

Use [VD Isolate Export Conflicts](https://github.com/ViableDahlia/xedit-scripts) to export your conflicts:
- Select plugins in xEdit
- Right-click → **Apply Script**
- Select `VD_Isolate_Export_Conflicts_v2.pas`
- Output saved to: `Edit Scripts\Isolated_Conflicts_Only.txt`

**Step 2**: Open the Conflict Analyser in your browser

**Step 3**: Drag `Isolated_Conflicts_Only.txt` onto the page, or click **Browse File**

**Step 4**: Explore your conflicts

- **Search** by FormID to find specific records
- **Expand** each FormID to see all conflicting plugins
- **Note** which plugin is WINNING (loads last) and which are LOSING (overridden)
- Use **load order position** to understand priority

---

## Understanding the Dashboard

- **WINNING badge** = plugin loads last (highest priority, wins conflicts)
- **LOSING badge** = plugin overridden by later-loading plugin
- **Load order position** = `[#N]` where N is the position index in your load order
- **Real conflicts** = actual value differences between plugins
- **[missing] entries** = fields not carried forward by an override (filtered out)

---

## Integration with The Method

This tool works with [The Method](https://moddinglinked.com/The%20Method.html) conflict resolution workflow:

1. Enable base plugins + one new mod
2. Export conflicts using VD Isolate Export Conflicts
3. Upload TSV to this dashboard
4. Identify conflicts needing attention
5. Patch in xEdit as needed
6. Create modgroups to hide harmless conflicts
7. Repeat for next mod

---

## Compatibility

- Works with **FO4Edit 4.1.5f** or later
- Designed for **Fallout 4** (base game and Anniversary Edition)
- Should work with any conflict export in TSV format

---

## About

Made by **Viable Dahlia** - a Fallout 4 settlement builder focused on architectural approaches to in-game construction.

- [Nexus Mods Profile](https://www.nexusmods.com/profile/viabledahlia)
- [GitHub](https://github.com/ViableDahlia)
- YouTube: [There's a mod for that!](https://www.youtube.com/@theresamodforthat)

---

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)

You are free to share and adapt this tool for any purpose, including commercially, as long as you give appropriate credit to **Viable Dahlia**.
