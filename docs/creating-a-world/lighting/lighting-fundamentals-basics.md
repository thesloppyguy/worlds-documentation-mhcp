# Lighting Fundamentals: Understanding Light in Horizon Worlds

**Master the foundational principles of lighting to create compelling, atmospheric worlds.** This guide covers the essential concepts of lighting theory, gizmo types, and basic setup techniques that form the foundation for all lighting work in Meta Horizon Worlds.

**Creator Skill Level**
Beginner to Intermediate

**Recommended Background Knowledge**
Basic understanding of the Horizon Worlds Desktop Editor.

**Estimated Time to Complete**
45-60 minutes for fundamentals, 30 minutes for basic setup

## Table of Contents

1. [Understanding Light Theory](#understanding-light-theory)
2. [Lighting Gizmo Types](#lighting-gizmo-types)
3. [Basic Three-Point Lighting](#basic-three-point-lighting)
4. [Color Temperature and Mood](#color-temperature-and-mood)
5. [Lighting Best Practices](#lighting-best-practices)

## Understanding Light Theory

Lighting is one of the most powerful tools in your world-building arsenal. It can transform a simple scene into something magical, guide player attention, and create emotional responses. Understanding how light works in the real world helps you create more convincing virtual environments.

### The Nature of Light

**Light Properties:**
- **Intensity**: How bright the light is
- **Color**: The wavelength and temperature of the light
- **Direction**: Where the light is coming from
- **Quality**: How hard or soft the light appears
- **Falloff**: How quickly light diminishes with distance

### Light Behavior in 3D Space

**Key Concepts:**
- **Attenuation**: Light gets weaker as it travels further
- **Diffusion**: Light scatters when it hits surfaces
- **Reflection**: Light bounces off surfaces
- **Shadows**: Areas where light is blocked
- **Ambient Light**: Indirect light that fills shadows

![Light Behavior Diagram - Visual representation of light attenuation, diffusion, and reflection]

### The Three Pillars of Good Lighting

**1. Key Light (Primary Light Source)**
- The main light that illuminates your scene
- Usually the brightest and most directional
- Creates the primary shadows and highlights
- Determines the overall mood and direction

**2. Fill Light (Secondary Light Source)**
- Softer, less intense light that fills in shadows
- Reduces harsh contrast and reveals detail
- Often positioned opposite to the key light
- Usually 1/3 to 1/2 the intensity of the key light

**3. Rim Light (Backlight)**
- Light positioned behind subjects
- Creates separation from the background
- Adds depth and dimension to objects
- Creates a subtle outline or "rim" around subjects

![Three-Point Lighting Diagram - Visual representation of key, fill, and rim lights positioned around a central object]

### Light Quality and Direction

**Light Quality:**
- **Hard Light**: Sharp, defined shadows (directional lights, spot lights)
- **Soft Light**: Diffuse, gradual shadows (area lights, bounced light)
- **Mixed Light**: Combination for natural appearance

**Light Direction:**
- **Front Lighting**: Flat, even illumination
- **Side Lighting**: Dramatic shadows and depth
- **Back Lighting**: Silhouettes and rim effects
- **Top Lighting**: Natural overhead illumination
- **Bottom Lighting**: Unnatural, dramatic effects

![Light Direction Examples - Screenshots showing different lighting directions and their effects]

## Lighting Gizmo Types

Horizon Worlds offers several types of lighting gizmos, each with specific use cases and properties. Understanding when to use each type is crucial for effective lighting.

### Directional Light Gizmo

**Best For**: Outdoor scenes, sun simulation, large area illumination

**Key Properties**:
- **Intensity**: Controls brightness (0-10 range recommended)
- **Color**: RGB color picker for light tint
- **Rotation**: Determines light direction and shadow casting
- **Cast Shadows**: Enable/disable shadow generation
- **Shadow Distance**: How far shadows are rendered
- **Shadow Resolution**: Quality of shadow rendering
- **Shadow Bias**: Prevents shadow acne and peter panning

![Directional Light Setup - Screenshot showing directional light gizmo properties panel with intensity, color, and shadow settings]

**Pro Tip**: Use directional lights for your primary light source in outdoor environments. Position them to simulate realistic sun angles for different times of day.

**Basic Directional Light Setup:**
```typescript
class BasicDirectionalLight extends Horizon.Component {
    private lightGizmo: Horizon.DirectionalLightGizmo;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.DirectionalLightGizmo);
        this.setupBasicLighting();
    }
    
    private setupBasicLighting() {
        // Basic outdoor lighting setup
        this.lightGizmo.setIntensity(5.0);
        this.lightGizmo.setColor(255, 248, 220); // Warm daylight
        this.lightGizmo.setCastShadows(true);
        this.lightGizmo.setShadowDistance(50);
    }
}
```

### Point Light Gizmo

**Best For**: Interior lighting, lamps, candles, localized illumination

**Key Properties**:
- **Intensity**: Brightness at the source
- **Range**: How far the light travels
- **Color**: Light color tint
- **Cast Shadows**: Shadow generation toggle
- **Attenuation**: How quickly light fades with distance
- **Falloff Type**: Linear, quadratic, or custom falloff

![Point Light Configuration - Screenshot showing point light gizmo with range visualization and attenuation settings]

**Pro Tip**: Point lights are perfect for creating intimate, focused lighting. Use multiple point lights with different ranges to create layered lighting effects.

**Basic Point Light Setup:**
```typescript
class BasicPointLight extends Horizon.Component {
    private lightGizmo: Horizon.PointLightGizmo;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.PointLightGizmo);
        this.setupPointLight();
    }
    
    private setupPointLight() {
        // Basic interior lighting setup
        this.lightGizmo.setIntensity(3.0);
        this.lightGizmo.setRange(8.0);
        this.lightGizmo.setColor(255, 240, 200); // Warm interior light
        this.lightGizmo.setCastShadows(true);
        this.lightGizmo.setAttenuation(1.0); // Linear falloff
    }
}
```

### Spot Light Gizmo

**Best For**: Focused illumination, stage lighting, dramatic effects

**Key Properties**:
- **Intensity**: Brightness at the source
- **Range**: Maximum light travel distance
- **Angle**: Cone of light spread (narrower = more focused)
- **Color**: Light color tint
- **Cast Shadows**: Shadow generation
- **Inner Angle**: Soft edge falloff
- **Cookie**: Custom light pattern projection

![Spot Light Setup - Screenshot showing spot light gizmo with cone visualization and angle controls]

**Pro Tip**: Spot lights are excellent for highlighting specific objects or creating dramatic stage lighting effects. Use narrow angles for focused beams and wider angles for broader illumination.

**Basic Spot Light Setup:**
```typescript
class BasicSpotLight extends Horizon.Component {
    private lightGizmo: Horizon.SpotLightGizmo;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.SpotLightGizmo);
        this.setupSpotLight();
    }
    
    private setupSpotLight() {
        // Basic spot lighting setup
        this.lightGizmo.setIntensity(4.0);
        this.lightGizmo.setRange(12.0);
        this.lightGizmo.setAngle(30); // 30-degree cone
        this.lightGizmo.setInnerAngle(20); // Soft edges
        this.lightGizmo.setColor(255, 255, 255);
        this.lightGizmo.setCastShadows(true);
    }
}
```

### Area Light Gizmo

**Best For**: Soft, diffused lighting, large surface illumination

**Key Properties**:
- **Intensity**: Overall brightness
- **Size**: Physical size of the light source
- **Color**: Light color tint
- **Cast Shadows**: Shadow generation
- **Shape**: Rectangular or circular light source
- **Softness**: Edge softness of the light

![Area Light Configuration - Screenshot showing area light gizmo with size controls and shape options]

**Pro Tip**: Area lights create the most natural, soft lighting. They're perfect for simulating light from windows, large light fixtures, or sky illumination.

**Basic Area Light Setup:**
```typescript
class BasicAreaLight extends Horizon.Component {
    private lightGizmo: Horizon.AreaLightGizmo;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.AreaLightGizmo);
        this.setupAreaLight();
    }
    
    private setupAreaLight() {
        // Basic area lighting setup
        this.lightGizmo.setIntensity(2.5);
        this.lightGizmo.setSize(3.0, 2.0); // 3m wide, 2m tall
        this.lightGizmo.setShape(Horizon.AreaLightShape.Rectangle);
        this.lightGizmo.setSoftness(0.8);
        this.lightGizmo.setColor(255, 255, 255);
        this.lightGizmo.setCastShadows(true);
    }
}
```

## Basic Three-Point Lighting

The three-point lighting system is the foundation of professional lighting. It provides a balanced, natural-looking illumination that works for most scenes.

### Step 1: Setting Up Your Key Light

1. **Add a Directional Light Gizmo**
   - In the Desktop Editor, go to **Shapes > Gizmos > Directional Light**
   - Position it above and slightly to one side of your main subject
   - Set intensity to **3-5** for a moderate key light

![Key Light Placement - Screenshot showing directional light positioned above and to the side of a central object]

2. **Configure Key Light Properties**
   - **Color**: Set to warm white (255, 248, 220) for natural lighting
   - **Cast Shadows**: Enable for realistic shadow generation
   - **Shadow Distance**: Set to **50-100** depending on scene size

### Step 2: Adding Fill Light

1. **Add a Point Light Gizmo**
   - Position it opposite to your key light
   - Set intensity to **1-2** (about 1/3 of key light intensity)
   - Place it slightly lower than the key light

![Fill Light Setup - Screenshot showing point light positioned opposite to key light with lower intensity]

2. **Configure Fill Light Properties**
   - **Color**: Use slightly cooler temperature (240, 245, 255)
   - **Range**: Set to cover the entire subject area
   - **Cast Shadows**: Usually disable for softer fill

### Step 3: Implementing Rim Light

1. **Add a Spot Light Gizmo**
   - Position it behind your subject, pointing toward the camera
   - Set intensity to **2-3** for subtle rim effect
   - Angle it to create a thin line of light around the subject

![Rim Light Configuration - Screenshot showing spot light positioned behind subject creating rim lighting effect]

2. **Configure Rim Light Properties**
   - **Angle**: Set to **15-25 degrees** for focused rim
   - **Color**: Use neutral white or slightly warm
   - **Cast Shadows**: Disable to avoid unwanted shadows

### Step 4: Testing Your Setup

1. **Enter Play Mode** to see your lighting in action
2. **Move around the scene** to view lighting from different angles
3. **Adjust intensities** based on how the scene looks in VR
4. **Fine-tune positions** for optimal shadow and highlight placement

![Three-Point Lighting Result - Screenshot showing final three-point lighting setup with balanced illumination]

**Complete Three-Point Lighting Script:**
```typescript
class ThreePointLighting extends Horizon.Component {
    private keyLight: Horizon.DirectionalLightGizmo;
    private fillLight: Horizon.PointLightGizmo;
    private rimLight: Horizon.SpotLightGizmo;
    
    override start() {
        this.keyLight = this.props.keyLight.as(Horizon.DirectionalLightGizmo);
        this.fillLight = this.props.fillLight.as(Horizon.PointLightGizmo);
        this.rimLight = this.props.rimLight.as(Horizon.SpotLightGizmo);
        
        this.setupThreePointLighting();
    }
    
    private setupThreePointLighting() {
        // Key Light Setup
        this.keyLight.setIntensity(4.0);
        this.keyLight.setColor(255, 248, 220);
        this.keyLight.setCastShadows(true);
        this.keyLight.setShadowDistance(75);
        
        // Fill Light Setup
        this.fillLight.setIntensity(1.5);
        this.fillLight.setColor(240, 245, 255);
        this.fillLight.setRange(10.0);
        this.fillLight.setCastShadows(false);
        
        // Rim Light Setup
        this.rimLight.setIntensity(2.5);
        this.rimLight.setColor(255, 255, 255);
        this.rimLight.setAngle(20);
        this.rimLight.setRange(8.0);
        this.rimLight.setCastShadows(false);
    }
}
```

## Color Temperature and Mood

Understanding color temperature helps you create the right atmosphere for your scenes. Color temperature is measured in Kelvin (K) and affects how warm or cool light appears.

### Color Temperature Scale

**Warm Light (2000K-4000K):**
- **2000K**: Candlelight, very warm and intimate
- **2500K**: Incandescent bulbs, cozy and comfortable
- **3000K**: Warm white, good for living spaces
- **4000K**: Cool white, neutral and balanced

**Neutral Light (4000K-5500K):**
- **4000K**: Cool white, neutral and balanced
- **5000K**: Daylight, natural and bright
- **5500K**: Pure daylight, crisp and clear

**Cool Light (5500K-10000K):**
- **5500K**: Pure daylight, crisp and clear
- **6500K**: Cool daylight, bright and energetic
- **7500K**: Overcast sky, soft and diffused
- **10000K**: Blue sky, very cool and clinical

![Color Temperature Chart - Visual representation of warm, neutral, and cool lighting temperatures]

### Mood and Atmosphere

**Warm Lighting (2000K-4000K):**
- **Mood**: Cozy, intimate, romantic, comfortable
- **Best For**: Homes, cafes, restaurants, bedrooms
- **Emotional Response**: Relaxation, warmth, comfort

**Neutral Lighting (4000K-5500K):**
- **Mood**: Natural, balanced, comfortable, professional
- **Best For**: Offices, retail spaces, outdoor scenes
- **Emotional Response**: Clarity, focus, naturalness

**Cool Lighting (5500K-10000K):**
- **Mood**: Tension, mystery, clinical, energetic
- **Best For**: Hospitals, sci-fi environments, dramatic scenes
- **Emotional Response**: Alertness, tension, mystery

### Practical Color Temperature Implementation

**Interior Lighting Setup:**
```typescript
class ColorTemperatureController extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private currentMood: string = "neutral";
    
    override start() {
        this.lights = this.getLightsInScene();
        this.setupColorTemperature();
    }
    
    private setupColorTemperature() {
        // Set up different mood lighting
        this.setMoodLighting("warm");
    }
    
    private setMoodLighting(mood: string) {
        this.currentMood = mood;
        
        switch (mood) {
            case "warm":
                this.lights.forEach(light => {
                    light.setColor(255, 240, 200); // 3000K equivalent
                });
                break;
                
            case "neutral":
                this.lights.forEach(light => {
                    light.setColor(255, 255, 255); // 5000K equivalent
                });
                break;
                
            case "cool":
                this.lights.forEach(light => {
                    light.setColor(200, 220, 255); // 6500K equivalent
                });
                break;
        }
    }
}
```

## Lighting Best Practices

Follow these fundamental principles to create effective lighting in your worlds.

### Do's ✅

**Start with Three-Point Lighting:**
- Use key, fill, and rim lights as your foundation
- Adjust ratios based on your scene needs
- Test from multiple angles

**Use Appropriate Light Types:**
- Directional lights for outdoor/sun simulation
- Point lights for interior/localized lighting
- Spot lights for focused illumination
- Area lights for soft, natural lighting

**Consider Color Temperature:**
- Match lighting to your scene's mood
- Use consistent color temperatures
- Avoid mixing very warm and very cool lights

**Test in VR:**
- Lighting looks different in VR than in the editor
- Check from player perspective
- Ensure comfortable viewing experience

**Optimize for Performance:**
- Limit the number of shadow-casting lights
- Use appropriate shadow distances
- Consider mobile device limitations

### Don'ts ❌

**Avoid Over-Lighting:**
- Too many lights create flat, uninteresting scenes
- Start with fewer lights and add as needed
- Let shadows create depth and interest

**Don't Ignore Shadows:**
- Shadows are crucial for depth and realism
- Use shadows to guide player attention
- Avoid completely shadowless scenes

**Avoid Inconsistent Lighting:**
- Don't mix dramatically different color temperatures
- Maintain consistent lighting direction
- Avoid random light placement

**Don't Forget Performance:**
- Too many lights impact frame rate
- Don't enable shadows on every light
- Consider LOD for distant lighting

### Common Lighting Mistakes

**Flat Lighting:**
- **Problem**: Scene appears flat and uninteresting
- **Solution**: Add shadows and vary light intensities
- **Fix**: Use three-point lighting system

**Harsh Shadows:**
- **Problem**: Shadows are too dark and sharp
- **Solution**: Add fill light or use softer light sources
- **Fix**: Reduce key light intensity, add fill light

**Inconsistent Color:**
- **Problem**: Mixed color temperatures look unnatural
- **Solution**: Use consistent color temperature
- **Fix**: Choose one temperature range and stick to it

**Poor Performance:**
- **Problem**: Too many lights causing lag
- **Solution**: Optimize light count and settings
- **Fix**: Reduce light count, disable unnecessary shadows

## Next Steps

Now that you understand the fundamentals of lighting, you're ready to explore more advanced techniques:

1. **Advanced Lighting Techniques**: Atmospheric effects, bloom, and volumetric lighting
2. **Dynamic Lighting Systems**: Interactive and responsive lighting
3. **Day/Night Cycles**: Creating dynamic time-based lighting
4. **Performance Optimization**: Advanced optimization techniques
5. **Cinematic Lighting**: Creating specific moods and atmospheres

---

**Ready to Illuminate Your Worlds?**

Start with the three-point lighting system and practice with different light types. Remember, good lighting is often subtle - it should enhance your world without drawing attention to itself. Test everything in VR to see how it feels to players.

**Practice Exercises:**
1. Set up three-point lighting in a simple scene
2. Experiment with different light types and properties
3. Try different color temperatures for different moods
4. Test your lighting from multiple angles in VR

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
