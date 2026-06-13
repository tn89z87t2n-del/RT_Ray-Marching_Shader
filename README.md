# RT Ray-Marching Shader Playground

A real-time **ray-marching (sphere tracing) shader playground** in a single self-contained
`index.html` — inline CSS + JS, **WebGL2**, zero external dependencies. All rendering happens
in one fullscreen fragment shader.

**▶ Try it live:** https://raw.githack.com/tn89z87t2n-del/RT_Ray-Marching_Shader/main/index.html
*(or just download `index.html` and open it in any modern browser)*

## Scenes

| Scene | Description |
|---|---|
| **Mandelbulb** | Classic power-8 fractal with orbit-trap coloring and optional animated power morphing (2 → 12) |
| **Menger Sponge** | Infinitely detailed sponge with distance-based LOD and slowly rotating fold space |
| **Infinite City** | Hash-based domain repetition of towers — endless alien city at night with emissive windows |
| **Mirror Room** | Sphere + pillars in a reflective box, up to 3 reflection bounces, colored area light panels |
| **Gyroid World** | TPMS gyroid caves with pulsing glowing veins (second SDF shell, emissive) |

## Rendering features

- Sphere-traced ray marching: up to 256 steps, pixel-cone adaptive epsilon, step relaxation, distance bailout
- Normals via the tetrahedron technique (4 SDF samples)
- Soft shadows (penumbra factor from shadow-ray marching), 5-sample SDF ambient occlusion
- 2–3 light setup per scene, Blinn-Phong speculars, Fresnel rim lighting
- Exponential distance fog blending into per-scene sky gradients (with twinkling stars)
- Inigo Quilez cosine palettes — 3 presets per scene with live gradient swatches in the UI
- ACES-ish tonemapping (Narkowicz) + gamma correction + subtle vignette

All 5 scenes live in **one** shader switched by a `uScene` uniform — the branch is uniformly
coherent across the whole frame (no warp divergence) and it avoids 5× compilation and
program-switching complexity.

## Performance

- Render-resolution scale (0.25×–1.0×, default 0.75×) — renders to a smaller framebuffer and upscales
- `devicePixelRatio` capped at 1.5
- **Adaptive quality**: automatically lowers resolution when FPS < 45, restores when > 58
- FPS counter + effective render resolution in the HUD

## Controls

| Input | Action |
|---|---|
| **Click** | Capture mouse (pointer lock) |
| **Mouse** | Look around |
| **W / A / S / D** | Move |
| **E / Space** · **Q / Shift** | Up · Down |
| **Scroll** | Movement speed |
| **H** | Hide / show all UI |
| **Touch** | 1 finger = look, 2 fingers = move |

## UI

Dark glassmorphism panel (collapsible) with: scene selector, palette presets, resolution scale,
fog density, shadow softness, AO strength, animation speed, FOV, and a **per-scene parameter**
(mandelbulb power, menger iterations, city density, mirror reflectivity, gyroid thickness).
Plus a full-resolution PNG **screenshot** button, crosshair, and position/FPS HUD.

## Code layout (`index.html`)

1. **Shader sources** — vertex (fullscreen triangle via `gl_VertexID`), main ray-marching fragment shader (SDF comments in Slovak, identifiers in English), blit shader
2. **WebGL boilerplate** — context, program linking, offscreen FBO for resolution scaling
3. **Camera / input** — fly camera with smooth velocity damping, pointer lock, touch
4. **UI binding** — state, sliders, palettes, adaptive quality, screenshot, render loop
