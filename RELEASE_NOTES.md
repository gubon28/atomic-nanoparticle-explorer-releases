# Release Notes — Atomic Nanoparticle Explorer 1.0.0 (release candidate)

**Status:** release candidate **published for external testing** as the pre-release
`v1.0.0-rc.1` (Windows). The application source code is **not** published and no
licence is granted — these packages are distributed for testing only, and not as a
stable production release.
**Date:** 2026-10-15
**Platform:** Windows 10/11 x64, offline desktop application (Electron shell +
frozen Python backend) plus the original browser build.

---

## 1. What 1.0.0 adds

### Offline Windows desktop application
* Electron shell + PyInstaller-frozen FastAPI/ASE/NumPy/SciPy backend, shipped as
  an NSIS installer and a portable ZIP. No Python, Node.js or WebView2 needed on
  the target machine.
* Fully offline: the renderer loads `file://` content with
  `connect-src 'none'`, and every backend call goes through a main-process IPC
  proxy. No accounts, telemetry, update checks or analytics.

### Desktop security and lifecycle
* Backend binds `127.0.0.1` on a kernel-assigned port (port 0) and requires a
  per-session token — the token stays in the main process and is never exposed to
  the renderer.
* `contextIsolation: true`, `nodeIntegration: false`, `sandbox: true`, minimal
  preload surface, navigation/new-window/webview denied, external links handed to
  the OS browser.
* Single-instance lock, graceful shutdown on quit, hard kill fallback, and a
  parent-process watchdog in the backend so a crashed shell cannot leave an
  orphaned backend process behind (fixed in this cycle).
* Readiness is only reported after `/api/health` answers with the token, never
  when the socket is merely bound.

### Application branding
* Custom application icon (dark navy, FCC atomic cluster, cyan/blue spheres with
  depth shading), generated locally by `tools/make_app_icon.py` — no third-party
  artwork. It is embedded in the executable, the NSIS installer and uninstaller,
  the window/taskbar icon, and every shortcut the installer creates.
* The icon ships as a multi-resolution `.ico` (16/24/32/48/64/128/256 px) with
  simplified frames for 16/24/32 px so it stays readable at list sizes.

### Scientific export integrity (defect fix)
* **Fixed:** non-JSON backend responses were truncated at 4 000 characters in the
  renderer IPC proxy, silently corrupting large XYZ, CIF and CSV exports
  (measured before the fix: a 98 376-character CIF came out as 4 000 characters).
* Exports are now written from the main process, byte-for-byte, to a
  user-chosen path with the correct file name and extension; nothing is
  silently truncated and failures are reported instead of leaving partial files.
* Verified byte-identical against independently computed reference files up to a
  **12 nm, 113 491-atom cube** (XYZ 5.4 MB, CIF 10.4 MB, CSV complete).

### Verification (all re-run for this candidate)
* Backend scientific test suite: **84 passed**.
* Verification-harness regression tests: **10 passed**.
* Built-in validation suite: **22 checks**, 0 failures across the element/size sweep.
* Desktop application suite: **26/26** in development mode and **26/26** on the
  packaged application.
* Export-integrity matrix (small/medium/large/maximum × XYZ/CIF/CSV):
  every file byte-identical, atom counts consistent (63 / 1 099 / 12 195 / 113 491).
* Sidecar smoke test: **8/8** (ephemeral port, 401 without token, generation,
  shutdown, exit code, orphan guard).
* Installed per-user package: installer, shortcuts, launch, exports and uninstall
  verified on Windows 11.

---

## 2. Scientific functionality (unchanged core)

* **5 FCC metals** — Au, Ag, Pt, Pd, Cu with room-temperature lattice constants
  and derived nearest-neighbour distances.
* **22 structure presets in 5 engines**: convex polyhedra (cube, octahedron,
  rhombic dodecahedron, truncated octahedron, cuboctahedron, high-index
  {210}/{211}/{310}/{321}, custom facet mixes, user-defined hkl), concave
  pit-per-face variants, nanorods, triangular/hexagonal nanoplates, bipyramids,
  and multiply twinned decahedra/icosahedra.
* **Deterministic atom generation** — no random numbers; identical inputs always
  give identical coordinates, and the clipped lattice is cross-checked against an
  independent FCC construction.
* **Coordination analysis** — KD-tree neighbour counts (cross-checked against a
  brute-force O(N²) count and ASE's own neighbour list), site classes (bulk,
  terrace, edge, vertex, other surface), coordination distributions, bond
  statistics, and five colour modes.
* **Exports** — XYZ (exact coordinates), CIF (written with ASE), analysis CSV
  (every metric, long format), PNG screenshot of the current view.

---

## 3. Known limitations

See [KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md). Short version: the binaries are
**unsigned** (SmartScreen warning), there is no auto-update, the uninstaller
leaves an empty folder behind, unattended installation from a non-interactive
service session was not validated, and the models are ideal unrelaxed
coordination-only geometries that are not a substitute for DFT/MD or for
matching a specific experimental particle.

---

## 4. Not included in this candidate

* **No code signing** — and therefore no SmartScreen reputation.
* **No source code publication**: the installer and the portable ZIP are published
  as release assets in a distribution-only repository; the application, backend
  and build sources remain unpublished.
* **No licence is granted** — deliberately undecided (see
  [KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md) § 4).
* **No automatic updates** — a new version means downloading a new package.
* Later release-preparation phases (packaging polish, distribution workflow) are
  not part of this candidate, and the version is **not** promoted to stable.
