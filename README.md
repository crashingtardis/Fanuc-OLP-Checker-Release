# 🤖 Fanuc OLP Program Checker

[![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.7.2-512BD4?logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![Release](https://img.shields.io/badge/Version-v2.52-blue.svg)]()
[![License: AGPL v3](https://img.shields.io/badge/License-AGPL_v3.0-blue.svg)](LICENSE.txt)

A high-performance Windows desktop application designed to validate and audit **FANUC robot offline programming (OLP)** packages and simulation download folders (`.LS`, `.CM`, etc.) before deployment to physical equipment.

---

## ⬇️ Download

👉 **[Download Latest FanucOLPChecker.exe](https://github.com/crashingtardis/Fanuc-OLP-Checker-Release/raw/main/FanucOLPChecker.exe)** *(v2.52 standalone executable — no installation required)*

---

## ✨ Features

### 🔍 General Program Checks
- 📁 **Missing Files**: Verifies that required robot system files (`tooldata.ls` and `armload.cm` or `ARMLD_V2.CM`) exist in each robot directory.
- 📄 **Empty Files**: Detects empty or corrupted files (< 1KB or containing no valid text content).
- ⚖️ **Inertia Value Errors**: Validates payload inertia parameters ($I_{xx}, I_{yy}, I_{zz}$) to ensure they fall within valid operational limits ($0 \le \text{inertia} \le 8000$).
- 📞 **Empty CALL Statements**: Detects blank or unfinished subroutine calls (`CALL ;`).
- 📦 **Empty PAYLOAD Statements**: Flags unassigned payload statements (`PAYLOAD[]`).
- 🔧 **Invalid UTOOL Selection**: Flags invalid tool definitions (`UTOOL_NUM = 0;`).
- 📏 **Invalid Location Coordinates (Customizable Reach Limit)**:
  - Scans Cartesian position data for extreme coordinates ($X, Y, Z$).
  - **Adjustable UI threshold**: Allows modifying the maximum allowable distance (defaults to `10,000` mm) directly in the UI to support robots on linear rails, gantries, or extended work cells.
- 📐 **Joint Coordinates**: Flags unexpected joint-coordinate motion points (`J2=`, `J3=`, etc.) in `.ls` programs (automatically excludes `tooldata.ls` and `DIAG1.ls`).
- ⚡ **Spot Weld GP2 Check (Reserved)**: Placeholder verification for auxiliary axis definitions with `SPOT[]` instructions.

### 🏭 GM-Specific Checks
- 🎯 **Invalid AT POUNCE Macro Usage**: Verifies `AT POUNCE` macro invocations for missing required `SO` signal arguments.
- 🏷️ **Robot ID vs. Folder Name**: Validates that robot IDs declared within program headers/comments match the parent robot directory name.
- 📍 **td_path.ls Location Comments**: Verifies that `td_path.ls` contains all four required position labels — `READY`, `AIM`, `FIRE`, and `VERIFY` — in the form `P[N:LABEL]`. Reports one error per missing label.
- 🔧 **repair.ls Location Comment**: Verifies that `repair.ls` contains a `REPAIR` position label in the form `P[N:REPAIR]`. All label matching is case-insensitive.

### 🛡️ Filtering & Exclusions
- 🗂️ **Subfolder Filtering**: Automatically skips archive, backup, and historical folders (`Archive`, `Obsolete`, `Old`, `History`, `Obs`, or folders prefixed with `_`).
- 📅 **Date Filtering**: Allows restricting parsing to files modified within the last $N$ days (configurable).

### 💻 User Interface & Productivity
- ⚡ **Asynchronous Multithreading**: Scans run in a background worker task to keep the UI completely responsive during large scans.
- 🌓 **Dynamic Dark / Light Themes**: Switch between dark mode and light mode with a single click.
- 💾 **Persistent Settings**: Automatically saves and restores window size, maximized state, splitter position, dark mode preference, and customized coordinate reach threshold between sessions.
- 🖱️ **Double-Click File Navigation**: Double-clicking an error row in the results grid immediately opens the file in the default text editor, or opens its folder in Windows Explorer.
- 📊 **CSV Export**: Export all discovered errors with timestamps, folder, file name, error type, and detailed context to CSV.
- 🕒 **Folder History**: Remembers recently scanned directories in a dropdown selector for fast re-auditing.
- 🔄 **GitHub Version & Update Checker**: Checks the GitHub repository on launch (and on-demand via the `↻` button) for newer versions, displaying an update notice with a direct link to GitHub.

---

## ⚙️ Requirements & Building

### 📦 Prerequisites
- Windows 10 / 11
- .NET Framework 4.7.2 or higher
- Visual Studio 2019/2022 or MSBuild (for compiling from source)

### 🔨 Compiling from Source
To build the application with MSBuild:

```powershell
MSBuild.exe FanucOLPChecker.csproj /p:Configuration=Release
```

The compiled standalone executable will be generated at:
```
bin\Release\FanucOLPChecker.exe
```

---

## 📜 Revision Log

### 🔍 v2.52
- 📍 **td_path.ls Location Comment Check**: Added a new GM-specific check that verifies `td_path.ls` contains position labels for all four required stages — `READY`, `AIM`, `FIRE`, and `VERIFY` — in the form `P[N:LABEL]`. Reports one error per missing label (case-insensitive, matches both `/MN` and `/POS` section formats).
- 🔧 **repair.ls Location Comment Check**: Added a new GM-specific check that verifies `repair.ls` contains a `REPAIR` position label in the form `P[N:REPAIR]` (case-insensitive).
- 🖥️ **UI Updates**: Added two new checkboxes to the GM Specific Checks section with **Check All / Uncheck All** support. Exclusions & Filters section shifted down to accommodate.
- 🔄 **GitHub Version & Update Checker**: Added automated background version check on launch (and manual on-demand check via `↻` button) comparing against `version.txt` in the GitHub repository. Notifies users with a clickable link to open the repository.

### 🚀 v2.51
- 📏 **Configurable Coordinate Distance in UI**:
  - Replaced static 10,000mm check with an interactive text box: `(X,Y,Z > [ 10,000 ] mm)` to support robots mounted on linear transfer rails, overhead gantries, or large-envelope stations.
  - Added persistence for the reach distance limit in `FanucCheckerSettings.txt`.
  - Added culture-invariant decimal parsing with fallback for numerical coordinate reading.
  - Dynamically formatted error reporting and CSV exports reflecting the selected coordinate limit.
- 🛠️ **Project Structure**:
  - Updated `FanucOLPChecker.csproj` compile target to `FanucOLPChecker.cs`.

### 💾 v2.50
- 💾 **Persistent Settings**:
  - Saved and restored window dimensions, window state, splitter distance, and theme preference to `FanucCheckerSettings.txt`.

### ⚡ v2.00
- 🖥️ **C# Native Migration**:
  - Full rewrite from PowerShell/PS2EXE into a native C# Windows Forms application (.NET Framework 4.7.2).
  - Modernized two-panel layout with `SplitContainer` featuring controls on the left and a searchable results grid with live console logging on the right.
  - Multithreaded scanning using async/await and `Task.Run` to eliminate UI freezing.
  - Added Dark / Light theme engine.
  - Added double-click event handling to quickly open error files or containing directories.
  - Embedded application icon and FANUC Robotics logo resources directly into the assembly.

---

### 📦 Desktop Edition Releases (PowerShell Era)

#### 🏷️ v1.44
- **UPDATED**: Added `DIAG1.ls` to the exclusion list for the Joint Position check.

#### 🏷️ v1.43 - UI Update
- Rebalanced the 9 general checkboxes across the two columns.

#### 🏷️ v1.42
- **MOVED**: Relocated SPOT GP2/J1 check to the general validation section.

#### 🏷️ v1.41
- **ADDED**: GP2/J1 definition check for files containing `SPOT[]` commands.

#### 🏷️ v1.40 - Stability & Compatibility Update
- Added direct folder scan support when no subdirectories exist.
- Fixed `.ls` extension check to avoid false matches on `.xls` / `.xlsx`.
- Invariant culture parsing for inertia and reach numbers (locale-safe).
- Added line numbers to CALL, PAYLOAD, UTOOL, and Reach error reports.
- Fixed subfolder false positives for `tooldata.ls` and `armload.cm`.
- Added `SaveFileDialog` for CSV export with desktop fallback.
- Added validation for date filter and live `ComboBox` history update.

#### 🏷️ v1.39
- **ADDED**: Joint Coordinates check (`J2-J6`) in `.ls` files (excluding `tooldata.ls`).

#### 🏷️ v1.38
- **ADDED**: GM Robot ID vs Folder Name mismatch check.

#### 🏷️ v1.37
- **ADDED**: Check All and Uncheck All buttons for Exclusions.

#### 🏷️ v1.36
- **ADDED**: Check All and Uncheck All quick toggle buttons.

#### 🏷️ v1.35
- **UPDATED**: Exclusions checkbox is now checked by default.

#### 🏷️ v1.31
- **UPDATED**: Missing file check now explicitly ignores folders named `DCS`.

#### 🏷️ v1.24
- Added `UTOOL_NUM = 0` invalidation check and combined file read loops for performance.

#### 🏷️ v1.23
- **FIXED**: Missing file logic groups by specific robot folder to avoid Zone-level false passes.

#### 🏷️ v1.22
- Added file counts to console.

#### 🏷️ v1.21
- Recursive missing file search.

#### 🏷️ v1.20
- Switched from Google Sheets version to standalone `.exe`.

---

### 🌐 Google Sheets & Cloud Edition (Legacy)

#### 🏷️ v1.16
- Upgraded memory bank to resolve and display human-readable Drive paths.

#### 🏷️ v1.15
- Added delta scanning (Date Filter) to skip unmodified files.

#### 🏷️ v1.14
- Added UserProperties to save and retrieve recently checked folders.

#### 🏷️ v1.09 – v1.13
- Upgraded Empty check, formatting fixes, auto-open, and URL parsing.

#### 🏷️ v1.08
- Added CacheService integration for an IMMEDIATE kill switch.

#### 🏷️ v1.00 – v1.07
- Initial setup, UI polish, batch writing, client-side arch, and inertia limits.

---

## 📄 License
This project is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. Refer to the [LICENSE.txt](LICENSE.txt) file in this repository for full license details.
