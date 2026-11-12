# 🤖 Fanuc OLP Program Checker — Downloads & Releases

[![Release](https://img.shields.io/badge/Version-v2.52-blue.svg)](FanucOLPChecker.exe)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.7.2-512BD4?logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)

This repository hosts compiled, ready-to-run releases of the **Fanuc OLP Program Checker** Windows application.

---

## ⬇️ Download

👉 **[Download FanucOLPChecker.exe](https://github.com/crashingtardis/Fanuc-OLP-Checker-Release/raw/main/FanucOLPChecker.exe)**

*No installation required — simply download and run the `.exe`.*

---

## 📋 System Requirements
- Windows 10 or 11 (64-bit)
- .NET Framework 4.7.2 or higher (pre-installed on Windows 10/11)

---

## 🚀 Key Features
- **Simulation Download Checks**: Validates robot OLP packages (`tooldata.ls`, `armload.cm`, `ARMLD_V2.CM`, etc.).
- **Syntax & Coordinate Verification**: Checks reach limits, UTOOL selection, empty CALL/PAYLOAD statements, and inertia values.
- **GM Specific Rules**: Checks for `AT POUNCE` macro arguments, Robot ID vs. directory name matching, and required location comments in `td_path.ls` and `repair.ls`.
- **Automatic Updates**: Automatically notifies you when a newer version is released.
