

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

Minimally Invasive Spine Surgery (MISS) procedures — pedicle screw placement, vertebroplasty, facet-joint injection, and percutaneous biopsy — demand sub-millimetre targeting precision within an anatomically constrained operative field. While 3D Slicer offers powerful image-guided tracking functionality, it is limited by a fixed coordinate system, tool-centric-only viewing, and its monolithic architecture that prevents standalone, reusable deployment. **TrackVision** is an open-source navigation platform that repackages Slicer3D's image-guided tool tracking (IGTT) capabilities into an independent, standalone application built on the SlicerCAT (Slicer Custom Application Template) using Qt, CMake, and VTK. It integrates seamlessly with the existing open-source IGT ecosystem — PLUS Toolkit, SlicerIGT, IGSIO, and OpenIGTLink — with PLUS Toolkit launched automatically as an internally supervised subprocess, eliminating manual server management. TrackVision supports three concurrent guidance modalities (2D–2D fluoroscopy overlay, 2D–3D multi-planar CT reslicing, and 3D volumetric navigation) and dynamically switches between VTK, RAS, and LPS coordinate conventions. Phantom validation on an L1–L5 lumbar spine model demonstrated a target registration error of 0.62 ± 0.27 mm, end-to-end latency of 15.38 ms, and a rendering rate of 65 Hz — performance within clinical benchmarks.

---

## Architecture

<!-- Architecture Image 1 -->
<p align="center">
  <img width="1600" height="789" alt="WhatsApp Image 2026-07-15 at 1 38 04 PM (2)" src="https://github.com/user-attachments/assets/e234eeec-5504-4527-9745-5830a6a05d44" />

</p>

**Intraoperative Hardware Configuration**
The Atracsys FusionTrack 500 optical camera simultaneously tracks three passive retro-reflective IR marker sets — the C-arm-mounted calibration drum, the surgical instrument tooltip, and the patient reference marker (PRM). The instrument-to-patient spatial relationship is derived online from these live 6-DOF streams.


<p align="center">
  <img width="1600" height="1146" alt="WhatsApp Image 2026-07-15 at 1 38 03 PM" src="https://github.com/user-attachments/assets/76e819be-ff6e-41d0-aaa2-42bc4c397fec" />
</p>

**2D–2D Transform Composition Pipeline**
Live tool and PRM transforms compose to yield the tooltip-to-PRM transform, which is then projected onto pre-acquired AP and LP C-arm fluoroscopy images using static calibrated projection matrices — enabling real-time crosshair updates at 65 Hz with no additional radiation.

<!-- Architecture Image 3 -->
<p align="center">
  <img width="1600" height="1171" alt="WhatsApp Image 2026-07-15 at 1 38 06 PM" src="https://github.com/user-attachments/assets/4e08bbab-1041-4b10-afc3-bc0dd545600d" />

</p>

**2D–3D Transform Composition Pipeline**
The live instrument tooltip is mapped into the CT volume frame via a static, pre-operatively derived registration matrix, driving multi-planar CT reslicing and a real-time 3D needle mesh render, with pose smoothing applied via an EMA stabiliser.

---

## Results

<p align="center">
  <img width="1518" height="1036" alt="WhatsApp Image 2026-07-15 at 1 38 06 PM (1)" src="https://github.com/user-attachments/assets/66a772ce-c30d-4e2c-bff3-1a19ac91b6c5" />
</p>

<p align="center">
<img width="1600" height="1104" alt="WhatsApp Image 2026-07-15 at 1 38 05 PM" src="https://github.com/user-attachments/assets/e6ed9e15-1caa-4570-a528-40a67c388a60" />
</p>

<p align="center">
<img width="1448" height="1086" alt="WhatsApp Image 2026-07-15 at 1 38 05 PM (1)" src="https://github.com/user-attachments/assets/c6fba5a2-1a19-4fde-b7de-203d0c04182c" />
</p>

<p align="center">
<img width="1600" height="916" alt="WhatsApp Image 2026-07-15 at 1 38 04 PM" src="https://github.com/user-attachments/assets/b3c5fde9-7cd4-4ee9-a92b-dbd782dc3a3c" />
</p>

**Tool-Centric Mode**
CT slice plane is centred on and oriented along the instrument axis, showing a cross-section of the anatomy exactly at the tooltip location and trajectory angle, validated across insertion angles of 25°, 50°, 75°, and 90°.

**Scan-Centric Mode**
CT slice plane retains the standard anatomical orientation (axial, coronal, or sagittal) while translating to follow the instrument tooltip position, preserving anatomical context as the instrument advances.

**Tool-Dynamic Mode**
CT slice plane both translates with the instrument tooltip and rotates to remain perpendicular to the instrument axis, offering continuous cross-sectional guidance throughout instrument advancement.

<p align="center">
  <img src="path/to/results_phantom_validation.png" alt="Phantom Validation Setup" width="700"/>
</p>

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

### Getting the software onto the machine

1. Copy the installer `SpineTracker2-1.0.0-win-amd64.exe` to the PC.
2. Right-click the installer → **Run as administrator**.
3. In the NSIS setup wizard, accept the license agreement.
4. Keep the default install folder `C:\Program Files\Spine Tracker 2.0 1.0.0` (or choose another), then click **Install**.
5. Click **Finish** — a Start-menu / desktop shortcut `SpineTracker2` is created.

> No driver/SDK step needed — the fusionTrack runtime and PlusServer are bundled in the installer.

### Setup (after installation, before use)

1. Connect the fusionTrack camera to the PC over Ethernet and power it on.
2. Confirm the camera is reachable on the network (correct IP / subnet).
3. Launch `SpineTracker2` from the desktop / Start-menu shortcut.
4. On startup, the app automatically starts the internal PlusServer, loads the Pointer and Phantom geometry files, and begins streaming tracking transforms (`PointerToPhantom`, `PointerToTracker`, `PhantomToTracker`) on port `18944`.
5. Verify the status indicators show **PlusServer running** and **tracker connected**.
6. Proceed to registration / tracking.

---

<p align="center">
  <sub>Operational documentation and updates are available on this repository.</sub>
</p>
