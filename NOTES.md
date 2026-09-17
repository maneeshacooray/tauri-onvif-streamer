# Project Notes

Context for picking this up in a fresh session — this app is unrelated to `stream-debugger`
(an Expo/HLS playback debugger) even though it originated inside that repo.

## Origin

Extracted from `stream-debugger`'s `feat/tauri-onvif-streamer` branch (never merged to that
repo's `main`), which was authored by Jarrod Philips. History was preserved with
`git subtree split -P tauri-app`, so this repo's first commit (`5d78bd2`) keeps the original
author, date, and message. The branch was then deleted from `stream-debugger`'s remote once
confirmed preserved here.

Released as `v0.1.0` (tag + GitHub release), matching the version already in `package.json` /
`Cargo.toml`. `README.md` was rewritten from the default Vite/Tauri scaffold to actually
describe the app: an ONVIF camera discovery/control desktop app (device info, streaming,
video encoder config, PTZ, OSD).

## Known issue: `onvif-discover` dependency path

`src-tauri/Cargo.toml` currently has:

```toml
onvif-discover = { version = "0.1.0", path = "../../84-horcery-ip-cam-discovery-rust/onvif-discover" }
```

That path pointed at a private company repo on disk (`/Users/maneesha/WorkDir/Atlas/...`) and
won't exist on any other clone. That crate has since been extracted into its own repo:
**https://github.com/maneeshacooray/ip-cam-discover** (see its own `NOTES.md`).

`onvif-discover` there now exposes a real library API — `Credentials`, `get_device_information`,
`normalize_stable_id`, `continuous_move`, `stop` — matching exactly what
`src-tauri/src/commands.rs` imports from `onvif_discover::`. Verified it compiles standalone
with `cargo build -p onvif-discover --lib`.

**Not yet done:** repoint this `Cargo.toml` dependency at the new repo (e.g. as a git
dependency) instead of the stale local path, then do a full `cargo build` of `tauri-app` to
confirm the whole thing links.
