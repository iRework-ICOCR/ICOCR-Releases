# ICOCR and ICDB — Windows beta setup

**ICOCR 0.8.9.6-RC1 · ICDB 0.8.2-RC1**

ICOCR reads markings on integrated circuits and electronic components from microscope-camera images or saved images using local OCR. ICDB manages the component catalogue and pinout definitions used for lookups and references in ICOCR.

This beta includes both applications and a reduced demo database. The demo contains only a subset of parts; a missing database match does not necessarily mean the OCR failed.

## Downloads

Download both Windows application installers and the demo DATA archive from the release assets. For ICOCR, choose either the CPU build or the NVIDIA GPU build.

| Application | Installer |
| --- | --- |
| ICOCR — CPU | `ICOCR_0.8.9.6-RC1_CPU_Setup.exe` |
| ICOCR — NVIDIA GPU | `ICOCR_0.8.9.6-RC1_GPU_Setup.exe` |
| ICDB | `ICDB_0.8.2-RC1_Setup.exe` |

Compiled builds do not require Python. The ICOCR GPU installer bundles CUDA/cuDNN dependencies but requires a compatible NVIDIA GPU and driver. It falls back to CPU when GPU execution is unavailable. The CPU and GPU installations replace each other.

Use the actual application assets attached to the release. GitHub's automatic **Source code** archives are not application installers.

## 1. Install both programs

1. Run the chosen ICOCR installer and complete installation.
2. Run the ICDB installer and complete installation.
3. Close both programs before preparing the shared database.

If using portable builds instead, extract each complete application archive into its own folder. Launch `ICOCR.exe` and `ICDB.exe` from their respective folders. Do not copy only the EXEs or combine their supporting files.

## 2. Extract the demo DATA folder into Local AppData

Both applications must use the same shared DATA folder:

```text
%LOCALAPPDATA%\ICSuite\DATA
```

This is normally:

```text
C:\Users\YOUR_USERNAME\AppData\Local\ICSuite\DATA
```

1. Press **Win + R**, enter `%LOCALAPPDATA%`, and press **Enter**. Use Local AppData, not `%APPDATA%` (the Roaming folder).
2. Create an `ICSuite` folder if it does not exist.
3. Extract the demo database archive so its DATA contents end up directly inside `ICSuite\DATA`.
4. If the archive already contains a top-level `DATA` folder, extract that folder into `ICSuite`. If it contains the contents of DATA directly, extract them into `ICSuite\DATA`.

Avoid an extra nested `DATA\DATA` folder. Preserve all supplied subfolders and files.

The current applications support a catalogue at either of these locations:

```text
%LOCALAPPDATA%\ICSuite\DATA\db\components.sqlite
%LOCALAPPDATA%\ICSuite\DATA\components.sqlite
```

The second location is a supported legacy layout. Keep the supplied layout; do not create a second copy of the database in the other location. If both exist, the root-level legacy file takes priority.

**Existing users:** back up your current DATA folder before extracting anything. Keep the demo in a separate folder if you already have a working catalogue; do not overwrite your own database. Both programs can select that separate folder instead.

## 3. Launch ICDB and select DATA

1. Start **ICDB**.
2. Open the **Components** tab and check the displayed DATA path.
3. If it does not point to the extracted demo DATA folder, click **Choose DATA folder…** and select `%LOCALAPPDATA%\ICSuite\DATA` (or your chosen alternative).
4. Close and restart ICDB after changing the path.
5. Confirm that the component list contains demo records.

Choose the **DATA folder**, not the `db` subfolder or the SQLite file. Selecting a DATA folder changes the shared setting; it does not copy or move existing files. Restart both applications whenever this setting changes.

## 4. Build the SVG pinout diagrams

1. In ICDB, open **Pinout library**.
2. Save any pending pinout edits.
3. Click **Rebuild SVGs**.
4. Wait for the background operation to finish. The status reports diagrams rendered and links updated; inspect any skipped/error report.

**Rebuild SVGs also publishes the rebuilt references to ICOCR.** A separate Publish action is not required for this rebuild.

Only **accepted component-to-pinout links** are exported. Rebuild does not create missing pin definitions, accept suggestions or generate a pinout for every component in the database. If no diagrams are rendered, check whether the demo contains accepted links and supported pinout definitions.

Generated SVGs are stored under `DATA\pinouts` and linked through the shared database. Manually attached images are retained. Older generated SVG revisions remain on disk.

For later changes, save pin edits and use **Publish to ICOCR** to update accepted references. Use **Rebuild SVGs** when you need to regenerate all accepted diagrams, including unchanged ones.

## 5. Launch ICOCR and load the shared catalogue

1. Start **ICOCR** after ICDB finishes rebuilding.
2. Open **Settings → Database & Documentation** and check the shared DATA location. Use **Choose DATA folder…** if necessary, then restart both applications.
3. ICOCR loads the catalogue from the selected DATA folder at startup. If no catalogue appears, check the DATA path and the database locations described above.
4. Use **Reload database** after changes made in ICDB.
5. Scan a component whose record exists in the demo. Where an accepted, published reference exists, inspect the pinout panel and package/reference choices.

An absent pinout can mean there is no accepted link or published reference for that part. It does not establish that recognition failed.

## 6. Scan with a microscope camera

1. Connect the microscope camera. ICOCR starts the camera after its startup device search.
2. Select the required camera, capture backend and format. Click **Stop**, then **Start** after changing these settings.
3. Focus on the marking and reduce glare.
4. Drag a selection around one component, keeping the full marking inside the selection and unrelated PCB text outside it.
5. Click **Scan** or press **Enter**.
6. Review recognised text, candidate database matches and available references. Enable **Live recognition** for repeated-frame checks and tracking.

Use **Open image** to scan a saved image instead of a camera feed.

| Action | Control |
| --- | --- |
| Scan selection | **Scan** / **Enter** |
| Restore default selection | Right-click |
| Save a clean full-frame image | **Capture image** / **Space** |
| Toggle fullscreen | **F11** |
| Exit fullscreen | **Escape** |

## Troubleshooting

| Problem | Checks |
| --- | --- |
| ICDB component list is empty | Verify the DATA path, archive nesting and location of `components.sqlite`. Restart after selecting DATA. |
| ICOCR shows different records | Confirm both programs use the same DATA folder and database. Restart both; reload the catalogue in ICOCR. Check for two database copies. |
| Rebuild SVGs produces no diagrams | Inspect accepted links in Pinout library/Matching. Only accepted, renderable definitions are published. Review skipped/error details. |
| Pinout changes do not appear | Save edits, publish or rebuild in ICDB, then reload the catalogue in ICOCR. Check the package/reference selector. |
| Camera does not open | Close other camera applications, try DirectShow/Media Foundation, lower resolution/frame rate, and Stop/Start. |
| Marking is read incorrectly | Improve focus/lighting; tighten the selection without clipping text. Compare Fast scan and automatic cropping on/off using the same saved image. |
| Correct text has no database match | Search ICDB for that part. The reduced demo may not contain it. |
| GPU build uses CPU | Check **Settings → Tools** and `DATA\ICOCR.log` for the active processor and fallback reason. Verify NVIDIA GPU/driver compatibility. |
| Live recognition is paused | Multi-chip mode pauses Live. Recognition also sleeps after inactivity; movement or manual action wakes it. |
| Selected reading is not logged | Enable **Log clicked readings**. Close `confirmed.csv` in Excel and retry if it is locked. Check DATA write permissions. |

## Data and saved files

| Location | Purpose |
| --- | --- |
| `DATA\db\components.sqlite` or legacy `DATA\components.sqlite` | Shared component catalogue and pinout/link records |
| `DATA\pinouts` | Published diagrams and reference assets |
| `DATA\models` | Model files |
| `DATA\rules` | ICOCR modular recognition rules |
| `DATA\captures` | Clean full-frame captures |
| `DATA\confirmed` | Selected-reading JPEGs and `confirmed.csv` |
| `DATA\ICOCR.log` | ICOCR diagnostic log |
| `%LOCALAPPDATA%\ICSuite\ICOCR\config` | ICOCR per-user settings |

OCR inference runs locally. Explicit search/datasheet actions open your browser. Selected-reading logs record the user's choice, not independently verified component identity. Session history is cleared when ICOCR closes; saved captures and logs persist separately.

## Beta limitations and bug reports

OCR text, database matches and physical component identity are separate evidence. Verify the marking, package and manufacturer documentation before relying on a result. Pinout links and diagrams do not establish electrical interchangeability.

Automatic body cropping, chip detection and multi-chip scanning are experimental. Multi-chip mode reads at most ten chips per scan and pauses Live. An asterisk indicates multiple catalogue variants/descriptions, not confidence.

Report issues with both application versions, CPU/GPU build, Windows version, reproduction steps and relevant log/error details. For OCR failures, include a non-confidential sample image; for missing references, include the component name and ICDB rebuild result. Remove confidential data before posting.
