# Known Limitations — Atomic Nanoparticle Explorer 1.0.0

This file lists what the release candidate does **not** do, what was not fully
tested, and where the scientific model stops being applicable. Nothing here is
hidden in the application itself: the same modelling caveats are shown in the UI
and written into every CSV export.

---

## 1. Distribution and packaging limitations

### 1.1 The executables are unsigned
There is no code-signing certificate for this build. Windows therefore shows
**"Windows protected your PC — Unknown publisher"** the first time the installer
or the portable executable is started (**More info → Run anyway**). SmartScreen
reputation is never built up, so the warning can reappear on other machines.
Signing requires a paid certificate and is out of scope for this zero-cost build.

### 1.2 No automatic updates
The application has no updater. A new version means downloading and installing a
new package manually.

### 1.3 Empty installation folder left behind after uninstall
The uninstaller removes the program files, shortcuts and registry entries, but
on the machines tested it leaves the (empty) installation folder itself. This is
cosmetic; the folder can be deleted by hand. Low priority, not investigated in
this phase.

### 1.4 Unattended installation in a non-interactive service session
The **supported** installation path is the per-user installation started from a
normal desktop session, which has been validated end to end (install, launch,
export, uninstall). A silent installation launched from a non-interactive service
session (Windows session 0) did not complete and its cause was not investigated;
prefer the interactive installer, or the portable ZIP for automated deployment.

### 1.5 One icon per build, one language
The installer is English-only and the icon has no localised variants.

---

## 2. Testing limitations

* **No clean machine** without Python and Node.js was available. What is verified
  is that the packaged app runs its bundled interpreter
  (`resources\backend\…`) and never touches a development environment or an
  installed Python.
* **Interactive installer wizard steps** (mouse clicks in the setup UI) were not
  automated. The installer itself, the resulting file layout, the shortcuts and
  the uninstaller were verified programmatically.
* **The native Windows save dialog** used for exports was not driven by an
  automated click. Export integrity was verified through the same code path with
  a scripted destination directory; the dialog itself is the standard OS dialog.
* **WebGL** was validated with the hardware renderer where available and with
  software rendering (SwiftShader) in the automated tests. Some GPU/driver
  combinations may still render differently.
* **Visual checks** of the application window were made through automated
  screenshots, not by a person sitting in front of the machine.
* The application icon was verified by extracting it from the built executable
  and from the installer, and by inspecting the shortcut targets — not by
  looking at a Windows Explorer window.
* **Very large exports** (12 nm, 113 491 atoms ≈ 10 MB CIF) are written
  completely, but the validation harness bounds its own independent CIF parsing
  for those sizes (the ASE reader scales quadratically), so the largest CIF files
  are checked record-by-record rather than fully re-parsed by ASE.

---

## 3. Scientific modelling assumptions and applicability boundaries

These are the assumptions the application states about its own models.

1. **Ideal, unrelaxed models.** Atoms sit on perfect FCC sites. No surface
   relaxation, reconstruction, adatoms, steps or thermal disorder are included.
2. **Ideal Wulff-type polyhedra.** Real nanocrystals of the same size usually
   show rounded corners, stepped {111} facets and a distribution of shapes.
3. **Coordination-only site classification.** Bulk FCC coordination is 12, so any
   atom with CN < 12 is "undercoordinated" — but CN alone cannot separate
   chemistries: a {111} terrace atom of a small particle and a {100} terrace atom
   can share the same CN, and a subsurface atom just below a facet can look like
   a terrace atom.
4. **Terrace / edge / corner by counting facet planes** within a tolerance
   (1 = terrace, 2 = edge, 3+ = vertex). Atoms far from every facet plane are
   grouped as "other surface" even when they are clearly surface atoms.
5. **The {110} surface is grooved, not flat.** Its second atomic layer is exposed
   and therefore reported as "other surface" rather than as {110} terrace atoms,
   so the {110} terrace count understates the true {110} surface population.
6. **Geometric facet areas vs discrete atom counts.** Areas come from the
   continuous polyhedron, atom counts from discrete lattice sites; the two agree
   only within one lattice spacing, and small particles (a few hundred atoms)
   show large relative differences.
7. **Ideal plane areas.** Facet areas ignore atomic roughness; experimental
   surface areas from gas adsorption (BET) will be larger.
8. **Single-element, single-crystal models** (plus explicitly multiply twinned
   decahedra/icosahedra built from twin-related grains).
9. **No electronic structure, magnetism, ligand/solvent coverage, support
   interaction or temperature effects.** For catalytic activity predictions these
   are starting geometries only and require DFT or MD relaxation.

### Applicability summary

| Use case | Suitable |
|---|---|
| Teaching, method development, visualisation of facet/site statistics | yes |
| Preparing starting geometries for DFT/MD | yes |
| Estimating geometric surface areas / site ratios of ideal shapes | yes, with the caveats above |
| Predicting catalytic activity, adsorption energies or experimental morphologies | no |
| Matching a specific experimental particle | no |

---

## 4. Licensing and distribution

No licence is granted for this software. The licence for this project has **not**
been decided yet, and publishing one requires explicit approval. The application
source code is not published; the Windows packages are distributed **for testing
only**, as pre-release assets of a distribution-only repository. All rights
reserved by default.
