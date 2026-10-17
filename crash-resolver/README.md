# Crash Resolver

**Version:** Level 1 (crash log only), first release, 16 October 2026
**File:** `crash-resolver.html` (single file, runs in the browser, nothing is uploaded)
**Part of:** Commonwealth Tools

## Summary

Drop in a Fallout 4 Addictol crash log. The tool explains in plain language what the game was doing when it crashed, shows the evidence, and suggests what to try first. An optional description box (for example a forum post) lets it check the poster's claims against the log.

## Bigger picture

Crash Resolver is the first tool in a planned three-level diagnostic path. Each level needs more input and gives a more precise answer.

| Level | Input | Answers | Status |
|---|---|---|---|
| 1. Crash log | Addictol crash log | What crashed, why, what to try first | Built |
| 2. Load order | Level 1 plus plugins.txt, modlist.txt, loadorder.txt (MO2) | Which of my mods are involved | Planned |
| 3. Full checkup | Level 2 plus a report from a local Python scanner | Which specific files are broken | Planned |

The design rule: someone with only a crash log should still get a useful answer in seconds.

## How to use

1. Open `crash-resolver.html` in a browser.
2. Paste the log into the first box, or drop the log file onto it, or use Choose file.
3. Optional: paste a description of the problem into the second box.
4. Select Analyse (choosing a file analyses automatically).
5. Read the results from the top: what happened, what to try in order, then the extra flags.
6. Use Copy summary to paste a plain-text result into a forum reply. It contains no file paths.

## What the page shows

1. **What happened.** Headline, confidence, a numbered chain of events, and the evidence.
2. **What to try, in order.** Numbered steps, most useful first.
3. **Checking the description.** Only appears when a description was entered. Each claim is marked Supported, Not shown in log, Present, Matches or Differs.
4. **Other things worth knowing.** Secondary flags.
5. **Checks that passed.** Reassurance for things that look fine.
6. **Details.** Call stack, environment, graphics modules, F4SE plugins, texture-name guess list, load order from the log.
7. **Share or keep.** Copy summary and Download parsed data (JSON).

## Rules in this version

Every finding has a confidence (high, medium, low) and a level tag. All rules here are level 1.

| ID | What it detects | Trigger | Confidence |
|---|---|---|---|
| `texture-rejected` | Direct3D refused to create a texture and the game used the empty result | A texture function in the top three probable frames or the exception line, plus a read from an address below 0x10000 (or a null base register) | High if RAX holds a recognised error code (invalid argument or out of memory), otherwise medium |
| `category-*` | Crash area when no specific rule matched: graphics, previs, scripts, meshes, physics, audio | Function names in the top four probable frames match the category dictionary | Low |
| `null-read` | Generic read from an empty address | Read below 0x10000 with no category match | Low |
| `unclassified` | Nothing matched | Fallback | Low |
| `wrapper` | A graphics wrapper (such as ENB or ReShade) is installed | d3d11.dll, dxgi.dll, d3d9.dll or d3d12.dll loaded more than once | Medium if a stack-scan frame points into the extra copy, otherwise low |
| `elevated` | Game ran as administrator | Elevated: Yes | Low |
| `version` | F4SE files named for a different game version | Version-stamped DLL names do not contain the game version | Medium |
| `limit` | Too many full plugins | Warn at 245 or more, error above 255 | Medium or high |
| `mem-*` | Memory figures may show pressure | Reported ratio above 0.9 or below 0.05 | Low |

### Claim checks (description box)

| Claim detected | Supported when |
|---|---|
| Texture | A texture function is in the main call stack |
| PRP or previs | A previs, precombine or combined-object function is in the main call stack |
| Memory | RAX holds an out-of-memory code |
| Scripts or Papyrus | A script engine function is in the main call stack |
| Meshes | A mesh or scene function is in the main call stack |
| ENB or ReShade | A graphics wrapper was detected |
| Game version | The version in the text matches the log |

"Not shown in log" means the log gives no evidence for the claim. It does not prove the claim is wrong.

## Worked example

Input: a public Reddit log (Addictol 1.7.0, Fallout 4 v1.11.240, 260 plugins).

Result: Direct3D refused to create a texture (RAX is `0x80070057`, invalid argument), then the game read from address 0 inside `BSGraphics::RegisterD3DTexture`. High confidence.

- Poster's "texture related": supported.
- Poster's "PRP": not shown in log. No previs or precombine function in the call stack.
- A graphics wrapper is loaded (two copies of d3d11.dll and dxgi.dll, plus d3dcompiler_46e.dll, which is characteristic of ENBSeries).
- The log cannot say which texture file was rejected.

## Architecture

The page is one file with five parts. Each can be changed without touching the others.

```
Raw log text
   |
1. Parser (Addictol)     turns text into a plain data object
   |
2. Rules and claim checks   read the data object, return findings
   |
3. Knowledge tables      GLOSSARY, CATEGORIES, HRESULTS, regexes
   |
4. Renderer              turns findings into the page
   |
5. UI                    input, file drop, copy, download
```

**Extension points**

- **New log format (for example Buffout 4):** add a parser that returns the same data object. Nothing downstream changes.
- **New crash pattern:** add an entry to `CATEGORIES`, or write a rule function and add it to `analyse()`.
- **New plain-language function names:** add to `GLOSSARY`.
- **New error codes:** add to `HRESULTS`.

### Parsed data shape

```
format, crashTime, gameVersion, loggerName, loggerVersion
exception: { type, address, module, offset, instruction, func }
access:    { kind, addr, instruction, baseReg, baseVal, computed }
process:   { uptimeMs, uptimeText, elevated }
specs:     { os, cpu, gpus[], ram{a,b}, vram{a,b} }
callStack: [ { idx, tag (P or S), addr, module, offset, func } ]
registers: { RAX: "0x...", ... }
modules:   [ { name, base } ]
f4se:      [ { name, version } ]
plugins:   { light, regular, total, list: [ { index, light, name } ] }
```

The working directory and command line are deliberately not parsed or stored, so the JSON download does not carry the user's folder paths.

## Known limits and assumptions

1. **Addictol logs only.** Buffout 4 logs are detected and politely refused.
2. **It cannot name the bad texture file.** Addictol logs do not include the path. Naming it needs levels 2 and 3.
3. **The wrapper check is a heuristic.** A duplicate d3d11.dll or dxgi.dll is treated as a wrapper, and the copy at the highest address is assumed to be the Windows one. ENB is inferred from the file name `d3dcompiler_46e.dll`.
4. **Memory figures are ambiguous.** The log does not say whether the first number is used or free, so the tool only flags a figure when it looks bad under either reading.
5. **Texture-name guess is a guess.** It matches plugin names against keywords such as tex, parallax, vivid, FaceGen, LOD, water and rocks. It will miss some texture mods and include some that are not.
6. **Claim detection uses keywords.** Unusual wording will be missed.
7. **Test step is unverified.** The advice to move a wrapper's `d3d11.dll` out of the game folder for one test is the author's judgement and has not been checked against a real fix.
8. **File size limit.** Files over 5 MB are refused.
9. **Fonts.** VT323 and Share Tech Mono load from Google Fonts. Without a connection the page falls back to a system monospace font.

## Tested so far

- Headless Chromium at 1100 px and 390 px wide: layout, no horizontal scroll, results correct on the Reddit log.
- Empty input, plain text and a Buffout 4 header all give a clear message.
- Copy summary and the clipboard fallback work in headless Chromium.
- **Not yet tested:** Firefox, Safari, real phones, your own logs, other crash types.

## Stress test checklist

Try each of these and note anything that looks wrong, confusing or too confident.

1. **Your own logs.** Run at least three different Addictol logs from the TMR build. Do the headline and chain match what you know about each crash?
2. **Different crash types.** Find logs that crash in scripts, meshes, physics or audio. Check the category fallback gives a sensible area and does not claim more than the log shows.
3. **Truncated log.** Paste only the first half. Does it fail clearly or give a misleading result?
4. **Windows line endings and odd encodings.** Open a log saved by different tools. Does parsing still work?
5. **No description, then a description.** Compare the two results. Does the claim table add value?
6. **Contradicting description.** Enter a different game version, or blame scripts. Does it say Differs or Not shown, without sounding certain?
7. **Plugin names.** Check the texture-name list on your own load order. Note misses and false matches.
8. **Browsers and devices.** Chrome, Edge, Firefox, Safari and a phone. Check drag and drop, Copy summary and the download.
9. **Offline.** Disconnect and reload. Does the page still work with fallback fonts?
10. **Wording.** Read the output as if you were the Reddit poster. Is anything unclear, alarming or too technical?

When reporting back, include the log (or the relevant lines), what the tool said, and what you expected.

## Roadmap

1. Fix whatever the stress test finds.
2. **Level 2:** accept plugins.txt, modlist.txt and loadorder.txt (MO2 only), cross-check them against the log, and add a tier tag to each finding.
3. **Level 3:** a read-only local Python scanner that checks DDS headers and resolves which mod wins each file path, writing a `report.json` that this page can load.
4. A Buffout 4 parser.
5. Link the tool into the shared Commonwealth Tools interface with a tool switcher.
