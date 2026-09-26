# space

A personal visual essay that travels from the planets of our solar system to nearby galaxies. The repository preserves the original Three.js edition and adds a native C / SDL2 recreation that also runs in a browser as WebAssembly.

GitHub Pages publishes the C / SDL2 browser build from `web/`. It uses a stylized 2D/2.5D renderer rather than the original Three.js 3D renderer; the original `index.html`, `main.js`, and `styles.css` remain in the repository.

---

## Live Demo

[View the project](https://stevealex-source.github.io/space/)  

---

## Original Three.js edition features

- Cinematic intro screen
- Smooth camera controls (drag to orbit, scroll to zoom)
- Click any planet to open its chapter
- Procedural surface textures for all 8 planets
- Moons with independent orbits
- Saturn’s rings
- Asteroid belt between Mars and Jupiter
- Soft atmospheric glow on Earth & Venus
- Written field notes + data for every planet

---

## How to Run Locally

1. Download or clone the repository
2. Open a terminal in the project folder
3. Run:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000` in a browser. For the C / SDL2 browser build, use the `web/` directory as described below. For the native desktop edition, follow the build steps below.

---

## Browser edition (WebAssembly)

The C application can run in a browser through Emscripten/WebAssembly. It is a canvas-based SDL2 edition, not a pixel-identical port of the original Three.js renderer.

1. Install and activate [Emscripten](https://emscripten.org/docs/getting_started/downloads.html).
2. Build the browser files:

   ```sh
   make web
   ```

3. Serve them over HTTP (do not open the generated HTML with `file://` because the app loads its Wasm and font data files):

   ```sh
   python3 -m http.server 8000 --directory web
   ```

4. Open `http://localhost:8000`.

### Publish it on GitHub Pages

The repository includes `.github/workflows/static.yml`. On each push to `main`, GitHub Actions installs Emscripten, runs `make web`, and deploys the generated `web/` files to GitHub Pages.

For a shorter checklist, see [DEPLOY-GITHUB-PAGES.md](DEPLOY-GITHUB-PAGES.md).

1. Push this repository to GitHub's `main` branch.
2. In the repository, open **Settings → Pages** and set the build/deployment source to **GitHub Actions**.
3. Open the **Actions** tab and wait for **Deploy static content to Pages** to complete successfully.
4. Visit the Pages URL shown in the workflow summary (normally `https://<username>.github.io/<repository>/`).

The generated `.wasm`, `.js`, and `.data` files are build outputs; Actions creates them for deployment, so they do not need to be committed. `web/shell.html`, `web/site.css`, the fonts and their license notices are the authored browser-shell files.


---

# Native C / SDL2 Edition

This repository now also includes a native C recreation of the interactive visual essay. It preserves all 20 original catalogue entries—eight planets, five dwarf planets, the Milky Way, and six galaxies—including their chapter titles, field-note prose, epigraphs, and astronomy data.

The C build replaces the original browser's Three.js renderer with a stylized SDL2 2D/2.5D scene: an animated starfield, orbit paths, an asteroid belt, moonlets, cached procedural planet surfaces with spherical lighting, Saturn's rings, galaxy illustrations, four category views, navigation, and a chapter/data panel. It is not a pixel-identical or full 3D replacement; the original web files remain available as `index.html`, `main.js`, and `styles.css`.

## Build and run the C edition

Requirements: a C11 compiler, `pkg-config`, SDL2 development files, SDL2_ttf development files, and DejaVu Serif / Sans Mono plus Liberation Serif fonts. On Debian or Ubuntu:

```sh
sudo apt install build-essential pkg-config libsdl2-dev libsdl2-ttf-dev fonts-dejavu-core fonts-liberation
make
./space
```

Use `make run` to build and launch, or `make test` for the headless smoke test. The test renders every catalogue entry in all four views and saves a representative frame as `/tmp/space-c-smoke.bmp`.

## C edition controls

- Click **Begin transmission** to enter the experience.
- Click **Planets / Dwarfs / Milky Way / Galaxies**, or use keys **1–4** to switch views.
- Click a body or a bottom navigation dot to open its chapter.
- Click the arrows or use **Left / Right** (Space advances) to browse.
- Drag the scene to rotate; scroll over the scene to zoom.
- Scroll inside the essay panel to read longer entries; click **×** to close it.
- **Esc** closes the panel, returns to the intro, or quits.

## C port files

- `main.c` — SDL2 application, rendering, controls, and navigation.
- `bodies.h` — preserved catalogue and supporting body/moon data.
- `Makefile` — build, run, and test targets.
