# Design Decisions

Decisions made on top of [REQUIREMENTS.md](REQUIREMENTS.md). Where they differ, this file wins.

## Devices and views

- One implementation scales across every device and every view (full, half horizontal, half vertical, quadrant), in landscape and portrait. No device is special-cased by name.
- Capabilities come from the framework's screen classes, not from model names:

  | Screen class | Palette | Example devices |
  | --- | --- | --- |
  | `screen--color-4bwry` | black, white, red, yellow | TRMNL OG (B/W/R/Y), 800×480 |
  | `screen--4bit` | 16 grays | TRMNL X, 1872×1404 |
  | `screen--2bit` | 4 grays | TRMNL OG (2-bit), 800×480 |
  | `screen--1bit` | black, white | TRMNL OG (1-bit), 800×480 |

  Other color palettes (`color-6a`, `color-7a`, `color-full`) also contain pure red and yellow, so they get the color treatment.
- The B/W/R/Y version is checked in TRMNL's markup preview, since there's no hardware for it.

## Color

- **Color displays:** red (`#FF0000`) wax. The liquid is a setting (`liquid_color`): yellow (default), white or black.
- **Grayscale displays:** dark wax in light liquid, whatever `liquid_color` says.
- **Shading:** 1-bit and 4-color screens use flat fills only. Depth comes from shapes (an inner core, a highlight edge in yellow or white), never gradients. Tonal shading is used only where there are 4 or more grays: 2-bit screens get a rim and a core, 16-gray screens a rim, a mid band and a core. A layer whose tone matches the one beneath it is not drawn at all.

## Motion

- Each frame is computed from the current time on one fixed timeline. No state is stored between renders.
- The shift between refreshes is gentle: the scene should read as the same lamp a little later, not a new composition.
- The `speed` setting (very slow, slow (default), normal) scales the timeline. `trmnl.plugin_settings.refresh_interval_minutes` is available to the views, so movement per refresh can be kept gentle at any refresh interval.
- Each blob circulates in a convection loop: up one side of its lane, down the other, with a slow drift of the lane itself. The loop keeps the velocity turning rather than reversing, so nothing about a blob (its tail, its drip) jumps at the top or bottom of its travel.
- Rising wax is quicker, stretched and trails a drip that pinches off; sinking wax is slower, broader and blunt.
- Lanes follow the golden-ratio sequence, which spreads any number of blobs evenly across the width without columns or gaps.

## Rendering

- The wax is one continuous field built from overlapping blobs and drawn as SVG outlines at a threshold, so blobs merge and pinch apart naturally.
- Blob count scales with screen area and blob sizes vary within a range; the composition is never a stretched fixed layout.
- Blob size and area per blob scale with the square root of the screen's shorter side, taken from the screen rather than the view. The TRMNL X has nearly twice the pixel density of the OG, so this keeps blobs a similar physical size there while still fitting more of them. Mashup views on one device use the same scale as its full screen.
- Outlines are resampled evenly along their length before the curve is fitted through them, which smooths the marching-squares wobble and keeps the path data small. Outlines smaller than a drip's tip are dropped. The gloss crescent, only a few pixels thick, is extracted on a grid twice as fine as the wax.
- Plain JavaScript in the page, no libraries, no external data.
- A still composition is present before the script runs, so a script failure still shows a lava lamp.
