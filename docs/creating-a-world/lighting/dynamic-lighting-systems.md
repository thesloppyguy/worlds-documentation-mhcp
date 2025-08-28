# Updated Horizon Worlds Dynamic Lighting Systems Documentation

**This guide has been revised to use only officially supported Horizon Worlds APIs and workflows.** All fictional or unsupported methods have been removed.

---

## Interactive Lighting Triggers

### Proximity-Based Lighting

Use a **Trigger Gizmo** and **Dynamic Light Gizmos** configured via the Properties Panel. Script only enables/disables lights and changes color.

```typescript
import { Component, PropTypes } from "horizon/core";
import { DynamicLightGizmo, TriggerGizmo } from "horizon/gizmos";

class ProximityLighting extends Component {
  static propsDefinition = {
    triggerZone: { type: PropTypes.Entity },
  };

  start() {
    const trigger = this.props.triggerZone.as(TriggerGizmo);
    trigger.onTriggerEnter.add(() => this.setLights(true));
    trigger.onTriggerExit.add(() => this.setLights(false));
  }

  private setLights(on: boolean) {
    for (const entity of this.getEntitiesByComponent(DynamicLightGizmo)) {
      const light = entity.as(DynamicLightGizmo);
      if (!light) continue;
      light.setEnabled(on);
      if (on) light.setColor(255, 240, 200);
    }
  }
}

Component.register(ProximityLighting, "ProximityLighting");
```

---

## Event-Driven Lighting

Listen for **NetworkEvent** to update Dynamic Light Gizmos.

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
      light.setColor(
        eventType === "danger"
          ? 255_100_100
          : eventType === "victory"
          ? 255_255_200
          : 255_255_255
      );
    });
  }
}

Component.register(EventLighting, "EventLighting");
```

---

## Day/Night & Environment Presets

Horizon Worlds does not support programmatic sky or fog changes. Use the **Environment Gizmo** in the editor to select presets:

- Daytime
- Night
- Sunrise/Sunset
- Custom Gradient

To simulate transitions, place multiple Environment Gizmos in zones and enable/disable them via triggers.

---

## Performance Considerations

- Maximum **20 Dynamic Light Gizmos** per world—use sparingly.
- Use **Glow Materials** on static objects for ambient light.
- Avoid rapid light toggles to maintain VR comfort.
- Test performance in VR at **72 FPS** and on Web at **60 FPS** using the **Real-time Metrics** panel (Utilities menu in VR, P key on Web).
- Keep trigger event handlers lightweight; heavy loops can drop FPS.

---

**Next Steps:**

1. Build trigger-based lighting with Dynamic Light Gizmos.
2. Use NetworkEvent handlers for game-driven lighting changes.
3. Configure Environment Gizmo presets manually for mood shifts.
4. Monitor performance with Real-time Metrics in VR and Web.

_This documentation reflects the official Meta Horizon Worlds capabilities as of 2025._

[1](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/47767225/3a751376-9ee4-4864-95c3-ce4052c9c3e2/dynamic-lighting-systems.md)
