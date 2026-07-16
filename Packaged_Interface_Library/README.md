# TrackVision

**Version:** 5.11 (Build Revision 34462)    
**Platform:** Windows (x64)

---

## Overview

**TrackVision** is a medical imaging and surgical navigation desktop application. It is a customized distribution of [3D Slicer](https://www.slicer.org/) — an open-source platform for medical image informatics, image processing, and 3D visualization.

The application is designed for **spine tracking, surgical navigation, and image-guided interventions**, integrating real-time instrument tracking with medical image visualization.

---

## Installation Directory Structure

```
C:\Program Files\TrackVision\
│
├── TrackVision.exe              # Main application launcher (8.1 MB)
├── Uninstall.exe                # Application uninstaller
│
├── bin\                         # Core binaries and runtime libraries
│   ├── TrackVisionApp-real.exe  # Actual application executable
│   ├── PythonSlicer.exe         # Embedded Python interpreter
│   ├── SlicerDesigner.exe       # Qt Designer with Slicer plugins
│   ├── Qt5*.dll                 # Qt 5 framework libraries
│   ├── vtk*.dll                 # VTK 9.6 (Visualization Toolkit) libraries
│   ├── ITK*.dll                 # ITK 5.4 (Insight Toolkit) libraries
│   ├── python312.dll            # Python 3.12 runtime
│   ├── OpenIGTLink.dll          # OpenIGTLink real-time device communication
│   ├── igtlio*.dll              # IGT Link I/O libraries
│   ├── dcm*.exe                 # DCMTK DICOM utilities
│   ├── resources\               # Application resource files
│   └── translations\            # Localization files
│
├── lib\                         # Application libraries
│   ├── SpineTracker2-5.11\      # Core SpineTracker2 modules
│   │   ├── cli-modules\         # Command-line interface modules
│   │   │   ├── ResampleDTIVolume
│   │   │   └── ResampleScalarVectorDWIVolume
│   │   ├── qt-loadable-modules\ # Qt-based loadable modules (244 DLLs)
│   │   └── qt-scripted-modules\ # Python scripted modules
│   │       ├── Home.py          # Main application home module (~523 KB)
│   │       ├── DICOM.py         # DICOM browser and import
│   │       ├── SegmentEditor.py # Image segmentation tools
│   │       └── ...              # Additional scripted modules
│   ├── Python\                  # Embedded Python 3.12 distribution
│   ├── PlusServer\              # PLUS Toolkit server for device tracking
│   ├── QtPlugins\               # Qt plugin libraries
│   ├── site-packages\           # Python site packages
│   └── cmake\                   # CMake configuration files
│
├── include\                     # C/C++ header files
│   └── json\                    # JSON library headers
│
├── share\                       # Shared data and resources
│   ├── SpineTracker2-5.11\
│   │   ├── ColorFiles\          # Color lookup tables
│   │   ├── OrientationMarkers\  # 3D orientation marker models
│   │   ├── ParameterSets\       # Preset parameter configurations
│   │   ├── Slicer.crt           # SSL certificate bundle
│   │   └── Wizard\              # Extension wizard templates
│   ├── ITK-5.4\                 # ITK shared resources
│   └── doc\                     # Documentation
│
├── libexec\                     # Internal executables
│   └── SpineTracker2-5.11\      # Extension wizard scripts
│
├── modulefiles\                  # Organization-specific config
│   └── TrackVision-34462.ini    # Module configuration
│
└── [configuration files]
    └── TrackVisionLauncherSettings.ini  # Launcher environment config
```

---

## Key Dependencies & Technology Stack

| Component | Version | Purpose |
|---|---|---|
| **3D Slicer** | Custom build (5.11-based) | Core medical imaging platform |
| **VTK** | 9.6 | 3D visualization and rendering |
| **ITK** | 5.4 | Image processing and segmentation |
| **Qt** | 5.x | GUI framework |
| **Python** | 3.12 | Scripting and module development |
| **DCMTK** | — | DICOM image handling |
| **OpenIGTLink** | — | Real-time surgical device communication |
| **PLUS Toolkit** | — | Data acquisition from tracking/imaging devices |
| **PythonQt** | — | Python-Qt bindings |
| **OpenSSL** | 1.1 | Secure network communication |
| **TBB** | 12 | Multi-threading (Intel Threading Building Blocks) |

---

## Modules & Features

### Imaging & Visualization
- **Volume Rendering** — 3D volume visualization of medical images
- **Segmentation** — Manual and semi-automatic image segmentation tools
- **DICOM** — Full DICOM browser, import/export, and query/retrieve (DIMSE)
- **Markups** — Anatomical landmark placement and measurement
- **Colors** — Color lookup table management
- **Transforms** — Spatial transformation tools
- **Models** — 3D surface model visualization

### Surgical Navigation & Tracking
- **OpenIGTLink IF** — Real-time communication with tracking systems and imaging devices
- **OpenIGTLink Remote** — Remote device control
- **PLUS Remote** — PlusServer integration for data acquisition
- **Fiducial Registration Wizard** — Point-based registration between image and physical space
- **Pivot Calibration** — Surgical tool calibration via pivot method
- **Breach Warning** — Safety alerts when instruments approach critical structures
- **Path Explorer** — Surgical trajectory planning and navigation
- **Landmark Detection** — Automatic/assisted anatomical landmark identification
- **Watchdog** — Monitoring of tracking tool status and connections
- **Volume Reslice Driver** — Real-time image reslicing driven by tracked tools
- **Ultrasound Remote Control** — Ultrasound device remote control interface
- **Ultrasound Snapshots** — Capture ultrasound image snapshots
- **Volume Reconstruction** — 3D volume reconstruction from tracked 2D images
- **Video Util** — Video stream management

### Analysis & Utilities
- **Segment Statistics** — Quantitative analysis of segmented regions
- **Crop Volume / Crop Volume Sequence** — Region-of-interest cropping
- **Screen Capture** — Screenshot and video recording
- **Line Profile** — Intensity profile measurement along lines
- **Sequence Replay** — Temporal data playback
- **Viewpoint** — Camera viewpoint management
- **Web Server** — Built-in web server for remote access
- **Collect Points** — Point cloud collection from tracked tools
- **Transform Processor** — Real-time transform computation and filtering
- **Create Models** — Procedural 3D model generation
- **Generalized Reformat** — Advanced image reformatting

### Data Management
- **Subject Hierarchy** — Hierarchical data organization (patient → study → data)
- **Tables** — Tabular data display and management
- **Plots** — Data charting and plotting
- **Texts** — Text annotation management
- **Sequences** — Time-series data management
- **Metafile Importer** — Import tracked ultrasound sequences (MHA/MHD)
- **Units** — Measurement unit management

---

## How to Run

### Launch the Application
```
"C:\Program Files\TrackVision\TrackVision.exe"
```

### Command-Line Options
```
TrackVision.exe [options]
    --no-splash           Launch without splash screen
    --no-main-window      Launch without main window (headless)
    --help / -h           Show help
    --version             Show version information
    --home                Open home directory
    --program-path        Show program path
    --settings-path       Show settings path
    --temporary-path      Show temporary path
```

### Embedded Python Console
```
"C:\Program Files\TrackVision\bin\PythonSlicer.exe"
```

---

## DICOM Utilities

The application ships with DCMTK command-line utilities located in `bin\`:

| Utility | Description |
|---|---|
| `dcmdump.exe` | Dump DICOM file contents |
| `dcm2xml.exe` | Convert DICOM to XML |
| `dcmdjpeg.exe` | Decompress JPEG-encoded DICOM |
| `dcmqrscp.exe` | DICOM Query/Retrieve SCP |
| `echoscu.exe` | DICOM verification (C-ECHO) |
| `storescp.exe` | DICOM storage SCP |
| `storescu.exe` | DICOM storage SCU |
| `img2dcm.exe` | Convert images to DICOM |
| `dump2dcm.exe` | Convert dump to DICOM |
| `dsr2html.exe` | Convert DICOM SR to HTML |
| `dsr2xml.exe` | Convert DICOM SR to XML |
| `dsrdump.exe` | Dump DICOM SR contents |
| `xml2dcm.exe` | Convert XML to DICOM |
| `xml2dsr.exe` | Convert XML to DICOM SR |

---

## System Requirements

- **OS:** Windows 10/11 (64-bit)
- **Runtime:** Microsoft Visual C++ 2015-2022 Redistributable (x64) — bundled
- **GPU:** OpenGL 3.2+ compatible GPU recommended for 3D rendering
- **Display:** 1920×1080 or higher recommended

---

## Uninstallation

Run the included uninstaller:
```
"C:\Program Files\TrackVision\Uninstall.exe"
```

---

## License & Credits

TrackVision is built upon [3D Slicer](https://www.slicer.org/), an open-source software distributed under a BSD-style license. Additional components include:

- **VTK** — BSD License
- **ITK** — Apache 2.0 License
- **Qt** — LGPL / Commercial License
- **DCMTK** — BSD-style License
- **OpenIGTLink** — BSD License
- **PLUS Toolkit** — BSD License

