# Microsoft Edge & WebView2 Deployer & Toolkit for Windows 7 (x64)

> [English] | [Русский](README_RU.md)

An interactive automation tool for system-level deployment, WebView2 Runtime registration, and complete clean uninstallation of **Microsoft Edge** (both official final build 109 and modern builds **110–153+**) on **Windows 7 SP1 x64**.

---

## 📌 The Problem & Purpose

Installing and maintaining modern Edge or WebView2 on Windows 7 poses several technical challenges:

1. **Standalone Installers Refuse to Launch:**  
   Official Microsoft standalone packages (`microsoftedgestandaloneinstallerx64_*.exe`) block direct execution on Windows 7, even when extended kernels or compatibility layers are active.
2. **Forced User-Level Installation:**  
   Manual extraction often leads to Edge installing exclusively into the user's profile (`%LOCALAPPDATA%`). This toolkit forces a true system-wide installation into `C:\Program Files (x86)\Microsoft\Edge` using the `--system-level` switch.
3. **Setup Crash or Hang at 100%:**  
   During modern build deployment on Windows 7, `setup.exe` completes file extraction but crashes or hangs indefinitely attempting to register Windows 10-specific telemetry and update services. This script includes an automated watchdog to detect the hang and safely proceed.
4. **Broken WebView2 Runtime Registration:**  
   Third-party applications relying on WebView2 fail to detect the runtime. The toolkit constructs the proper `EdgeCore` folder structure, creates directory junctions, and injects complete registry hives to register the runtime globally.
5. **Standard Uninstall Failure:**  
   Once installed at the system level via extended compatibility layers, Edge cannot be removed cleanly via Windows "Programs and Features". A dedicated, clean removal engine is integrated.

---

## 🚀 Key Features

* **Two-Stage 7-Zip Extraction:** Bypasses outer wrapper restrictions by extracting internal `setup.exe` binaries and `MSEDGE.PACKED.7Z` directly from official offline installers.
* **Dynamic Compatibility Runner Modes:**
  * **Native Mode:** Pure Windows 7 environment for official build 109.x (all hooks disabled).
  * **VxKex Mode:** Configures Application Verifier hooks for builds 110–153+.
  * **Second System Mode:** Injects `ntdlx.dll` using the `DefaultConfig` profile (Win10 22H2 spoofing).
  * **Deadlock Prevention:** Automatically disables conflicting hooks prior to running `setup.exe`.
* **Hang Watchdog & Recovery:** Interactive timeout prompt allowing you to kill stalled setup processes without losing extracted binaries.
* **WebView2 Runtime Registry Injector:** Creates `msedgewebview2.exe` fallbacks, sets up `EdgeCore` junctions, and imports `.REG` keys while suppressing "Unsupported OS" notifications.
* **Deep Clean Uninstaller:**
  * Force-terminates all active Edge, WebView2, and updater processes.
  * Stops and deletes services (`edgeupdate`, `MicrosoftEdgeElevationService`) and scheduled tasks.
  * Removes `Program Files (x86)`, `ProgramData`, and `HKLM` registry entries.
  * Optional user profile wipe (clears browsing history, cookies, and `HKCU` keys).
* **Orphaned Process Killer:** One-click utility to terminate lingering background browser processes.

---

## 🛠 Prerequisites

* **Target OS:** Windows 7 SP1 (x64) with Administrator privileges.
* **Console Font:** **Consolas** or **Lucida Console** (set in `cmd.exe` properties).
* **Archiver:** **7-Zip** (`7z.exe` / `7za.exe`) installed or placed in the script directory.
* **For Builds 110+:** **VxKex** or **Second System (SS)** installed.
* **Source Installer:** Official Microsoft Edge standalone installer (`.exe`) placed in the script folder or subfolder.

---

## 📖 Quick Start

1. Place your downloaded Edge standalone installer x64 in the toolkit directory.
2. Run `Edge_Win7_Toolkit_EN.bat` as Administrator.
3. Select option `[1]` to install:
   * The script scans and unpacks the installer.
   * Choose your runner mode (Native 109, VxKex, or Second System).
   * Edge will deploy silently to `C:\Program Files (x86)\`.
4. Run option `[3]` to register and fix the WebView2 Runtime.
