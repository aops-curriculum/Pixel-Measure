# Pixel Measure

**`measure.html`** is a universal, parameterized image measuring tool. Drop it once; configure any instance via URL parameters. All settings sync to the URL in real time, so you can configure via the control panel and copy the URL to embed in a class.

## Quick start

```
measure.html?image=https://example.com/photo.jpg&cal=151,11.6,ft&unit=ft_in
```

Open the page, click **⚙ Panel** in the top-left to open the control panel, configure everything visually, then click **📋 Copy shareable URL** to get a ready-to-embed link.

## How to use

- **Drag** on the image to measure a distance. Arrowheads appear at both ends.
- **Click-click** (alternative): single click sets the start point; an animated arrow follows your cursor; second click commits the endpoint.
- **Multiple measurements** accumulate (each gets a numbered label).
- **Right-click** to remove the last measurement (or cancel a pending click-click measurement). **Esc** cancels a measurement in progress.
- **Scroll wheel** to zoom in/out toward the cursor.
- **Alt + drag** (or middle-click + drag) to pan when zoomed in.
- **Arrow keys** nudge the current drag endpoint by 1 image pixel (**Shift + arrow** = 10 px).
- A **floating magnifier** follows the cursor for precise endpoint placement.

## URL Parameters

| Parameter | Default | Description |
|---|---|---|
| `image` | *(none)* | URL of the image to load |
| `cal` | *(none)* | Calibration as `px,dist,unit` — e.g. `cal=151,11.6,ft` means 151 image pixels = 11.6 ft |
| `unit` | `px` (or cal unit) | Display unit: `px`, `ft`, `ft_in`, `m`, `cm`, `km` |
| `grid` | *(off)* | Grid spacing with unit — e.g. `grid=1ft`, `grid=50px`, `grid=0.5m`. Omit or `grid=false` to hide. |
| `grid_color` | `#0064c8` | Grid line color (any CSS hex color) |
| `mag` | `true` | `mag=false` hides the floating magnifier |
| `zoom` | `true` | `zoom=false` disables scroll-to-zoom and alt-drag pan |
| `single` | `false` | `single=true` — each new drag replaces the previous measurement (replicates original sim behavior) |
| `status` | `true` | `status=false` hides the coordinate/distance status bar below the image |
| `px` | `image` | Pixel coordinate system when `unit=px`: `image` = original image pixels (default, zoom-independent); `display` = canvas pixels at the initial fit scale (matches legacy hardcoded sims) |
| `title` | *(none)* | Text displayed above the image |
| `panel` | `true` | `panel=false` hides the control panel (use for production embeds) |
| `max_w` | `820` | Maximum canvas width in pixels |

### Legacy separate calibration params (also accepted)

`cal_px=151&cal_dist=11.6&cal_unit=ft` — equivalent to `cal=151,11.6,ft`.

## Calibration units

`cal` and `grid` accept these unit strings:

| String | Meaning |
|---|---|
| `ft` | feet |
| `m` | meters |
| `cm` | centimeters |
| `km` | kilometers |
| `px` | image pixels (grid only) |

Display unit `ft_in` formats as `6' 2"` (feet and inches).

## Examples

### Pixels only (no scale)
```
measure.html?image=https://host/photo.jpg&panel=false
```

### Feet, with calibration
```
measure.html?image=https://host/basketball.png&cal=151,11.6,ft&unit=ft_in&panel=false
```

### Metric, with grid
```
measure.html?image=https://host/guitar.png&cal=604,64.6,cm&unit=cm&grid=10cm&panel=false
```

### Large image with title
```
measure.html?image=https://host/satellite.jpg&cal=500,1,km&unit=km&title=Satellite+Image&max_w=1000&panel=false
```

## Pixel coordinate systems

By default (`px=image`), distances are reported in **original image pixels** — the actual pixel dimensions of the source file. This is zoom-independent and consistent regardless of canvas size.

The legacy hardcoded sims (`basketball_player.html`, `guitar_measure.html`, etc.) reported **canvas pixels** — the drag distance in their fixed 650×650 canvas. Because those canvases scaled the image down ~3×, their pixel values are ~3× smaller than `measure.html`'s defaults.

To match legacy pixel values exactly: use `?px=display&max_w=650`. The `px=display` mode multiplies by the initial fit-to-canvas scale (zoom-independent, so measurements don't change when you zoom in).

For **real-world units** (`cal=` + `unit=ft/m/cm/km`), this distinction doesn't matter — calibration always works through original image pixels internally.

## Simulations replaced

These hardcoded files in this repo are now superseded by `measure.html` with parameters:

| File | Equivalent URL |
|---|---|
| `basketball_player.html` | `measure.html?image=…basketball_player.png&cal=151,11.6,ft&unit=ft_dec&panel=false` |
| `guitar_measure.html` | `measure.html?image=…guitar_fretboard_counted.png&cal=604,64.6,cm&unit=cm&panel=false` |
| `pixel_measure_4_script_moon_airplane.html` | `measure.html?image=…04_script_moon_airplane.jpg&panel=false` |

See **`comparison.html`** to view all three side-by-side.

## Workflow for new images

1. Open `measure.html` (panel is open by default).
2. Paste the image URL and click **Load Image**.
3. If you know a reference distance: enter it in the Calibration fields, or click **▶ Calibrate by clicking**, click two known points on the image, and enter the real distance when prompted.
4. Choose the display unit.
5. Optionally enable the grid, set a title, etc.
6. Click **📋 Copy shareable URL** — this URL contains all your settings.
7. Embed that URL in an AoPS classroom iframe.

To hide the control panel in the embedded version, click the **×** in the panel header before copying the URL, or add `&panel=false` manually.
