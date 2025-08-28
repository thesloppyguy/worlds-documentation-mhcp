# Horizon Worlds Lighting Guide: Accurate Implementation

**Create effective lighting in your Horizon Worlds with the tools actually available.** This guide covers the real lighting capabilities, proper implementation methods, and practical techniques that work in Horizon Worlds.

**Creator Skill Level**
Beginner to Intermediate

**Recommended Background Knowledge**
Basic understanding of Horizon Worlds building and TypeScript fundamentals.

**Estimated Time to Complete**
45-60 minutes for core concepts, 30 minutes for advanced techniques

## Table of Contents

1. [Understanding Horizon Worlds Lighting](#understanding-horizon-worlds-lighting)
2. [Environment Gizmo for Atmosphere](#environment-gizmo-for-atmosphere)
3. [Dynamic Light Gizmo](#dynamic-light-gizmo)
4. [Static Lighting with Glow Materials](#static-lighting-with-glow-materials)
5. [Practical Lighting Techniques](#practical-lighting-techniques)
6. [TypeScript Implementation](#typescript-implementation)
7. [Performance Considerations](#performance-considerations)

## Understanding Horizon Worlds Lighting

Horizon Worlds provides three main ways to create lighting effects in your worlds:

### **Available Lighting Methods:**

1. **Environment Gizmo**: Controls overall world lighting, fog, and atmosphere
2. **Dynamic Light Gizmo**: Interactive lights with scripting capabilities (limited to 20 per world)
3. **Glow Materials**: Unlimited static lighting using material properties

### **Important Limitations:**

- **No volumetric lighting** or visible light beams
- **No bloom post-processing** controls via scripting
- **No light cookies** or projection patterns
- **No WorldSettings API** - use Environment Gizmo instead
- **No atmospheric fog API** - controlled through Environment Gizmo properties panel

## Environment Gizmo for Atmosphere

The Environment Gizmo is your primary tool for setting the overall mood and atmosphere of your world.

### **Setting Up Environment Lighting:**

1. **Add Environment Gizmo**

   - Go to **Gizmos** menu
   - Select **Environment Gizmo**
   - Place in your world

2. **Configure Environment Properties**
   - **Skydome**: Choose from preset environments (Daytime, Night, Sunrise, etc.)
   - **Fog Density**: Adjust atmospheric fog (0.0-1.0, use very low values like 0.1)
   - **Custom Gradient**: Create custom sky colors
   - **Grid Visibility**: Show/hide floor grid

### **Environment Presets Available:**

- **Daytime**: Bright, natural lighting
- **Night**: Dark atmosphere with subtle ambient light
- **Sunrise/Sunset**: Warm, golden lighting
- **Various themed environments**: Space, underwater, fantasy settings

### **Fog Density Guidelines:**

- **0.0**: Crystal clear visibility
- **0.01-0.05**: Subtle depth enhancement
- **0.1**: Noticeable atmospheric effect
- **0.2+**: Heavy fog (use sparingly)

**Important**: Fog density is extremely sensitive. Values above 0.1 can make building difficult as it affects visibility in build mode.

## Dynamic Light Gizmo

Dynamic lights provide interactive, scriptable lighting with real-time shadows and color changes.

### **Dynamic Light Properties:**

- **Type**: Point light (360°) or Spot light (directional)
- **Color**: Full RGB color range
- **Intensity**: Light brightness
- **Falloff Distance**: How far light travels
- **Spread** (Spot lights only): Cone angle of light beam

### **Critical Limitation:**

**Maximum 20 dynamic lights per world**. Plan your lighting carefully as this limit includes all dynamic lights, whether scripted or not.

### **Setting Up Dynamic Lights:**

1. **Add Dynamic Light Gizmo**

   - Go to **Gizmos** menu
   - Select **Dynamic Light Gizmo**
   - Position where needed

2. **Configure Properties**

   - Open **Properties Panel**
   - Adjust **Intensity** (brightness)
   - Set **Falloff Distance** (range)
   - Change **Type** to Point or Spot
   - For Spot lights, adjust **Spread** angle

3. **Change Color**
   - Use **Paint Tool**
   - Point at the light's color indicator
   - Squeeze trigger to apply new color

## Static Lighting with Glow Materials

For unlimited lighting effects, use the glow material on objects.

### **Creating Glow Objects:**

1. **Apply Glow Material**

   - Select object
   - Open **Style** panel
   - Choose **Glow** material
   - Adjust **Glow Intensity** slider

2. **Optimize Glow Lighting**
   - **Scale affects intensity**: Larger objects emit more light
   - **Color selection**: Choose appropriate colors for your scene
   - **Invisible glow**: Turn off visibility while keeping light emission

### **Making Invisible Light Sources:**

```
1. Create object with glow material
2. Open Properties Panel
3. Turn OFF "Visibility"
4. Keep "Light and Shadow" ON
5. Object now emits light but is invisible
```

**Note**: Glow objects lose light-emitting properties when animated. Use Dynamic Light Gizmos for moving lights.

## Practical Lighting Techniques

### **Three-Point Lighting Setup:**

**Key Light (Primary):**

- Use Dynamic Light Gizmo (Spot light)
- High intensity (main illumination)
- Position at 45° angle from subject

**Fill Light (Secondary):**

- Use glow object or second Dynamic Light
- Lower intensity than key light
- Opposite side of key light

**Rim Light (Accent):**

- Use Dynamic Light Gizmo (Spot light)
- Behind subject, pointing toward camera
- Creates outline/separation

### **Creating Mood with Environment:**

**Horror/Dark Scenes:**

- Environment: Night preset
- Fog density: 0.05-0.1
- Minimal dynamic lights with cool colors
- Use invisible glow objects for subtle ambient lighting

**Cozy/Warm Scenes:**

- Environment: Sunrise/Sunset preset
- Low fog density: 0.02-0.05
- Warm-colored glow objects (oranges, yellows)
- Multiple small light sources

**Sci-Fi Scenes:**

- Environment: Space or custom gradient
- Higher fog density: 0.1-0.15
- Bright colored dynamic lights (blues, purples, greens)
- Animated color changes via scripting

### **Performance-Friendly Lighting:**

1. **Use glow materials** for most static lighting needs
2. **Reserve dynamic lights** for interactive elements only
3. **Group invisible glow objects** for easy management
4. **Keep fog density low** to maintain performance
5. **Test in VR** to ensure comfort

## TypeScript Implementation

Here's how to properly script lighting in Horizon Worlds:

### **Basic Dynamic Light Control:**

```typescript
import { Component, PropTypes } from "horizon/core";

class LightController extends Component {
  static propsDefinition = {
    dynamicLight: { type: PropTypes.Entity },
  };

  start() {
    // Light is configured through Properties Panel
    // Script can control on/off and color
  }

  onPlayerEnterTrigger() {
    // Turn light on
    this.props.dynamicLight.as(DynamicLightGizmo)?.setEnabled(true);
  }

  onPlayerExitTrigger() {
    // Turn light off
    this.props.dynamicLight.as(DynamicLightGizmo)?.setEnabled(false);
  }
}

Component.register(LightController, "LightController");
```

### **Animated Color Changing:**

```typescript
import { Component, PropTypes } from "horizon/core";

class ColorCycleLight extends Component {
  static propsDefinition = {
    light: { type: PropTypes.Entity },
  };

  private hue: number = 0;

  start() {
    // Start color cycling
    this.connectBroadcastEvent("loop", this.updateColor);
    this.sendBroadcastEvent("loop");
  }

  private updateColor = () => {
    const light = this.props.light.as(DynamicLightGizmo);
    if (light) {
      // Convert HSV to RGB for smooth color transitions
      const rgb = this.hsvToRgb(this.hue, 1.0, 1.0);
      light.setColor(rgb.r, rgb.g, rgb.b);

      this.hue = (this.hue + 0.01) % 1.0;
    }

    // Continue loop
    this.async.setTimeout(() => {
      this.sendBroadcastEvent("loop");
    }, 100);
  };

  private hsvToRgb(h: number, s: number, v: number) {
    const i = Math.floor(h * 6);
    const f = h * 6 - i;
    const p = v * (1 - s);
    const q = v * (1 - f * s);
    const t = v * (1 - (1 - f) * s);

    let r: number, g: number, b: number;

    switch (i % 6) {
      case 0:
        r = v;
        g = t;
        b = p;
        break;
      case 1:
        r = q;
        g = v;
        b = p;
        break;
      case 2:
        r = p;
        g = v;
        b = t;
        break;
      case 3:
        r = p;
        g = q;
        b = v;
        break;
      case 4:
        r = t;
        g = p;
        b = v;
        break;
      case 5:
        r = v;
        g = p;
        b = q;
        break;
      default:
        r = g = b = 0;
    }

    return {
      r: Math.round(r * 255),
      g: Math.round(g * 255),
      b: Math.round(b * 255),
    };
  }
}

Component.register(ColorCycleLight, "ColorCycleLight");
```

### **Interactive Torch/Flashlight:**

```typescript
import { Component, PropTypes } from "horizon/core";

class InteractiveTorch extends Component {
  static propsDefinition = {
    torchLight: { type: PropTypes.Entity },
  };

  private isLit: boolean = true;

  onPlayerEnterTrigger() {
    // Toggle torch on/off
    this.isLit = !this.isLit;
    this.props.torchLight.as(DynamicLightGizmo)?.setEnabled(this.isLit);
  }
}

Component.register(InteractiveTorch, "InteractiveTorch");
```

## Performance Considerations

### **Dynamic Light Budget:**

- **20 lights maximum** per world
- Reserve for interactive elements only
- Use glow materials for ambient lighting
- Group management for complex scenes

### **Fog Performance:**

- Keep fog density **below 0.1** for optimal performance
- Higher fog affects build mode visibility
- Test on Quest devices for mobile optimization

### **VR Comfort:**

- Avoid **rapidly flashing lights** (seizure risk)
- Test color intensity in VR headsets
- Ensure adequate contrast for readability
- Consider colorblind accessibility

### **Optimization Tips:**

1. **Use invisible glow objects** instead of multiple dynamic lights
2. **Group related lighting objects** for easier management
3. **Turn off "Light and Shadow"** on decorative objects when possible
4. **Test lighting in published world** before finalizing

## Common Mistakes to Avoid

1. **Trying to use fictional APIs** from other engines
2. **Exceeding the 20 dynamic light limit**
3. **Setting fog density too high** (makes building impossible)
4. **Using animated glow objects** (they lose lighting properties)
5. **Not testing in VR** for comfort and visibility

## Next Steps

Now that you understand Horizon Worlds lighting:

1. **Practice with Environment Gizmo** presets and fog settings
2. **Create simple dynamic light interactions** with TypeScript
3. **Experiment with glow materials** for ambient lighting
4. **Build complete lighting scenes** using all three methods
5. **Test your worlds in VR** for optimal player experience

---

**Ready to Light Your World?**

Remember: Horizon Worlds lighting is about working within the platform's capabilities rather than trying to replicate advanced rendering features from other engines. Focus on creative use of the available tools to create compelling atmospheres and interactive experiences.

**Practice Exercises:**

1. Create a campfire scene using glow materials and dynamic lights
2. Build a light switch system with TypeScript
3. Design a mood-changing room with environment presets
4. Make an interactive torch for exploration gameplay

---

_This guide reflects the actual capabilities of Meta Horizon Worlds as of 2025. For the latest updates and features, visit the [official Horizon Worlds documentation](https://developers.meta.com/horizon-worlds/)._
