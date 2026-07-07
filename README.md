# Daltonize

A single-page, client-side tool that shows what an image looks like to someone with color vision deficiency (color blindness). Everything runs in the browser — no image is ever uploaded anywhere.

Open [daltonize.html](daltonize.html) in any modern browser to use it.

## How it works

1. **Upload an image.** Drag a file onto the dropzone or click to browse. The image is downscaled (longest side capped at 1400px) and drawn onto an off-screen `<canvas>` that acts as the working copy — this keeps simulation fast even for large photos.

2. **Simulate color vision deficiency.** For each condition, the tool reads the image's pixel data (`getImageData`) and transforms every pixel's RGB values using a fixed 3×3 matrix that approximates how that condition alters color perception:
   - **Protanopia** — missing long-wavelength (red) cones
   - **Deuteranopia** — missing medium-wavelength (green) cones, the most common form
   - **Tritanopia** — missing short-wavelength (blue) cones
   - **Achromatopsia** — no functioning cones; the image is converted to grayscale using luminance weighting instead of a matrix

   Each simulated result is cached per mode so switching back and forth doesn't recompute it.

3. **Compare side by side.** The original and the simulated version are shown next to each other in the viewer, each capped to a max height so the pair always fits on screen without scrolling.

4. **Browse all types at once.** A thumbnail grid below the viewer renders all four conditions at once; clicking a thumbnail loads it into the main comparison view.

5. **Download.** The "Download simulated image" button exports the currently selected simulation as a PNG via `canvas.toDataURL`.

## Tech notes

- No build step, no dependencies — plain HTML/CSS/JS in one file.
- Fonts (Fraunces, Inter, IBM Plex Mono) are loaded from Google Fonts; everything else is self-contained.
- All image processing happens locally via the Canvas API; nothing is sent to a server.
