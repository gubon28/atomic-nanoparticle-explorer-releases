# Testing Guide — Atomic Nanoparticle Explorer v1.0.0-rc.1

Thank you for testing this Release Candidate on your Windows computer. The goal is to find out
whether it installs cleanly, runs without any developer tools, and produces usable 3D models and
export files.

Please work through the checklist below in order and note **PASS** or **FAIL** for each step. A short
screenshot of anything that looks wrong helps.

**Please do not send us personal files, credentials, licence keys or detailed information about your
system beyond the Windows version — none of that is needed, and please do not include it in a report.**

---

## Before you start

* Windows version you are testing on: `Windows 10 / 11, 64-bit?` — you can find it under
  *Settings → System → About*.
* Confirm that **Python is not installed** on the machine (or note it if it is — the app must not
  need it): open a Command Prompt and run `python --version`. "not recognized" is the expected,
  ideal result.
* Confirm that **Node.js is not installed** (`node --version`) for the same reason.

## Checklist

| # | What to check | Expected result | PASS/FAIL |
|---|---|---|---|
| 1 | Download the installer from the Releases page | Download completes; file size 162 332 020 bytes | |
| 2 | Run the installer | Setup wizard opens; SmartScreen "Unknown publisher" appears and can be bypassed with **More info → Run anyway** | |
| 3 | Install (per-user, no admin prompt) | Installation completes without asking for administrator rights | |
| 4 | Application icon | The desktop shortcut and Start Menu entry show the navy-blue atomic-cluster icon (not a generic/blank icon) | |
| 5 | Launch from the shortcut | The window opens and shows a 3D gold nanocube; no command window appears | |
| 6 | Launch without Python/Node.js | The application starts on the machine where neither is installed | |
| 7 | 3D interaction | Rotate with the mouse, zoom, and the model responds smoothly | |
| 8 | Material change | Switching the element (Au → Ag/Pt/Pd/Cu) regenerates the model and re-colours the atoms | |
| 9 | Morphology change | Switching the shape preset (cube, octahedron, truncated octahedron, cuboctahedron, rod, plate, icosahedron …) shows a clearly different particle | |
| 10 | Size change | Moving the size slider changes the particle size; the achieved diameter and atom count update | |
| 11 | Coordination information | The right-hand panel shows coordination numbers, site classes (bulk / terrace / edge / vertex) and facet data | |
| 12 | Validation report | The validation panel reports all checks as passing (22 checks) | |
| 13 | XYZ export | Export → XYZ: the native save dialog appears, the file is written, and it opens in a text editor with one coordinate line per atom | |
| 14 | CIF export | Export → CIF: file written and readable (e.g. opens in VESTA, OVITO, Avogadro or a text editor) | |
| 15 | CSV export | Export → CSV: file written and opens in Excel with the analysis rows | |
| 16 | PNG export | Export → PNG: screenshot of the current 3D view is saved | |
| 17 | Large model export | Try a large particle (e.g. 8–12 nm) and export CIF: the file is complete (not truncated) and larger than a few MB | |
| 18 | Shutdown | Close the window: the app exits and **no** `atomic-nanoparticle-backend.exe` remains in Task Manager | |
| 19 | Uninstall | Uninstall via *Settings → Apps*: the app is removed and the shortcuts (desktop + Start Menu) disappear | |
| 20 | Portable ZIP | Extract the ZIP to a USB stick or another folder and run the `.exe` inside it — it also works without installation | |

## Report template

```
Tester name (optional):
Date:
Windows version (from Settings → System → About):
Python installed?  (yes/no)     Node.js installed?  (yes/no)
Machine: laptop/desktop, CPU, GPU (optional):

Step 1  PASS/FAIL  - note
Step 2  PASS/FAIL  - note
...
Step 20 PASS/FAIL  - note

Errors seen (exact message if any):
Screenshots attached (yes/no):
Performance / usability feedback (optional):
```

## Optional feedback we would like

* Time from double-click to the first 3D model appearing.
* How smooth rotation and zoom feel with a large particle (> 10 000 atoms) and whether the *low*
  sphere quality helps.
* Anything that was confusing in the interface, and the first thing you tried to do that did not
  work.
* Whether the exported XYZ/CIF/CSV files opened correctly in the tool you normally use.
* Whether the icon and shortcuts look right after installation.

## What this build is not

It is an **unsigned testing Release Candidate**, not a stable release: expect the SmartScreen
warning, no automatic updates, and idealised (unrelaxed) scientific models. See
[KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md) for the full list.
