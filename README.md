*Read this in other languages: [English](README.md) | [Español](README-es.md)*
<p align="center">
  <img src="https://img.shields.io/github/actions/workflow/status/pablobp10/nuclear-android/android.yml?branch=main&label=Build%20Status&logo=githubactions&logoColor=white" alt="Build Status">
  <img src="https://img.shields.io/github/license/pablobp10/nuclear-android?color=blue&logo=open-source-initiative&logoColor=white" alt="License">
  
  <img src="https://img.shields.io/github/v/release/pablobp10/nuclear-android?include_prereleases&sort=semver&label=Latest%20APK&logo=android&logoColor=white&color=success" alt="Latest Release">
  <img src="https://img.shields.io/github/downloads/pablobp10/nuclear-android/total?color=blueviolet&logo=github&label=Downloads" alt="Total Downloads">

  <img src="https://img.shields.io/badge/Platform-Android%20%7C%20aarch64-3DDC84?logo=android&logoColor=white" alt="Platform">
  <img src="https://img.shields.io/github/last-commit/pablobp10/nuclear-android?logo=git&logoColor=white&label=Last%20Update" alt="Last Commit">
</p>

<p align="center">
  <picture>
    <!-- Usa aquí la ruta a tu icono generado o vectorial seguro -->
    <img alt="Nuclear CE Logo" src="android_ui/packages/player/public/logo-icon-on-white.png" width="200" style="border-radius: 20%;">
  </picture>
</p>
<div align="center">
# Nuclear CE (Community Edition)
### *Unofficial Android Mobile Port*
</div>
<div align="center">
  **Nuclear CE** is a free, open-source music player without ads or tracking, strictly engineered to provide native **Android (aarch64)** compatibility and a custom touch-optimized mobile UI.<br>
  Search for any song or artist, build playlists, and start listening on the go.
  
</div>

## ⚠️ Legal Disclaimer & Trademark Notice
**THIS PROJECT IS 100% UNOFFICIAL.** It is **not** affiliated with, endorsed by, or supported by the original Nuclear developers. 
* The "Nuclear" name is utilized strictly under **Nominative Fair Use** to indicate the software's origin and backend engine. "CE" stands for Community Edition.
* To respect original graphical trademarks, all official copyrighted logos have been programmatically replaced with generic safe-harbor icons during our   automated CI/CD build process.

## 📱 Mobile Architecture Overhaul

Porting a desktop-first React/Tauri application to a native Android environment requires aggressive DOM manipulation and backend patching. This fork introduces the **V18 Mobile Architecture**, a comprehensive set of CSS injections and Rust overrides designed to bypass upstream desktop constraints without altering the core React component tree.

### 1. Advanced DOM Restructuring & Radix UI Overrides
The original desktop app relies heavily on Radix UI for its modal and dialog management, which behaves unpredictably on mobile screens. We implemented a strict floating-card paradigm:
* **95% Floating Modals:** Dialogs (like Settings) are forcibly detached from their native Radix wrappers using `position: absolute` and `transform: translate(-50%, -50%)`. They are mathematically constrained to `95vw` width and `75vh` height, featuring rounded borders for a native Android UI feel.
* **The 2000px Backdrop Hack:** To fix Radix UI's broken background layering on mobile resolutions, we inject a massive `box-shadow: 0 0 0 2000px rgba(0, 0, 0, 0.6)` directly into the floating dialog container. This guarantees a perfect modal blackout effect across any screen size without relying on brittle React state changes.

### 2. Z-Index & Stacking Context Hierarchy
Mobile screens require strict overlapping rules to prevent UI collisions. The V18 patch enforces a rigid, conflict-free Z-Index queue:
* **`z-index: 10` (Floating Controls):** The QR and Theme switches are detached from the original desktop top bar and pinned to the bottom-left, floating above the main workspace but remaining completely subordinate to system modals.
* **`z-index: 40` (The Queue Panel):** The desktop right-sidebar queue is transformed into a bottom-up sliding panel. Its toggle button is surgically relocated to the bottom player bar (speaker zone) and rotated `-90deg` to indicate vertical expansion.
* **`z-index: 50+` (Modals & Dialogs):** Settings and search overlays remain at the top of the visual stack, ensuring no floating buttons pierce through the dark backdrops.

### 3. Touch Optimization & Notch Safe-Areas
* **Eradication of the Top Bar:** The desktop window frame (`<header data-tauri-drag-region>`) is aggressively collapsed to `height: 0` to reclaim vital vertical screen real estate.
* **Notch Compatibility:** Dynamic CSS using `env(safe-area-inset-top)` is applied to the workspace wrapper to prevent the user interface from bleeding into hardware camera cutouts.
* **Anti-Deformation Constraints:** Deep CSS nesting rules (`flex-shrink: 0`, `white-space: nowrap`) strictly prohibit text wrapping inside interactive elements and prevent the settings navigation sidebar from being crushed on narrow portrait displays.

### 4. Backend Cross-Compilation Engine (`patch.cjs`)
The original Rust backend is not configured for `aarch64-linux-android`. Our CI/CD pipeline injects a surgical script (`patch.cjs`) before compilation to aggressively mutate the codebase:
* **Desktop Plugin Stripping:** Automatically parses and removes PC-only Tauri plugins (`tauri-plugin-updater`, `tauri-plugin-window-state`, `tauri-plugin-process`) from `Cargo.toml` and `lib.rs` that would cause Android build failures (Exec format errors).
* **TLS & Network Patches:** Rewrites `reqwest` dependencies to force the use of `rustls-tls-webpki-roots`, resolving native Android SSL certificate validation issues on ARM architectures.
* **System Capabilities:** Injects an `android-almighty.json` capability file to grant the frontend necessary filesystem (`fs:*`) and window management permissions within the isolated Android Sandbox.

  
## 📥 Download
Grab the latest compiled Android artifacts from the [Releases page](../../releases).


| Platform | Formats | Architecture |
| :--- | :--- | :--- |
| Android | `.apk` | `aarch64` (ARM64) |

*Note: All APKs are automatically generated, signed, and optimized via our GitHub Actions pipeline.*

## ✨ Features
- **Mobile First:** Entirely restyled UI for Android environments.
- **Universal Streaming:** Search for music and stream it from any source.
- **Deep Exploration:** Browse artist pages with biographies, discographies, and similar artists.
- **Queue Management:** Shuffle, repeat, and drag-and-drop reordering optimized for touch screens.
- **Library Sync:** Favorites (albums, artists, and tracks) and Playlists.
- **Plugin Ecosystem:** Powerful plugin system with a built-in plugin store for metadata and streaming sources.
- **Customization:** Built-in themes optimized for OLED mobile displays.

## 🧩 Plugins & MCP
Nuclear CE inherits the powerful plugin system from the core project! Every functionality is driven by plugins. Plugins can provide streaming sources, metadata, playlists, and dashboard content.
* **Plugin Store:** Browse and install plugins directly from the mobile app.
* **MCP Integration:** The Model Context Protocol (MCP) server allows AI agents to drive the player locally.

## 🛠️ Development & Compilation
Nuclear CE is a pnpm monorepo managed with Turborepo. The main app is built with Tauri (Rust + React). This fork includes rigorous backend patches (`patch.cjs`) to enable `aarch64-linux-android` cross-compilation via the Android NDK.

### Prerequisites for Local Builds
- Node.js >= 22
- pnpm >= 9
- Rust (stable) with `aarch64-linux-android` target
- Android Studio / Android NDK & SDK toolchains

### Automated CI/CD (GitHub Actions)
We utilize a defensive GitHub Actions pipeline (`android.yml`) to compile the app without manual intervention. The pipeline automatically:
1. Strips incompatible desktop plugins (`tauri-plugin-updater`, etc.) from `Cargo.toml`.
2. Injects the touch-optimized UI.
3. Generates the generic CE branding.
4. Compiles the `.apk` using Tauri Android.
   
### Manual Build Instructions
```bash
git clone [https://github.com/YOUR_USERNAME/nuclear-android.git](https://github.com/YOUR_USERNAME/nuclear-android.git)
cd nuclear-android
pnpm install
# 1. Apply Android specific cross-compilation patches
node patch.cjs
# 2. Build frontend and Android APK
cd packages/player
pnpm run build:frontend
npx tauri android build --apk --target aarch64
