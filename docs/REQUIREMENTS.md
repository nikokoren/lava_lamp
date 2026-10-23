# Recipe Requirements: Lava Lamp

## 1. Concept

Create a TRMNL recipe that transforms the entire display into an abstract, full-screen lava lamp. The screen is filled edge to edge with slowly moving, organic blobs of red wax suspended in a contrasting liquid.

The display should feel like looking directly into a lava lamp at extreme close range. There is no visible lamp, glass vessel, base, or surrounding environment. The wax itself is the subject.

The goal is to create a calming, hypnotic visual that takes advantage of the TRMNL's e-paper display, particularly the larger TRMNL X and its RGBY color capabilities.

## 2. Visual Design

### Full-screen composition
- Fill the entire screen with the lava lamp scene.
- No borders, frames, glass outlines, lamp bases, labels, text, clocks, or other UI elements.
- Blobs should extend beyond the screen edges to create the impression of an infinite, continuous field of moving wax.
- Avoid obvious empty areas. The composition should remain visually rich without becoming cluttered.
- The visual should work in both portrait-like and landscape-like aspect ratios, adapting to the target device's dimensions.

### Wax blobs
- On RGBY TRMNL devices, the wax blobs must be red.
- Use the actual red channel supported by the device rather than approximating red with grayscale.
- Blobs should have smooth, rounded, organic silhouettes.
- Shapes should vary in size and aspect ratio, from small rounded blobs to large, elongated forms.
- Blobs should appear to stretch, narrow, bulge, merge, separate, and deform naturally.
- Avoid perfect circles, rigid geometric shapes, repeated identical silhouettes, or patterns that look mechanically generated.
- Multiple blobs should be visible at different stages of their movement at any given time.
- Large blobs may be partially cropped by the screen edges.

### Background liquid
- The background should provide strong contrast with the red wax.
- On RGBY devices, use a suitable non-red background color or combination supported by the display, selected to evoke the liquid surrounding the wax.
- On monochrome devices, adapt the palette to grayscale while preserving clear separation between wax and liquid.
- Keep the palette restrained. The wax should remain the visual focus.

### Shading and depth
- Use subtle internal shading, highlights, or tonal variation only where the display supports them and they improve the illusion of depth.
- Avoid gradients or effects that produce excessive dithering, noise, or visual artifacts on e-paper.
- The blobs should look like thick, translucent or semi-opaque wax rather than flat icons.
- Prioritize recognizable silhouettes and smooth contours over photorealistic detail.

## 3. Motion and Animation

### Core movement
The scene should simulate the characteristic slow convection of a physical lava lamp.

- Blobs generally rise and sink vertically.
- Blobs stretch as they move, then widen or contract as their movement changes.
- Blobs may approach, touch, merge, pinch apart, and separate.
- Movement should feel continuous and organic rather than like objects independently sliding up and down.
- Different blobs should move at different speeds and occupy different stages of the cycle.
- Avoid synchronized movement, abrupt jumps, random jitter, and rapid changes.

### E-paper constraints
TRMNL uses a refresh-based e-paper display rather than a continuously updating screen. The recipe must account for the fact that each rendered image may remain visible until the next refresh.

- Generate a new composition for each recipe refresh.
- Successive compositions should represent plausible later states of the same evolving lava lamp, rather than unrelated random arrangements.
- Preserve enough continuity between frames that blobs appear to have moved, deformed, or interacted.
- Avoid large, arbitrary changes between refreshes that make the display appear to flicker or jump.
- Do not depend on CSS animations or browser-side animation continuing after the image has been rendered to the device.
- Investigate whether state can be persisted between renders to support continuity. If persistence is unavailable or unreliable, use deterministic procedural generation based on time or another stable input.

The intended effect is slow-motion lava lamp movement across successive refreshes, not real-time animation.

## 4. Technical Approach

### Rendering
- Prefer procedural generation of organic blob shapes over a collection of static images.
- SVG or another suitable vector-based rendering approach is preferred if supported by the recipe environment.
- Shapes should be parameterized so their positions, dimensions, contours, and interactions can evolve between frames.
- Use a stable simulation timeline or equivalent state model to calculate each frame.
- Ensure the rendering method produces output compatible with the target TRMNL recipe format and device display capabilities.

### Color handling
- Detect or configure the target display's color capabilities where feasible.
- Use red wax on RGBY devices.
- Provide a deliberate monochrome fallback for black-and-white devices.
- Avoid relying on colors or shades that the target display cannot reproduce.
- Test actual device output rather than assuming browser previews accurately represent e-paper color reproduction.

### Performance
- Keep computation and generated output lightweight enough for routine recipe refreshes.
- Avoid unnecessary external services, API calls, or third-party dependencies.
- The core visual should not require external data.
- If the rendering process fails, the recipe should degrade gracefully rather than returning a broken image.

## 5. Refresh and Power Considerations

- The recipe should work with normal TRMNL refresh behavior.
- Do not require unusually frequent refreshes for the concept to function.
- Treat increased refresh frequency as an optional experiment, not a core requirement.
- Consider the impact of frequent refreshes on battery life and display wear.
- A new frame should provide a noticeable but coherent evolution of the scene.

## 6. Responsive Layout

- Use the full available display area on each supported device.
- Adapt blob dimensions, count, and placement to the display aspect ratio.
- Do not simply stretch a fixed composition to fit different screens.
- Ensure the scene remains compelling on the smaller OG display as well as the larger TRMNL X.
- On larger screens, use the additional space to accommodate more simultaneous blobs and more varied shapes, rather than scaling up a single blob disproportionately.

## 7. Explicit Non-Goals

The recipe is not intended to:
- Draw a conventional lava lamp with a visible glass vessel, cap, or base.
- Display a static illustration or photograph.
- Provide continuously animated video or real-time fluid simulation on the e-paper device.
- Include clocks, weather, statistics, text, controls, or other dashboard widgets.
- Introduce unrelated decorative elements.
- Use arbitrary random frame changes that undermine the impression of physical motion.

## 8. Acceptance Criteria

The first implementation is successful when:

1. The entire display is filled with an edge-to-edge lava lamp scene.
2. No lamp outline, base, frame, or other physical surroundings are visible.
3. The RGBY version uses red wax blobs with clear contrast against the background liquid.
4. The blobs have varied, organic silhouettes and do not look like uniform circles.
5. Multiple blobs appear at different stages of rising, sinking, stretching, and merging.
6. Successive refreshes produce plausible changes in blob position and shape.
7. The scene does not visibly jump to an unrelated composition at each refresh.
8. The result remains legible and visually appealing on both TRMNL X and smaller supported devices.
9. The output respects the target device's color palette and e-paper rendering constraints.
10. The recipe works without external data sources and remains lightweight enough for routine refreshes.

## 9. Recommended Development Order

1. **Static visual prototype:** Render a single full-screen composition with convincing red wax blobs and a contrasting liquid background.
2. **Procedural shapes:** Implement organic blob contours with varied sizes and proportions.
3. **Motion model:** Add vertical convection, deformation, merging, and separation.
4. **Frame continuity:** Ensure successive renders follow a coherent timeline.
5. **Device adaptation:** Tune colors, contrast, dithering, composition, and sizing for RGBY and monochrome displays.
6. **On-device validation:** Test successive refreshes on actual hardware and adjust the movement rate and visual complexity.

The priority is a convincing, calming visual first. More sophisticated fluid behavior should only be added if it improves the effect on the actual e-paper display.
