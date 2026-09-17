# Tauri ONVIF Streamer

A desktop app (Tauri + React + Rust) for discovering, viewing, and controlling ONVIF-compliant IP cameras on the local network.

## Features

- **Discovery** — find ONVIF cameras on the LAN via WS-Discovery.
- **Device info** — manufacturer, model, firmware version, serial number, and hardware ID.
- **Streaming** — fetch RTSP stream and snapshot URIs per media profile, preview snapshots, and open live streams in [mpv](https://mpv.io/).
- **Video encoder config** — read and update resolution, bitrate, quality, FPS, and GOP length.
- **PTZ control** — continuous and absolute pan/tilt/zoom moves, stop, live status, and hardware preset positions (list, save, go to, remove).
- **On-screen display** — enable/disable OSD and set custom OSD text.
- Per-device credentials for authenticated ONVIF/SOAP calls.

## Tech Stack

- [Tauri 2](https://tauri.app/) (Rust backend)
- React 19 + TypeScript + Vite
- [onvif-rs](https://github.com/lumeohq/onvif-rs) for ONVIF/SOAP (device management, media, PTZ)
- Tokio async runtime, reqwest for HTTP

## Prerequisites

- [Node.js](https://nodejs.org/) and npm
- [Rust](https://www.rust-lang.org/tools/install) toolchain
- [Tauri prerequisites](https://tauri.app/start/prerequisites/) for your OS
- [mpv](https://mpv.io/) installed and on `PATH` (used to open live streams)

> **Note:** `src-tauri/Cargo.toml` currently points `onvif-discover` at a local sibling path
> (`../../84-horcery-ip-cam-discovery-rust/onvif-discover`). That crate isn't part of this repo,
> so a fresh clone will need that dependency vendored, published, or repointed (e.g. to a git
> source) before `cargo build` will succeed.

## Getting Started

```bash
npm install
npm run tauri dev
```

## Build

```bash
npm run tauri build
```

## Project Layout

- `src/` — React frontend (`App.tsx` holds the main UI)
- `src-tauri/src/commands.rs` — Tauri commands exposing ONVIF operations to the frontend
- `src-tauri/src/lib.rs` — app setup and command registration
