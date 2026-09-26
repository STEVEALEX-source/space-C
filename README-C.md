# Field Notes from the Outer Dark — Native C / SDL2 Edition

A native C recreation of [STEVEALEX-source/space](https://github.com/STEVEALEX-source/space), an interactive solar-system visual essay. This edition includes the original 20-entry catalogue—eight planets, five dwarf planets, the Milky Way, and six nearby galaxies—with chapter titles, field-note prose, epigraphs, astronomy data, and moon details.

The browser's Three.js renderer has been replaced by an SDL2 2D/2.5D renderer with an animated starfield, orbit paths, asteroid belt, moonlets, cached procedural planet surfaces with spherical lighting, Saturn's rings, galaxy scenes, four category views, navigation, and a chapter/data panel. This is a stylized adaptation rather than a pixel-identical or full 3D replacement.

## Requirements

- C11 compiler (GCC or Clang)
- SDL2 and SDL2_ttf development libraries
- `pkg-config`
- DejaVu Serif and Sans Mono fonts plus Liberation Serif Italic

On Debian/Ubuntu:

```sh
sudo apt install build-essential pkg-config libsdl2-dev libsdl2-ttf-dev fonts-dejavu-core fonts-liberation
```

## Build and run

```sh
make
./space
```

Or use `make run`. To run the headless smoke test:

```sh
make test
```

The smoke test renders all 20 entries in all four modes and writes a representative frame to `/tmp/space-c-smoke.bmp`.

## Controls

- Click **Begin transmission** to enter.
- Click **Planets / Dwarfs / Milky Way / Galaxies**, or use keys **1–4**.
- Click a body or a bottom navigation dot to open its chapter.
- Use the arrow buttons or **Left / Right** (Space advances) to browse.
- Drag to rotate; scroll over the scene to zoom.
- Scroll inside the essay panel to read longer entries; click **×** to close it.
- **Esc** closes the panel, returns to the intro, or quits.

## Files

- `main.c` — rendering, interface, input handling, and app loop.
- `bodies.h` — complete body, essay, metrics, and moon catalogue.
- `Makefile` — build, run, and test targets.

## Browser edition

Install [Emscripten](https://emscripten.org/docs/getting_started/downloads.html), then build and serve the WebAssembly edition:

```sh
make web
python3 -m http.server 8000 --directory web
```

Open `http://localhost:8000`. The GitHub Pages workflow publishes this browser build automatically on pushes to `main`; see [DEPLOY-GITHUB-PAGES.md](DEPLOY-GITHUB-PAGES.md).
