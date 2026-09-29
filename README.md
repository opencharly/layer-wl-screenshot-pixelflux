# layer-wl-screenshot-pixelflux

Desktop screenshot capture for the OpenCharly selkies streaming desktop, via the
pixelflux pipeline.

The `wl-screenshot-pixelflux` candy installs the `pixelflux-screenshot` wrapper
script under `~/.local/bin`, which drives the selkies pixelflux capture pipeline
to grab a desktop frame. It connects to the in-process capture bridge at
`/tmp/charly-capture.sock` and decodes H.264 frames to PNG via ffmpeg. It is the
preferred `wl: screenshot` path on `selkies-desktop`.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `wl-screenshot-pixelflux` |
| Script | `~/.local/bin/pixelflux-screenshot` (mode `0755`) |
| Capture | `/tmp/charly-capture.sock` (selkies WebSocket bridge) |
| Requires | `pod-selkies` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list — typically
transitively through the `selkies-desktop` metalayer:

```yaml
my-desktop-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-wl-screenshot-pixelflux:v2026.247.1546'
```

Then, inside the desktop session:

```bash
pixelflux-screenshot > shot.png    # capture a frame to PNG
pixelflux-screenshot --status      # connection / frame state as JSON
```

The `wl: screenshot` method auto-detects `pixelflux-screenshot` (preferred over
grim) when it is available.

The candy's `plan:` asserts the wrapper is installed and executable at the
expected path.

## Layout

- `charly.yml` — the `wl-screenshot-pixelflux:` candy entity (the `require:` on
  `pod-selkies`, the `copy:` plan steps, the `check:` assertion) and the embedded
  `wl-screenshot-pixelflux-skill:` skill entity.
- `pixelflux-screenshot` — the Python wrapper copied to `~/.local/bin/`.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:wl-screenshot-pixelflux`
- `/charly-check:wl` — the `wl: screenshot` method that auto-detects pixelflux-screenshot
- `/charly-selkies:wl-record-pixelflux` — recording companion (same capture bridge + singleton)
- `/charly-selkies:wl-screenshot-grim` — alternative for sway-desktop (`wlr-screencopy`)
- `/charly-selkies:selkies` — parent candy providing the `ScreenCapture` singleton and capture bridge
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
