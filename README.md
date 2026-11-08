# NEXUS — Beyond Reality

An award-grade, cinematic 3D web experience. One continuous WebGL universe
(hero core → light tunnel → shard field → product artifact → open world →
portal finale) driven entirely by scroll, cursor and physics.

## Stack

- **Vite + React 18** — app shell
- **Three.js / React Three Fiber / Drei** — real-time 3D
- **@react-three/postprocessing** — Bloom, Depth of Field, Chromatic Aberration, Grain, Vignette
- **GSAP + ScrollTrigger + ScrollTo** — DOM & scroll choreography
- **Lenis** — inertial smooth scrolling
- Procedural geometry & shaders everywhere — **zero external assets** (models, textures, HDRIs)

## Run

```bash
npm install
npm run dev      # http://localhost:5173
npm run build    # production build → dist/
npm run preview  # serve the production build
```

## QA smoke test

With `npm run preview` (or dev) serving the site, run the Playwright suite:

```bash
npx playwright install chromium   # one-time, if browsers are missing
node scripts/smoke.cjs
```

It walks every section, exercises every control (material modes, hotspots,
drag, zoom, explore ripple, nav routing, mobile menu at a phone-size reload),
screenshots each stop into `scripts/shots/`, and exits non-zero if any console
error appears. It should end with `=== CONSOLE ERRORS === none`.

## Map

```
src/
  components/
    Navbar.jsx        glass nav + mobile menu + section routing
    Hero.jsx          typographic hero overlay
    Intro.jsx         S1 — cinematic statement + glass stats
    ExperienceSection.jsx  S2 — pinned captions through the tunnel
    Innovation.jsx    S3 — parallax chips over the shard field
    ProductShowcase.jsx    S4 — material modes, zoom, hotspots panel
    Explore.jsx       S5 — full-bleed interactive environment CTA
    Final.jsx / Footer.jsx S6 — portal finale + signal footer
    Scene3D.jsx       <Canvas> bootstrap (lazy-loaded)
    Cursor.jsx        custom cursor (hover/drag states)
    Loader.jsx        boot sequence + progress bar
    ProgressDots.jsx  section jump dots
  scenes/
    MainScene.jsx     lights, procedural HDRI, fog interpolation, zone assembly
    CameraRig.jsx     keyframe camera driven by scroll + mouse + shake + zoom
    ParticleScene.jsx GPU particle field (52k-class, mouse repulsion, zone swirl)
    InteractiveObject.jsx  hero "Synth Core 001" (raycast hover/click)
    IntroStreaks.jsx  zone-01 light streaks
    ExperienceTunnel.jsx   zone-02 rings reacting to scroll velocity
    InnovationField.jsx    zone-03 instanced shards + shader grid
    ShowcaseStage.jsx zone-04 artifact: drag physics, material morphs, hotspots
    ExploreZone.jsx   zone-05 nebula dome, signal ground, monoliths
    FinalPortal.jsx   zone-06 charged portal finale
    Effects.jsx       post stack
  animations/
    cameraAnimations.js  world state, camera keyframes, zones, helpers
    scrollAnimations.js  split typography, reveals, parallax, magnetic buttons
  styles/
    global.css / animations.css
```

## Interactions cheat-sheet

| Where | Action | Result |
|---|---|---|
| Hero | Move cursor | Core follows & camera parallaxes |
| Hero | Click the core | Core-sync pulse + camera kick |
| Anywhere | Scroll | The camera travels 320 m through six 3D zones |
| Showcase | Drag the artifact | Inertial 360° rotation |
| Showcase | METAL / GLASS / HOLO | Physical material morphing |
| Showcase | + / − / ⟲, hotspot dots | Zoom, reset, instanced detail cards |
| Explore | Click the ground | Signal-ripple wavefront |
| Explore | Hover/click monoliths | Wake + beam surge |
| Nav / dots / CTAs | Click | GSAP-eased routing through the world |

## Performance & gracefully-degraded paths

- Zones mount/unmount by camera proximity (no wasted GPU work)
- Mobile/coarse-pointer: reduced particle count, DPR clamp, no DOF/CA
- `prefers-reduced-motion`: Lenis off, instant reveals, static camera easing, post stack disabled
- No WebGL → static gradient scene + fully functional DOM site
