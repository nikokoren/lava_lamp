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
- **Shading:** 1-bit and 4-color screens use flat fills only. Depth comes from shapes (an inner core, a highlight edge in yellow or white), never gradients. Tonal shading is used only where there are 4 or more grays.

## Motion

- Each frame is computed from the current time on one fixed timeline. No state is stored between renders.
- The shift between refreshes is gentle: the scene should read as the same lamp a little later, not a new composition.
- The `speed` setting (very slow, slow (default), normal) scales the timeline. `trmnl.plugin_settings.refresh_interval_minutes` is available to the views, so movement per refresh can be kept gentle at any refresh interval.

## Rendering

- The wax is one continuous field built from overlapping blobs and drawn as SVG outlines at a threshold, so blobs merge and pinch apart naturally.
- Blob count scales with screen area and blob sizes vary within a range; the composition is never a stretched fixed layout.
- Plain JavaScript in the page, no libraries, no external data.
- A still composition is present before the script runs, so a script failure still shows a lava lamp.
