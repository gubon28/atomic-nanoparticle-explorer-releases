# Installation Guide — Atomic Nanoparticle Explorer 1.0.0

This guide covers the two Windows packages of the desktop application:

| Package | File | Best for |
|---|---|---|
| Installer | `Atomic Nanoparticle Explorer-Setup-1.0.0.exe` | normal use — Start Menu entry, desktop shortcut, uninstaller |
| Portable ZIP | `Atomic Nanoparticle Explorer-1.0.0-x64.zip` | running from a USB stick or without installing anything |

Both packages contain the complete application, including the Python scientific
backend. **Python and Node.js do not have to be installed** on the machine that
runs the app, and no administrator rights are required.

---

## 1. System requirements

* Windows 10 or Windows 11, 64-bit (validated on Windows 11 25H2, x64)
* ~600 MB of free disk space (installed ≈ 500 MB; the ZIP is ≈ 197 MiB compressed)
* A GPU with WebGL — or software rendering, which the packaged app falls back to
  automatically (slower with very large particles)
* No Python, no Node.js, no WebView2 runtime, no internet connection

---

## 2. Option A — Installer

1. Run `Atomic Nanoparticle Explorer-Setup-1.0.0.exe`.
2. If Windows SmartScreen shows **"Windows protected your PC"**, click
   **More info → Run anyway** (the binaries are unsigned — see
   [KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md)).
3. Choose the installation folder (default: a per-user folder under
   `%LOCALAPPDATA%\Programs`). Because the installation is per-user, no
   administrator prompt appears.
4. The wizard creates a **Start Menu entry** and a **desktop shortcut**.
5. Start the application from the shortcut. The first window appears after a few
   seconds while the bundled scientific backend starts.

### Removing the application

* **Settings → Apps → Installed apps → Atomic Nanoparticle Explorer → Uninstall**, or
* the uninstaller entry in the Start Menu folder, or
* `Uninstall Atomic Nanoparticle Explorer.exe` inside the installation folder.

The uninstaller removes the program files and the shortcuts. Your own files
(exported XYZ/CIF/CSV/PNG files) are never touched, and the per-user settings
folder `%APPDATA%\Atomic Nanoparticle Explorer` is kept so that a later
re-installation keeps your preferences.

---

## 3. Option B — Portable ZIP

1. Extract `Atomic Nanoparticle Explorer-1.0.0-x64.zip` anywhere you have write
   access (a USB stick works).
2. Run `Atomic Nanoparticle Explorer.exe` inside the extracted folder.
3. Nothing is installed and no shortcuts are created. Everything the application
   writes (exports, settings) goes to the locations you choose or to
   `%APPDATA%\Atomic Nanoparticle Explorer`.

Keep the extracted folder intact: `resources\backend\` holds the frozen Python
backend and `resources\app.asar` holds the user interface. Moving the `.exe`
alone will not work.

---

## 4. First run

1. The window opens on a default **Au nanocube, 2.5 nm** model.
2. The atom count, facet areas and coordination statistics appear in the right
   hand panel; the 3D view can be rotated with the mouse.
3. Change the element, the shape preset or the size slider and the model is
   regenerated immediately (deterministically — the same inputs always give the
   same coordinates).

If a message says the backend could not start, see §7.

---

## 5. Exporting a model

The export bar exports exactly what is on screen:

| Format | Contents |
|---|---|
| **XYZ** | every atom position in Å plus a comment line with element, shape, lattice constant and size |
| **CIF** | the same structure written with ASE (P1 cell = bounding box + vacuum) |
| **CSV** | all analysis metrics (shape, facets, atom counts, site classes, coordination distribution, limitations) |
| **PNG** | a screenshot of the current viewer state |

Choosing a format opens the standard Windows save dialog; the file name and
extension are pre-filled. Large particles produce large files (a 12 nm cube,
113 491 atoms, is a ~10 MB CIF) — the complete file is always written, and an
error is reported rather than an incomplete file being left behind.

## 6. Privacy and offline behaviour

* The application never connects to the internet. There are no accounts, no
  telemetry, no update checks and no analytics.
* The scientific backend runs as a local process on `127.0.0.1` with a
  kernel-assigned port and a per-session token; the user interface cannot reach
  anything else.
* Models, exports and preferences stay on the machine. All calculations run
  locally; the application is fully usable in an air-gapped environment.

## 7. Troubleshooting

| Symptom | Cause / fix |
|---|---|
| "Windows protected your PC" on first run | Unsigned build → **More info → Run anyway**. |
| Antivirus quarantines the `.exe` | Unsigned Electron/PyInstaller binaries are sometimes flagged; allow the file or use the portable ZIP. |
| Blocked by an administrator / policy | The installer is per-user; if policy still blocks execution, use the portable ZIP in a folder you own. |
| "The scientific backend did not start" dialog | Usually antivirus interference or an incomplete extraction. Retry; for the ZIP re-extract the whole folder. |
| Empty or black 3D view | WebGL unavailable — update the graphics driver; the app otherwise falls back to software rendering. |
| Very slow with > 10 000 atoms | Set sphere quality to *low* and/or reduce the diameter. |
| Requested size not matched exactly | "Lattice-aligned facets" is on: the achieved diameter is shown next to the slider. |
| Small empty folder left after uninstall | Known cosmetic issue — the folder can be deleted manually (see [KNOWN_LIMITATIONS.md](KNOWN_LIMITATIONS.md)). |

## 8. Verifying an installation (optional)

| Check | Expected |
|---|---|
| Start Menu / desktop shortcut icon | the navy-blue atomic cluster icon |
| `...\resources\app.asar` present in the install folder | yes |
| `...\resources\backend\atomic-nanoparticle-backend.exe` present | yes |
| Application starts, 3D model renders | yes |
| Closing the window leaves no `atomic-nanoparticle-backend.exe` in Task Manager | yes |
