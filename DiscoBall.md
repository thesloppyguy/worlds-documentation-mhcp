# How to Build a Disco Ball with Lighting Gizmos in Horizon Worlds

Sahil (thesloppyguy)

September 4, 2025

---

## Introduction

This tutorial shows step‑by‑step how to create a reusable, high‑impact **Disco Ball** effect using Horizon Worlds lighting gizmos, glow materials, and simple TypeScript scripts. By the end of this tutorial you'll have a polished, performance‑aware disco ball you can drop into clubs, party rooms, or interactive scenes.

### What you’ll learn

- Constructing the disco ball object and mirrored material
- Layering lights (static + dynamic + environment) for a cinematic result
- Implementing color cycling, rotation, and pulsing with TypeScript
- Adding bloom and atmospheric beams using glow materials and spot gizmos
- Performance tuning for desktop, web and VR

---

## Prerequisites and Expectations

- Basic familiarity with the Horizon Worlds Desktop Editor and Gizmos.
- Comfortable adding entities (Gizmos), materials, and editing properties in the editor.
- Basic TypeScript scripting experience for Horizon Worlds (code blocks/components).
- Recommended reading: Lighting fundamentals and dynamic lighting docs (see References).

---

## Document Organization

- [Overview & Design](#overview--design)
- [Step-by-step Build](#step-by-step-build)

  - Model & materials
  - Environment & base light
  - Disco spot system (dynamic lights)
  - Color cycling & rotation scripts
  - Bloom & atmospheric beams

- [Optimization & Testing](#optimization--testing)
- [Extra variations & reuse](#extra-variations--reuse)
- [Code snippets](#code-snippets)
- [References](#references)

---

## Overview & Design

Design goal: make a visually striking disco ball that:

- Rotates slowly and casts colorful, sparkling highlights.
- Uses a small number of dynamic lights for interactivity (respecting world limits).
- Leverages glow materials and the Environment Gizmo for bloom/atmosphere.

**Layering approach (recommended):**

1. **Base Layer (Environment Gizmo)** — sets time of day, ambient fog and global tone.
2. **Static Layer (Glow materials + static lights)** — broad illumination and bloom.
3. **Dynamic Layer (Spot / Point gizmos)** — moving colored beams and highlights.
4. **Accent Layer (specular material + rotation)** — the disco ball's mirrored material + rotation to catch lights.

---

## Step-by‑step Build

### 1) Create the Disco Ball mesh & material

**Option A — Faceted sphere mesh (recommended):**

- Add a sphere mesh and apply a **tiled mirror material** (high specular, low roughness). If you don't have a faceted mesh, use a normal map that simulates mirrored tiles.
- Material properties to emphasize:

  - **Metallic/Specular:** High (near 1.0)
  - **Roughness:** Very low (0.0–0.1) to create sharp specular highlights
  - **Emission / Glow:** Small value to let bloom accent tiny highlights (optional)

**Option B — Mirror with decal tiles:**

- Use a mirrored base material and overlay a small tiled emissive texture (tiny white dots) to fake micro-reflections.

**Place the disco ball entity above your dance floor** and add a simple parent entity for rotation control.

> Screenshot placeholder: Disco ball mesh and inspector showing material sliders.

---

### 2) Environment & Base Lighting

1. Add an **Environment Gizmo** and choose a Night / Club preset. Reduce ambient brightness slightly — you want the spotlights to pop.
2. Set **Fog Density** low (0.02–0.08) to enable visible light beams without obscuring the scene.
3. Add a couple of **Static Light Gizmos** (Ellipsoid/Cuboid) under the ceiling to give warm fill light. Use low intensities; these are the 'mood' lights.

---

### 3) Create the Disco Spot System (dynamic lights)

**Design:** Use 4–8 spot gizmos positioned around the ball. They will change color and intensity over time, producing classic disco effects.

**How to place them:**

- Add **Dynamic Light Gizmo** (Spot type) facing the ball. Set initial intensity moderate (3–6), falloff distance to cover floor, and spread (cone angle) 20–45°.
- Duplicate around the ball at 45–90° increments.

**Important performance note:** dynamic lights are expensive and limited — do not exceed 8 on mobile or 12 on web/desktop without testing. Reserve more only for high-end PC VR. (See Optimization section.)

---

### 4) Color Cycling & Rotation (TypeScript)

You’ll attach two scripts:

- **RotationController** — rotates the disco ball entity parent.
- **DiscoLightController** — cycles colors and pulses intensity on each spot gizmo.

(See Code Snippets section below for full code.)

Key behavior:

- Color cycle uses smooth HSV shifts for pleasing transitions.
- Intensity pulsing uses sine-based easing for natural breathing.
- Rotation speed configurable (0.5–4 degrees per frame unit).

---

### 5) Bloom, Glow & Atmospheric Beams

Horizon Worlds doesn’t expose a global post-process bloom slider in code; instead rely on materials + Environment Gizmo:

- Add a **Glow material** on small underlying emissive decals on the ball to make tiny highlights bloom.
- For visible light beams, add thin, elongated **static emissive geometry** (cone meshes) aligned with each spotlight; give those meshes an emissive glow material. Toggle their visibility together with the spot gizmos for synchronized beams.

---

## Optimization & Testing

### Light budget & device targets

- **Dynamic lights**: Keep ≤ 8 for mobile; ≤ 12 for web/desktop; test VR targets carefully. This tutorial defaults to **6 dynamic spots** to stay safe. Refer to platform limits when adjusting.

### Performance checklist

- Use static lights for ambient fill whenever possible. fileciteturn0file3
- Keep emissive texture sizes small; use atlases for repeated glow decals. fileciteturn0file4
- Avoid per-frame heavy scripting; use interval/tweening with moderate frequency (30–60Hz step logic). fileciteturn0file1

### Testing steps

1. Test in Web (press P for metrics) and VR (Real‑time Metrics panel) — validate FPS and memory. fileciteturn0file4
2. Try the scene with only 2 dynamic lights, then increment up to your intended count and observe FPS/memory.
3. If budget is tight, reduce spotlight spread, lower shadow settings, or convert beams to simple emissive geometry.

---

## Extra variations & reuse

- **Magic Orb**: Desaturate metallic, boost emission, and slow rotation for mystical versions.
- **Synchronized Club**: Tie color cycles to music beats (use event trigger broadcaster / audio analysis to dispatch network events). See dynamic lighting/event docs for trigger patterns. fileciteturn0file1
- **Mini‑Disco**: For small rooms, use 2–3 tiny spotlights and stronger glow materials to fake more lights.

---

## Code Snippets

### RotationController (simplified)

```ts
import { Component, PropTypes } from "horizon/core";

export class RotationController extends Component<typeof RotationController> {
  static propsDefinition = {
    rotationSpeed: { type: PropTypes.Number, defaultValue: 0.6 }, // degrees per second
  };

  override start() {
    this.async.setInterval(() => this.updateRotation(), 16); // ~60Hz
  }

  private updateRotation() {
    const current = this.entity.transform.rotationEuler.get();
    const newY = (current.y + this.props.rotationSpeed * 0.016) % 360;
    this.entity.transform.rotationEuler.set({
      x: current.x,
      y: newY,
      z: current.z,
    });
  }
}

Component.register(RotationController);
```

### DiscoLightController (color cycling & pulsing)

```ts
import { Component, PropTypes, DynamicLightGizmo, Color } from "horizon/core";

export class DiscoLightController extends Component<
  typeof DiscoLightController
> {
  static propsDefinition = {
    lights: { type: PropTypes.Array }, // array of entities (DynamicLightGizmo spots)
    cycleSpeed: { type: PropTypes.Number, defaultValue: 1.0 },
    pulseSpeed: { type: PropTypes.Number, defaultValue: 1.2 },
  };

  private t = 0;

  override start() {
    this.async.setInterval(() => this.tick(), 100); // 10Hz update — light changes need not be 60Hz
  }

  private tick() {
    this.t += 0.1 * this.props.cycleSpeed;
    const color = this.hsvToRgb(this.t % 1, 0.9, 0.9);

    for (const e of this.props.lights) {
      const g = e.as(DynamicLightGizmo);
      if (!g) continue;
      // pulse intensity
      const pulse =
        3 +
        Math.abs(
          Math.sin(this.t * this.props.pulseSpeed * (1 + Math.random() * 0.2))
        ) *
          4;
      g.intensity.set(pulse);
      // Note: color of dynamic lights must be set in editor in some systems; if scripting supports it, use:
      try {
        g.color.set(new Color(color.r, color.g, color.b));
      } catch (e) {}
    }
  }

  private hsvToRgb(h: number, s: number, v: number) {
    // returns {r,g,b} in 0..1
    const i = Math.floor(h * 6);
    const f = h * 6 - i;
    const p = v * (1 - s);
    const q = v * (1 - f * s);
    const t = v * (1 - (1 - f) * s);
    const mod = i % 6;
    if (mod === 0) return { r: v, g: t, b: p };
    if (mod === 1) return { r: q, g: v, b: p };
    if (mod === 2) return { r: p, g: v, b: t };
    if (mod === 3) return { r: p, g: q, b: v };
    if (mod === 4) return { r: t, g: p, b: v };
    return { r: v, g: p, b: q };
  }
}

Component.register(DiscoLightController);
```

> Note: some platforms limit setting light **color** via script — if you cannot change color at runtime, preconfigure lights with different colors and toggle `enabled` instead (or use multiple lights per position and alternate them).

---

## References

## <!-- TODO: PUT ITEMS HERE -->

<!-- TODO: REMOVE THIS -->

## Assets / Screenshots to include before submission

- Inspector screenshots for key gizmo settings (spot spread, falloff, intensity)
- Example material settings (specular, roughness, emission)
- Final scene full‑screen screenshot (day/night comparison)
- A short 60–90s screen capture showing the disco ball in action for the optional video submission

---
