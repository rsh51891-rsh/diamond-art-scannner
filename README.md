# Diamond Art Symbol Finder

Single-page web app for iPhone (works in any modern browser). Photograph a
diamond-painting canvas, tap one example of a symbol, and every matching
symbol in the photo is circled.

## Use

1. **Photo** → take a picture (or pick one from the library). Shoot square-on,
   tilt to avoid glare from the plastic film.
2. Pinch to zoom, drag to pan, **Fit** to reset.
3. Tap a symbol. The tap snaps to the symbol center; the cyan box and the
   preview show what will be matched. Use the arrow pad to nudge, ● to re-snap.
4. **Find**. Adjust **Match ≥** live: lower to catch faint/glare-affected
   symbols, raise to drop look-alikes (default 0.60).
5. **Box size** sets the template size relative to the grid cell (default 0.85).

## Install on iPhone

Host the folder over HTTPS (GitHub Pages: repo **Settings → Pages → Deploy from
a branch**, pick the branch and `/ (root)`). Open the URL in Safari →
Share → **Add to Home Screen**. It runs offline after the first load.
All processing is on-device; the photo is never uploaded.

## Method

1. Grid pitch: autocorrelation of row/column gradient-energy profiles
   (first strong peak, so harmonics are rejected).
2. Image resampled so pitch ≈ 22 px, converted to gray, then local contrast
   normalization `(g − μ_r)/σ_r` with r ≈ 0.2·pitch. This removes print tint,
   lighting gradients and the cell fill so matching keys on the strokes.
3. Normalized cross-correlation of the tapped cell against the whole image
   (integral images for window mean/variance), in a Web Worker.
4. Non-max suppression with radius 0.5·pitch; score threshold is the slider.

Limits: no rotation/scale invariance (keep the camera square to the canvas);
specular glare destroys contrast locally, so symbols under glare are missed —
take a second photo from a different angle for those areas.

`test/sample.jpg` is a reference photo for testing.
