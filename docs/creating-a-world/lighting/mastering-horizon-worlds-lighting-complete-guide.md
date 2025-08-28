# Mastering Horizon Worlds Lighting: Accurate Guide

**Transform your worlds with professional lighting techniques that work within Meta Horizon Worlds’ actual capabilities.** This guide covers foundational concepts, the supported lighting gizmos, interactive lighting systems, environment presets, performance constraints, and troubleshooting—all aligned with the official Horizon Worlds documentation.

**Creator Skill Level**  
Intermediate to Advanced

**Recommended Background Knowledge**  
Familiarity with the Horizon Worlds Desktop Editor, basic world-building, and TypeScript scripting patterns.

**Estimated Time**  
2–3 hours for full tutorial, 30–45 minutes for basic setup

---

## Table of Contents

1. [Lighting Fundamentals](#lighting-fundamentals)
2. [Supported Lighting Gizmos](#supported-lighting-gizmos)
3. [Three-Point Lighting Setup](#three-point-lighting-setup)
4. [Interactive Lighting Systems](#interactive-lighting-systems)
5. [Environment Gizmo & Day/Night Transitions](#environment-gizmo--daynight-transitions)
6. [Performance Constraints & Best Practices](#performance-constraints--best-practices)
7. [Troubleshooting](#troubleshooting)
8. [Resources](#resources)

---

## Lighting Fundamentals

Great lighting guides the eye, sets mood, and enhances immersion. Horizon Worlds lighting relies on gizmos and manual environment settings; there is no programmatic API for advanced effects like volumetrics or procedural weather.

**Three Pillars of Good Lighting**

- **Key Light**: Main directional or point light, creates prominent shadows.
- **Fill Light**: Softer light that reduces contrast.
- **Rim Light**: Backlight that separates subjects from backgrounds.

**Color Temperature**

- _Warm_ (2000K–4000K): Cozy, intimate
- _Neutral_ (4000K–5500K): Balanced, natural
- _Cool_ (5500K–10000K): Tense, mysterious

---

## Supported Lighting Gizmos

Horizon Worlds offers three dynamic light gizmos and static glow materials. All lighting properties are configured via the **Properties Panel**; scripting can only enable/disable lights and change color/intensity.

1. **Directional Light Gizmo**
   - Intensity, Color, Rotation
   - Cast Shadows toggle
2. **Point Light Gizmo**
   - Intensity, Color, Range
   - Cast Shadows toggle
3. **Spot Light Gizmo**
   - Intensity, Color, Range, Angle, Inner Angle
   - Cast Shadows toggle
4. **Glow Material**
   - Apply to static objects for ambient lighting
   - Unlimited uses, but does not support animation

**TypeScript Pattern**

```typescript
import { Component, PropTypes } from "horizon/core";
import { DynamicLightGizmo } from "horizon/gizmos";

class LightController extends Component {
  static propsDefinition = {
    lightEntity: { type: PropTypes.Entity },
  };

  start() {
    const light = this.props.lightEntity.as(DynamicLightGizmo);
    if (light) {
      light.setEnabled(true);
      light.setColor(255, 240, 200);
      light.setIntensity(3.0);
    }
  }
}

Component.register(LightController, "LightController");
```

---

## Three-Point Lighting Setup

1. **Key Light**

   - Add **Directional Light Gizmo** (Shapes > Gizmos > Directional Light)
   - Intensity: 3–5 | Color: 255,248,220 | Cast Shadows: On

2. **Fill Light**

   - Add **Point Light Gizmo** opposite key light
   - Intensity: 1–2 | Color: 240,245,255 | Cast Shadows: Off

3. **Rim Light**
   - Add **Spot Light Gizmo** behind subject
   - Intensity: 2–3 | Angle: 15–25° | Cast Shadows: Off

Enter Play Mode, move around in VR, and fine-tune positions and intensities for balanced shadows and highlights.

---

## Interactive Lighting Systems

### Proximity-Based Lighting

```typescript
import { Component, PropTypes } from "horizon/core";
import { DynamicLightGizmo, TriggerGizmo } from "horizon/gizmos";

class ProximityLighting extends Component {
  static propsDefinition = {
    triggerZone: { type: PropTypes.Entity },
  };

  start() {
    const trigger = this.props.triggerZone.as(TriggerGizmo);
    trigger.onTriggerEnter.add(() => this.toggleLights(true));
    trigger.onTriggerExit.add(() => this.toggleLights(false));
  }

  private toggleLights(on: boolean) {
    for (const entity of this.getEntitiesByComponent(DynamicLightGizmo)) {
      const light = entity.as(DynamicLightGizmo);
      if (light) {
        light.setEnabled(on);
        if (on) light.setColor(255, 240, 200);
      }
    }
  }
}

Component.register(ProximityLighting, "ProximityLighting");
```

### Event-Driven Lighting

```typescript
import { Component, PropTypes } from "horizon/core";
import { DynamicLightGizmo } from "horizon/gizmos";

class EventLighting extends Component {
  static propsDefinition = {
    mainLight: { type: PropTypes.Entity },
  };

  start() {
    Horizon.NetworkEvent.on("gameEvent").add(({ eventType }) => {
      const light = this.props.mainLight.as(DynamicLightGizmo);
      if (!light) return;
      switch (eventType) {
        case "danger":
          light.setColor(255, 100, 100);
          break;
        case "victory":
          light.setColor(255, 255, 200);
          break;
        default:
          light.setColor(255, 255, 255);
      }
    });
  }
}

Component.register(EventLighting, "EventLighting");
```

---

## Environment Gizmo & Day/Night Transitions

Horizon Worlds does **not** support scripted sky or fog changes. Place **Environment Gizmos** manually:

- Add **Environment Gizmo** (Gizmos menu)
- Configure presets in **Properties Panel**: Daytime, Night, Sunrise, Sunset, Custom
- To simulate transitions, use multiple Environment Gizmos in trigger zones and enable/disable via scripts.

---

## Performance Constraints & Best Practices

- **20 Dynamic Light Gizmos** max per world
- Favor **Glow Materials** for static lighting
- Test with **Real-time Metrics** panel (Utilities → Real-time Metrics in VR; P key on Web)
- VR target: **72 FPS**, Web target: **60 FPS**
- Keep scripts lightweight; avoid heavy loops

---

## Troubleshooting

- **Lights Not Visible**: Ensure `.setEnabled(true)`, correct entity attachment, and sufficient intensity.
- **Harsh Shadows**: Add fill lights, reduce key light intensity, or adjust shadow settings in Properties Panel.
- **Performance Drops**: Reduce dynamic lights, disable shadows on non-critical lights, minimize fog density.

---

## Resources

- Official Lighting Guide: https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/lighting
- Performance Best Practices: https://developers.meta.com/horizon-worlds/learn/documentation/performance-best-practices-and-tooling
- Gizmo Reference: https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/gizmos

---

**Ready to illuminate your worlds?** Start with three-point lighting, incorporate interactive triggers, and always test performance in VR with the Real-time Metrics panel.

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/47767225/f4555cc7-892e-4f35-8427-6d00d93a8783/mastering-horizon-worlds-lighting-complete-guide.md)
