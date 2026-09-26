# Publish the C/SDL2 version on GitHub Pages

The C application is compiled to WebAssembly, so visitors run it in their browser without installing anything. The original JavaScript edition stays in the repository; the Pages workflow publishes the native-C edition from `web/`.

## First-time setup

1. Add and push the files from this project to the repository's `main` branch. Do **not** upload the compiled `space` desktop executable. Include the source, `web/`, `Makefile`, and `.github/workflows/static.yml`.
2. Open the repository on GitHub and go to **Settings → Pages**.
3. Under **Build and deployment**, select **GitHub Actions** as the source.
4. Go to **Actions** and ensure workflows are enabled. The `Deploy static content to Pages` workflow should start after the push. You can also start it with **Run workflow**.
5. When the deployment job succeeds, open the Pages URL shown on the workflow run. For `STEVEALEX-source/space`, it should be `https://stevealex-source.github.io/space/`.

After that, every push to `main` rebuilds and republishes the site. If Pages reports that it cannot deploy, check that the Pages source is set to **GitHub Actions** and that workflow permissions are allowed in repository settings.

## Build and preview locally

Install Emscripten (the first `make web` build fetches the SDL2/SDL2_ttf ports), then run:

```sh
make web
python3 -m http.server 8000 --directory web
```

Open `http://localhost:8000`. Do not open `web/index.html` as a `file://` URL: browsers require a local HTTP server to fetch the `.wasm` and preloaded font data correctly.

The checked-in `web/fonts` directory contains the fonts used by the original C UI; their license notices are included in `web/fonts/FONT-LICENSES.txt`.
