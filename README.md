# ICOCR

ICOCR reads markings on integrated circuits and electronic components from microscope-camera images or saved images using local optical character recognition (OCR). It helps technicians read markings, look up candidate parts and consult references during electronics repair.

**Current beta: 0.8.9.6-RC1.** This repository distributes compiled releases and documentation. Application source code is not included.

## Features

- Live microscope-camera preview and scanning of saved images.
- Manual selection of a component, single scans and live recognition with tracking and repeated-frame checks.
- Matching against a separate ICDB component catalogue, with exact matches and labelled, unverified suggestions.
- Linked pinout/reference images, including SVG support, where available in ICDB.
- Clean image capture and optional logging of selected readings with snapshots.
- CPU processing and an NVIDIA GPU build with CPU fallback.
- Configurable recognition rules and an adjustable sidebar.
- Experimental body cropping, chip detection and multi-chip scanning.

**Reading a marking, finding a database match and establishing a component's identity are different steps.** Verify results against the component, package and manufacturer documentation. A suggested match is not proof of identity or electrical equivalence.

## Download and installation

Open this repository's **Releases** section, select **0.8.9.6-RC1**, and download the appropriate application asset.

| Build | Use |
| --- | --- |
| CPU | CPU processing; no NVIDIA GPU required. |
| GPU | NVIDIA GPU processing with a compatible, current driver; falls back to CPU when GPU execution is unavailable. |

Run the supplied setup EXE, or extract the complete portable ZIP and start `ICOCR.exe`. Keep the portable application's accompanying files and folders together. Compiled builds do not require Python. The GPU installer bundles CUDA/cuDNN dependencies. CPU and GPU installations replace each other.

Read the selected release's notes for its exact asset names and requirements. GitHub's automatic **Source code** archives contain this repository's documentation, not the application.

## First scan

1. Connect the microscope camera and start ICOCR. The camera starts after the startup device search.
2. If needed, choose the camera, backend and capture format. Use **Stop**, then **Start** to apply camera changes. Alternatively, use **Open image**.
3. Drag a selection around one component, including its complete marking and excluding unrelated PCB text.
4. Click **Scan** or press **Enter**.
5. Review the recognised text and any database suggestions. Enable **Live recognition** for repeated-frame reading checks and tracking.

## Connect ICDB

The component catalogue is separate from ICOCR. Use **Settings → Database & Documentation → Choose DATA folder…** to select the shared DATA folder, then restart both ICOCR and ICDB. Selecting a folder does not move existing files.

ICDB manages the catalogue. ICOCR expects its database schema, not an arbitrary CSV or SQLite file. Use **Reload database** after catalogue changes. Pinouts and reference availability depend on the records and files in your shared DATA.

## Controls

| Action | Control |
| --- | --- |
| Select a component | Drag over the image |
| Restore the default selection | Right-click |
| Scan the selection | **Scan** or **Enter** |
| Save the full frame without overlays | **Capture image** or **Space** |
| Toggle fullscreen | **F11** |
| Exit fullscreen | **Escape** |

Clicking a recognised reading can open a browser search. With **Log clicked readings** enabled, selecting a reading also saves a snapshot and CSV record. These records document the user's selection; they do not independently verify component identity.

## Data and privacy

OCR inference runs locally. Explicit web searches and datasheet actions open your browser. Selected-reading logs and captures are written locally; review them before sharing a bug report.

For compiled Windows builds, the default shared DATA folder is `%LOCALAPPDATA%\ICSuite\DATA`. A previously selected folder or environment override can change this location. Use **Open data folder** to find the active folder.

| Location | Contents |
| --- | --- |
| `DATA/captures/` | Full-frame JPEG captures |
| `DATA/confirmed/` | Selected-reading snapshots and `confirmed.csv` |
| `DATA/ICOCR.log` | Application diagnostics |
| `DATA/rules/` | Active modular recognition rules |
| `%LOCALAPPDATA%\ICSuite\ICOCR\config` | Per-user settings and configuration |

Session history is cleared when ICOCR closes. Captures and saved logs persist separately. Uninstalling retains settings and shared DATA according to the supplied installation documentation.

## Experimental features and limitations

- Body detection and automatic cropping can choose an unsuitable region. Compare with a manual selection if results are poor.
- Multi-chip scanning is off by default, reads up to ten chips per scan, and pauses live recognition while enabled.
- An asterisk indicates multiple catalogue variants or descriptions, not confidence.
- Short markings, glare, blur and surrounding PCB text can cause ambiguous or incorrect readings.
- Recognition does not establish package type, pin functions or electrical equivalence.

## Documentation and support

- [Beta release notes](RELEASE_NOTES.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

Report bugs through this repository's **Issues** tab. Include the version, CPU/GPU build, operating system, reproduction steps, expected/actual result and relevant log excerpt. For camera or OCR issues, include the camera format and a sample image where possible. Remove confidential information before posting.
