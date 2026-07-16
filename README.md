

<h1 align="center">TrackVision: A Modular Open-Source Platform for Real-Time Spine Surgical Navigation</h1>


<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/CMake-064F8C?style=for-the-badge&logo=cmake&logoColor=white" alt="CMake"/>
  <img src="https://img.shields.io/badge/Qt-41CD52?style=for-the-badge&logo=qt&logoColor=white" alt="Qt"/>
  <img src="https://img.shields.io/badge/OpenIGTLink-005A9C?style=for-the-badge&logoColor=white" alt="OpenIGTLink"/>
  <img src="https://img.shields.io/badge/PlusServer-orange?style=for-the-badge" alt="PlusServer"/>
  <img src="https://img.shields.io/badge/SlicerCAT-1E90FF?style=for-the-badge" alt="SlicerCAT"/>
</p>

---

## About

Minimally Invasive Spine Surgery (MISS) procedures — pedicle screw placement, vertebroplasty, facet-joint injection, and percutaneous biopsy demand sub-millimetre targeting precision within an anatomically constrained operative field. While 3D Slicer offers powerful image-guided tracking functionality, it is limited by a fixed coordinate system, tool centric only viewing, and its monolithic architecture that prevents standalone, reusable deployment. **TrackVision** is an open-source navigation platform that repackages Slicer3D's image guided tool tracking (IGTT) capabilities into an independent, standalone application built on the SlicerCAT (Slicer Custom Application Template) using Qt, CMake, and VTK. It integrates seamlessly with the existing open source IGT ecosystem — PLUS Toolkit, SlicerIGT, IGSIO, and OpenIGTLink with PLUS Toolkit launched automatically as an internally supervised subprocess, eliminating manual server management. TrackVision supports three concurrent guidance modalities (2D–2D fluoroscopy overlay, 2D–3D multi-planar CT reslicing, and 3D volumetric navigation) and dynamically switches between VTK, RAS, and LPS coordinate conventions. Phantom validation on an L1–L5 lumbar spine model demonstrated a target registration error of 0.62 ± 0.27 mm, end-to-end latency of 15.38 ms, and a rendering rate of 65 Hz performance within clinical benchmarks.

---

## Architecture

### Schematic

<p align="center">
  <img width="1600" height="789" alt="WhatsApp Image 2026-07-15 at 1 38 04 PM (2)" src="https://github.com/user-attachments/assets/e234eeec-5504-4527-9745-5830a6a05d44" />
</p>
This diagram presents the end-to-end TrackVision pipeline, from optical hardware tracking through data acquisition and transform composition to final multi-planar rendering. It illustrates how live 6-DOF pose streams from the Atracsys FusionTrack camera are relayed via PlusServer and OpenIGTLink into the transform engine. The color-coded blocks distinguish live tracker components, static pre-operative inputs, and protocol layers, giving a high-level view of the full navigation stack.

### 2D-2D Architecture

<p align="center">
  <img width="1600" height="1146" alt="WhatsApp Image 2026-07-15 at 1 38 03 PM" src="https://github.com/user-attachments/assets/76e819be-ff6e-41d0-aaa2-42bc4c397fec" />
</p>
This schematic outlines the SpineLite 2D–2D Transform Composition Engine used for fluoroscopy-based navigation. Live tooltip and PRM transforms are combined with static AP/LP projection matrices (P_AP, P_LP) to compute the instrument tooltip's pixel coordinates on each X-ray view. The output panel shows the resulting AP and LP overlay images with real-time crosshair projection of the tracked instrument.

<div align="center">
<table>
<tr>
<td width="580" align="center"><b>2D-2D Tracking</b></td>
</tr>
<tr>
<td width="580"><video src="https://github.com/user-attachments/assets/5881ba07-620c-4182-a97e-6e99429a0353" controls style="max-width:580px"></video></td>
</tr>
</table>
</div>

### 2D-3D Architecture

<p align="center">
  <img width="1600" height="1171" alt="WhatsApp Image 2026-07-15 at 1 38 06 PM" src="https://github.com/user-attachments/assets/4e08bbab-1041-4b10-afc3-bc0dd545600d" />
</p>
This schematic details the TrackVision 2D–3D Transform Composition Engine, tracing the pipeline from hardware tracking and OpenIGTLink data acquisition through to multi-planar CT rendering. It shows how live TooltipToCamera and PRMToCamera transforms combine with the static ReferenceToCT registration matrix to compute a smoothed PointerToCT pose. The right-hand panel shows the resulting output: synchronized axial/sagittal CT slices and a 3D volumetric render driven by this composed transform.

<div align="center">
<table>
<tr>
<td align="center"><b>2D-3D Tracking</b></td>
</tr>
<tr>
<td><video src="https://github.com/user-attachments/assets/59c2ec8e-85ac-41d2-9a7e-5faa50f53dca" controls width="380"></video></td>
</tr>
</table>
</div>

---

## Results

### 2D-2D Overlay

<p align="center">
  <img width="1518" height="1036" alt="WhatsApp Image 2026-07-15 at 1 38 06 PM (1)" src="https://github.com/user-attachments/assets/66a772ce-c30d-4e2c-bff3-1a19ac91b6c5" />
</p>
This screenshot shows the 2D/2D Registration interface with side-by-side AP and LP fluoroscopy views of the lumbar spine. A blue line marks the live tracked instrument trajectory, while the dashed yellow line indicates the planned/reference trajectory for comparison. This overlay allows the surgeon to visually confirm instrument alignment against the pre-planned path in real time during fluoroscopic guidance.

### 2D-3D Per-Vertebra Rendering

<p align="center">
<img width="1600" height="1104" alt="WhatsApp Image 2026-07-15 at 1 38 05 PM" src="https://github.com/user-attachments/assets/e6ed9e15-1caa-4570-a528-40a67c388a60" />
</p>
This panel shows the live 3D needle mesh rendered at its instantaneous tooltip-to-CT pose for each individual lumbar vertebra, L1 through L5. Each sub-view uses standard anatomical orientation labels (A/P, S/I, R) to orient the surgeon within the CT volume. The consistent blue trajectory line across all five vertebrae demonstrates stable tracking accuracy throughout the full lumbar range.

### Simultaneous Tracking of 2D-3D with TCP/IP

<p align="center">
<img width="1448" height="1086" alt="WhatsApp Image 2026-07-15 at 1 38 05 PM (1)" src="https://github.com/user-attachments/assets/c6fba5a2-1a19-4fde-b7de-203d0c04182c" />
</p>
This image demonstrates TrackVision's TCP/IP multi-screen output feature, where the live navigation view is streamed from the primary laptop to a secondary external display over a local network connection. Both screens mirror the same 2D–3D CT navigation panel, showing synchronized axial, coronal, and sagittal slice views. This setup enables a secondary viewer — such as a surgeon at the operating table — to monitor navigation independently of the primary workstation.

<div align="center">
<table>
<tr>
<td align="center"><b>Simultaneous Tracking</b></td>
</tr>
<tr>
<td><video src="https://github.com/user-attachments/assets/b7e92ee3-336e-43b5-800d-e30da6551432" controls width="380"></video></td>
</tr>
</table>
</div>

### Viewing Modes

<p align="center">
<img width="1600" height="916" alt="WhatsApp Image 2026-07-15 at 1 38 04 PM" src="https://github.com/user-attachments/assets/b3c5fde9-7cd4-4ee9-a92b-dbd782dc3a3c" />
</p>

This grid compares the three VolumeResliceDriver viewing modes — Scan-centric, Tool-centric, and Tool-dynamic — each validated across four instrument insertion angles (25°, 50°, 75°, and 90°). The blue line marks the live instrument trajectory relative to the lumbar vertebral anatomy in each slice. Together, the panels demonstrate how the CT slice plane behaves differently under each mode: staying anatomically fixed, following the tooltip, or rotating to stay perpendicular to the instrument axis.

<div align="center">
<table>
<tr>
<td align="center"><b>Tool_Centric</b></td>
<td align="center"><b>Scan_Centric</b></td>
<td align="center"><b>Tool_Dynamic</b></td>
</tr>
<tr>
<td><video src="https://github.com/user-attachments/assets/33439f56-d601-4e75-a628-0dbe57fc8839" controls width="380"></video></td>
<td><video src="https://github.com/user-attachments/assets/03abb5c2-0eae-4310-9800-34d0abd22624" controls width="380"></video></td>
<td><video src="https://github.com/user-attachments/assets/e62bb3dd-eed2-4112-b66d-50d98e7c2695" controls width="380"></video></td>
</tr>
</table>
</div>

---
### Comparative Performance Analysis

TrackVision is compared against Slicer, the closest open-source alternative:

| Comparison Parameters | Slicer | **TrackVision** |
|---|---|---|
| Nav. latency (ms) | ~50 | **15.38** |
| Rendering (FPS) | 20–30 | **65** |
| TRE — phantom (mm) | 2.3 ± 0.8 | **0.62 ± 0.27** |
| RMSE (mm) | N.R. | **0.67** |
| Multi-modal guidance | No | **Yes** |
| Auto server initialisation | No | **Yes** |
| Coordinate system flexibility | No | **Yes** |

*N.R. = not reported.*

---

## Prerequisites

### Hardware

| Item | Requirement |
|---|---|
| Computer | 64-bit PC (x86-64), Windows 10/11 |
| RAM | 8 GB minimum (Slicer-based; 16 GB recommended) |
| Graphics | Dedicated GPU with OpenGL 3.2+ support (for 3D rendering) |
| Storage | ~4 GB free for installation |
| Optical tracker | Atracsys fusionTrack optical tracking camera |
| Tracked tools | A passive-marker Pointer and a passive-marker Phantom/reference, each with its calibration geometry file (`.ini`) |
| Connection | Gigabit Ethernet link between the PC and the fusionTrack camera |

### Software

| Item | Requirement |
|---|---|
| Operating System | Windows 64-bit (win-amd64) |
| Atracsys fusionTrack runtime | Bundled inside the installer — no separate SDK or device-driver installation required. Ships under `...\lib\PlusServer\` (`fusionTrack64.dll`, `device64.dll`, and all supporting libraries) |
| PlusServer | Bundled inside the installer (self-contained) — no separate PLUS Toolkit installation needed |
| Network port | Port `18944` (TCP) free — PlusServer streams tracking data over OpenIGTLink on this port; allow it through the firewall |
| Permissions | Administrator rights on the machine (installs to `C:\Program Files\`) |

---

## Installation

### Step 1: Obtain the Installer
Copy the installer file `SpineTracker2-1.0.0-win-amd64.exe` to the target PC.

### Step 2: Run as Administrator
Right-click the installer and select **Run as administrator**. This is required, as the application installs to `C:\Program Files\`.

### Step 3: Accept the License Agreement
In the NSIS setup wizard, review and accept the license agreement to proceed.

### Step 4: Choose the Install Location
Keep the default installation folder `C:\Program Files\Spine Tracker 2.0 1.0.0`, or specify an alternate location, then click **Install**.

### Step 5: Complete the Installation
Click **Finish** once installation completes. A Start-menu and desktop shortcut named `SpineTracker2` will be created automatically.

> **Note:** No separate driver or SDK installation is required — the Atracsys fusionTrack runtime and PlusServer are both bundled inside the installer.

### Step 6: Connect the Hardware
Connect the Atracsys fusionTrack optical camera to the PC via Ethernet and power it on. Confirm the camera is reachable on the network with the correct IP/subnet configuration.

### Step 7: Launch the Application
Start `SpineTracker2` from the desktop or Start-menu shortcut.

### Step 8: Verify Automatic Initialization
On launch, the application automatically:
- Starts the internal PlusServer
- Loads the Pointer and Phantom calibration geometry files
- Begins streaming tracking transforms (`PointerToPhantom`, `PointerToTracker`, `PhantomToTracker`) over OpenIGTLink on port `18944`

### Step 9: Confirm System Status
Check the on-screen status indicators to confirm that **PlusServer is running** and the **tracker is connected**.

### Step 10: Proceed to Registration and Tracking
Once all status indicators are green, proceed with patient/phantom registration to begin navigation.

## Repository Structure
<details>
  
  ```
  TrackVision/
├── Media/
│   ├── Images/
│   │   ├── 2D-2D_Overlay.png
│   │   ├── 2d-2d_Architecture.png
│   │   ├── 2d-3d_Architecture.png
│   │   ├── IP.jpeg
│   │   ├── Per-Vertebra_3D_Rendering.png
│   │   ├── Schematic_Diagram.png
│   │   ├── Tracking_setup.jpeg
│   │   └── View_Modes.png
│   └── Videos/
│       ├── 2D-2D_Tracking.MOV
│       ├── 2D-3D_Tracking.MOV
│       ├── Scan_Centric_Mode.MOV
│       ├── Simultaneous_Tracking_2D_3D.mp4
│       ├── Tool_Centric_Mode.MOV
│       └── Tool_Dynamic_Mode.MOV
├── Packaged_Interface_Library/
│   └── TrackVision/
│       ├── bin/
│       ├── include/
│       ├── lib/
│       ├── libexec/SpineTracker2-5.11/
│       ├── share/
│       ├── TrackVision.exe
│       └── Uninstall.exe
├── TrackVision_App/
│   └── TrackVision-1.0.0-win-amd64.exe
├── .gitattributes
└── README.md
  ```
</details>
