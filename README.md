# Atomic Nanoparticle Explorer — Windows Release Candidate

Interactive 3D generator and analyser for **atomistic models of face-centred-cubic (FCC) metal
nanoparticles** — gold, silver, platinum, palladium and copper. Every atom is generated
deterministically and drawn individually, and the coordination statistics of the resulting surface
are computed locally on your machine.

## ⚠ This is a testing Release Candidate

**Version 1.0.0-rc.1 is a pre-release published so that external testers can try it on their own
Windows computers. It is not a stable production release.** Please read
[KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md) before relying on any number it produces, and report what
you find using [TESTING_GUIDE.md](TESTING_GUIDE.md).

## This repository contains no source code

It is a **distribution-only** repository. There is no application or backend source code, no build
scripts and no Python environment here — only documentation, screenshots and the two downloadable
Windows packages attached to the [Releases](../../releases) page. The application source is not
published, and **no licence is granted** (see [KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md) § 4).

---

## Download

Get the packages from the **Releases** page of this repository (tag `v1.0.0-rc.1`):

| Package | File name | Size | SHA-256 |
|---|---|---|---|
| Installer | `Atomic Nanoparticle Explorer-Setup-1.0.0.exe` | 162 332 020 B (154.8 MiB) | `91b4bedc06703f7bdf1109abe7213a57491093e2dc6559c7f90b1b9067ac4f72` |
| Portable ZIP | `Atomic Nanoparticle Explorer-1.0.0-x64.zip` | 206 434 667 B (196.9 MiB) | `0698488fde2f95637ce95a5062299e281081e8f70af62b8374a3a6a46f82bd77` |

The SHA-256 values above are the checksums of the uploaded binaries. Optional integrity check in
PowerShell:

```powershell
Get-FileHash ".\Atomic Nanoparticle Explorer-Setup-1.0.0.exe" -Algorithm SHA256
```

## Supported Windows platform

* Windows 10 or Windows 11, **64-bit** (validated on Windows 11 25H2, x64)
* **No separate Python installation required**
* **No Node.js installation required**
* No WebView2 runtime, no administrator rights, no internet connection required — everything runs
  offline and all science is computed locally

## Installer (recommended)

1. Download `Atomic Nanoparticle Explorer-Setup-1.0.0.exe`.
2. Run it. Windows SmartScreen will warn (the binaries are unsigned — see below):
   **More info → Run anyway**.
3. Choose the installation folder. The installation is per-user, so **no administrator prompt
   appears**.
4. The wizard creates a Start Menu entry and a desktop shortcut. Start the app from either.

To remove it: *Settings → Apps → Installed apps → Atomic Nanoparticle Explorer → Uninstall*, the
Start Menu uninstaller, or `Uninstall Atomic Nanoparticle Explorer.exe` in the installation folder.
Your exported files are never touched.

## Portable ZIP (no installation)

1. Download `Atomic Nanoparticle Explorer-1.0.0-x64.zip`.
2. Extract it anywhere writable (a USB stick works) and run
   `Atomic Nanoparticle Explorer.exe` inside the extracted folder.
3. Keep the folder intact — `resources\backend\` holds the bundled scientific backend and
   `resources\app.asar` the user interface. Moving the `.exe` alone will not work.

## What it does

* **Five FCC metals** — Au, Ag, Pt, Pd, Cu with room-temperature lattice constants.
* **22 structure presets in 5 engines**: cube `{100}`, octahedron `{111}`, rhombic dodecahedron
  `{110}`, truncated octahedron, cuboctahedron, high-index `{210}/{211}/{310}/{321}`, custom facet
  mixes and user-defined *(hkl)*; concave pit-per-face shapes; nanorods, triangular and hexagonal
  nanoplates, bipyramids; multiply twinned decahedra and icosahedra.
* **Every atom is drawn** (up to ~113 000 of them) in one instanced mesh; rotate, zoom, switch
  colour modes (element, coordination number, surface vs bulk, site class, facet family), hide bulk
  atoms, resize the spheres without touching a coordinate.
* **Deterministic crystallography** — no random numbers: identical inputs always give identical
  coordinates, and the clipped lattice is cross-checked against an independent FCC construction.
* **Surface analysis** — coordination numbers (KD-tree, cross-checked against a brute-force count
  and ASE), site classes (bulk / terrace / edge / vertex / other surface), coordination
  distributions, bond statistics, exact facet areas and their fractions, particle volume and
  specific surface area.
* **Facet geometry** — the shape is the intersection of crystallographic half-spaces, with a
  cross-check of the pyramid identity `V = ⅓·Σ A_facet·d_facet`; inactive (non-exposed) facet
  families are reported instead of being silently drawn.
* **Particle size 0.4 – 12.0 nm**, with an optional shell mode that completes the outer atomic layer
  and an optional lattice-alignment mode that snaps facets onto crystallographic planes.

## Export support

| Format | Contents |
|---|---|
| **XYZ** | every atom position in Å plus a comment line (element, shape, lattice constant, atom count, size) |
| **CIF** | the same structure written with ASE (P1 cell = bounding box with vacuum) |
| **CSV** | all analysis metrics in long format: shape, facets, atom counts, site classes, coordination distribution, limitations |
| **PNG** | screenshot of the current 3D view |

Exporting opens the standard Windows save dialog with the file name and extension pre-filled; the
complete file is written and an error is reported instead of leaving a partial file behind. Large
particles produce large files — a 12 nm cube exports as roughly 9–10 MB of CIF.

## Unsigned application — SmartScreen guidance

The packages are **not code-signed** (a certificate is out of scope for this zero-cost build).
Windows will therefore show **"Windows protected your PC — Unknown publisher"** on first run:

1. Click **More info**, then **Run anyway**.
2. If your antivirus quarantines the file, allow it or use the portable ZIP in a folder you own.
3. Never disable SmartScreen globally for this — the warning is expected and only needs to be
   bypassed once per file.

## Known limitations (short version)

Unsigned binaries and the SmartScreen warning · no automatic updates · the uninstaller can leave the
(empty) installation folder behind · the models are **ideal, unrelaxed, coordination-only**
geometries — no relaxation, reconstruction, adsorbates or temperature effects, so they are suitable
for visualisation, teaching and preparing starting geometries, but **not** for predicting catalytic
activity or for matching a specific experimental particle.

Full list, including the applicability boundaries of the science:
[KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md).

## Troubleshooting

| Symptom | What to do |
|---|---|
| "Windows protected your PC" | More info → Run anyway (unsigned build). |
| Antivirus blocks the file | Allow it, or use the portable ZIP in a folder you own. |
| Empty or black 3D view | Update the graphics driver; the app otherwise falls back to software rendering. |
| Very slow with > 10 000 atoms | Set the sphere quality to *low* and/or reduce the diameter. |
| Requested size not exactly matched | "Lattice-aligned facets" is on: the achieved diameter is shown next to the slider. |
| Backend start-up message | Retry; for the portable ZIP re-extract the complete folder. |
| Small empty folder after uninstall | Known cosmetic issue — delete it manually. |

Detailed instructions: [INSTALLATION.md](INSTALLATION.md).

## Test reports

Testing steps, the result table to fill in and what **not** to include are described in
[TESTING_GUIDE.md](TESTING_GUIDE.md). Please send the completed report back to the person who shared
this link with you.
