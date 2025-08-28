# Mastering Horizon Worlds Lighting: From Basic Gizmos to Cinematic Atmospheres

**Transform your worlds with professional lighting techniques that create mood, depth, and visual impact.** This comprehensive guide covers everything from basic lighting gizmos to advanced cinematic effects, helping you master the art of lighting in Meta Horizon Worlds. Whether you're creating a cozy interior scene or an epic outdoor environment, you'll learn how to use lighting to tell stories and enhance player immersion.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Basic understanding of the Horizon Worlds Desktop Editor and fundamental world-building concepts.

**Estimated Time to Complete**
2-3 hours for full tutorial, 30-45 minutes for basic lighting setup

## Table of Contents

1. [Understanding Lighting Fundamentals](#understanding-lighting-fundamentals)
2. [Lighting Gizmo Types and Properties](#lighting-gizmo-types-and-properties)
3. [Basic Lighting Setup](#basic-lighting-setup)
4. [Advanced Lighting Techniques](#advanced-lighting-techniques)
5. [Dynamic Lighting Systems](#dynamic-lighting-systems)
6. [Day/Night Cycle Implementation](#daynight-cycle-implementation)
7. [Performance Optimization](#performance-optimization)
8. [Cinematic Lighting Scenarios](#cinematic-lighting-scenarios)
9. [Troubleshooting Common Issues](#troubleshooting-common-issues)

## Understanding Lighting Fundamentals

Lighting is one of the most powerful tools in your world-building arsenal. It can transform a simple scene into something magical, guide player attention, and create emotional responses. In Horizon Worlds, lighting works through a combination of gizmos and environmental settings.

### The Three Pillars of Good Lighting

**1. Key Light (Primary Light Source)**
- The main light that illuminates your scene
- Usually the brightest and most directional
- Creates the primary shadows and highlights

**2. Fill Light (Secondary Light Source)**
- Softer, less intense light that fills in shadows
- Reduces harsh contrast and reveals detail
- Often positioned opposite to the key light

**3. Rim Light (Backlight)**
- Light positioned behind subjects
- Creates separation from the background
- Adds depth and dimension to objects

![Lighting Fundamentals Diagram - Three-point lighting setup showing key, fill, and rim lights positioned around a central object]

### Color Temperature and Mood

Understanding color temperature helps you create the right atmosphere:

- **Warm Light (2000K-4000K)**: Creates cozy, intimate, or romantic feelings
- **Neutral Light (4000K-5500K)**: Natural, balanced, and comfortable
- **Cool Light (5500K-10000K)**: Creates tension, mystery, or clinical environments

![Color Temperature Chart - Visual representation of warm, neutral, and cool lighting temperatures]

### Lighting Theory in Practice

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

## Lighting Gizmo Types and Properties

Horizon Worlds offers several types of lighting gizmos, each with specific use cases and properties.

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

**Technical Implementation:**
```typescript
class DirectionalLightController extends Horizon.Component {
    private lightGizmo: Horizon.DirectionalLightGizmo;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.DirectionalLightGizmo);
        this.setupLighting();
    }
    
    private setupLighting() {
        // Configure for outdoor daylight
        this.lightGizmo.setIntensity(5.0);
        this.lightGizmo.setColor(255, 248, 220); // Warm daylight
        this.lightGizmo.setCastShadows(true);
        this.lightGizmo.setShadowDistance(100);
        this.lightGizmo.setShadowResolution(2048);
        this.lightGizmo.setShadowBias(0.001);
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

**Advanced Point Light Setup:**
```typescript
class PointLightController extends Horizon.Component {
    private lightGizmo: Horizon.PointLightGizmo;
    private flickerEnabled: boolean = false;
    
    override start() {
        this.lightGizmo = this.props.light.as(Horizon.PointLightGizmo);
        this.setupPointLight();
    }
    
    private setupPointLight() {
        // Interior lighting setup
        this.lightGizmo.setIntensity(3.0);
        this.lightGizmo.setRange(8.0);
        this.lightGizmo.setColor(255, 240, 200); // Warm interior light
        this.lightGizmo.setCastShadows(true);
        this.lightGizmo.setAttenuation(1.0); // Linear falloff
        
        // Optional flicker effect for candles/fire
        if (this.flickerEnabled) {
            this.startFlickerEffect();
        }
    }
    
    private startFlickerEffect() {
        setInterval(() => {
            const flicker = 0.8 + (Math.random() * 0.4); // 0.8 to 1.2 intensity
            this.lightGizmo.setIntensity(3.0 * flicker);
        }, 100);
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

**Stage Lighting Implementation:**
```typescript
class StageLightingController extends Horizon.Component {
    private spotLight: Horizon.SpotLightGizmo;
    private targetEntity: Horizon.Entity;
    
    override start() {
        this.spotLight = this.props.light.as(Horizon.SpotLightGizmo);
        this.targetEntity = this.props.target;
        this.setupStageLight();
    }
    
    private setupStageLight() {
        // Stage lighting configuration
        this.spotLight.setIntensity(4.0);
        this.spotLight.setRange(12.0);
        this.spotLight.setAngle(25); // 25-degree cone
        this.spotLight.setInnerAngle(15); // Soft edges
        this.spotLight.setColor(255, 255, 255);
        this.spotLight.setCastShadows(true);
        
        // Follow target
        this.startTargetTracking();
    }
    
    private startTargetTracking() {
        setInterval(() => {
            if (this.targetEntity) {
                const targetPos = this.targetEntity.getPosition();
                const lightPos = this.spotLight.getPosition();
                const direction = targetPos.subtract(lightPos).normalize();
                
                // Point light at target
                this.spotLight.setRotation(
                    Math.atan2(direction.y, Math.sqrt(direction.x * direction.x + direction.z * direction.z)) * 180 / Math.PI,
                    Math.atan2(direction.x, direction.z) * 180 / Math.PI,
                    0
                );
            }
        }, 100);
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

**Window Lighting Setup:**
```typescript
class WindowLightingController extends Horizon.Component {
    private areaLight: Horizon.AreaLightGizmo;
    private timeOfDay: number = 0.5; // 0-1 range
    
    override start() {
        this.areaLight = this.props.light.as(Horizon.AreaLightGizmo);
        this.setupWindowLight();
    }
    
    private setupWindowLight() {
        // Window lighting configuration
        this.areaLight.setIntensity(2.5);
        this.areaLight.setSize(3.0, 2.0); // 3m wide, 2m tall
        this.areaLight.setShape(Horizon.AreaLightShape.Rectangle);
        this.areaLight.setSoftness(0.8);
        this.areaLight.setCastShadows(true);
        
        // Update based on time of day
        this.updateTimeOfDay();
    }
    
    private updateTimeOfDay() {
        setInterval(() => {
            this.timeOfDay += 0.001; // Slow progression
            if (this.timeOfDay > 1.0) this.timeOfDay = 0.0;
            
            // Adjust color and intensity based on time
            if (this.timeOfDay < 0.25) { // Morning
                this.areaLight.setColor(255, 240, 200); // Warm morning
                this.areaLight.setIntensity(2.0);
            } else if (this.timeOfDay < 0.75) { // Day
                this.areaLight.setColor(255, 255, 255); // Bright daylight
                this.areaLight.setIntensity(3.0);
            } else { // Evening
                this.areaLight.setColor(255, 200, 150); // Warm sunset
                this.areaLight.setIntensity(1.5);
            }
        }, 1000);
    }
}
```

## Basic Lighting Setup

Let's start with a simple three-point lighting setup that you can use as a foundation for any scene.

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

## Advanced Lighting Techniques

Once you've mastered basic lighting, these advanced techniques will help you create more sophisticated and cinematic scenes.

### Atmospheric Lighting

Create depth and mood with atmospheric effects:

1. **Add Atmospheric Fog**
   - Go to **World Settings > Environment**
   - Enable **Atmospheric Fog**
   - Set **Density** to **0.1-0.3** for subtle effect
   - Adjust **Color** to match your lighting scheme

![Atmospheric Fog Settings - Screenshot showing atmospheric fog configuration in world settings]

2. **Configure Fog Properties**
   - **Start Distance**: Where fog begins (usually 10-50 meters)
   - **End Distance**: Where fog becomes fully opaque
   - **Height**: How high the fog extends

**Pro Tip**: Use atmospheric fog to create depth and hide distant objects that might impact performance.

### Bloom and Glow Effects

Add cinematic bloom effects to bright light sources:

1. **Enable Bloom**
   - Go to **World Settings > Post Processing**
   - Enable **Bloom**
   - Set **Intensity** to **0.5-1.0** for subtle effect
   - Adjust **Threshold** to control what brightness triggers bloom

![Bloom Settings - Screenshot showing bloom configuration in post processing settings]

2. **Optimize Bloom Performance**
   - Use **Lower Intensity** values for better performance
   - **Disable Bloom** on mobile-optimized worlds
   - Test bloom effects in VR for realistic appearance

### Volumetric Lighting

Create visible light beams and atmospheric effects:

1. **Set Up Volumetric Light Source**
   - Use **Spot Light** with narrow angle (10-20 degrees)
   - Enable **Cast Shadows** for beam definition
   - Position light to create visible beam paths

2. **Add Atmospheric Particles**
   - Use **Particle System Gizmo** for dust/fog particles
   - Configure particles to be visible in light beams
   - Adjust particle density and size for realistic effect

![Volumetric Lighting Setup - Screenshot showing spot light creating visible light beam with particle effects]

## Dynamic Lighting Systems

Create interactive and responsive lighting that adapts to gameplay and player actions.

### Interactive Lighting Triggers

**Proximity-Based Lighting:**
```typescript
class ProximityLighting extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private triggerZone: Horizon.TriggerGizmo;
    private isActive: boolean = false;
    
    override start() {
        this.setupProximityLighting();
    }
    
    private setupProximityLighting() {
        // Get all lights in the scene
        this.lights = this.getLightsInScene();
        
        // Set up trigger zone
        this.triggerZone = this.props.trigger.as(Horizon.TriggerGizmo);
        this.triggerZone.onTriggerEnter.add((player) => {
            this.activateLights();
        });
        
        this.triggerZone.onTriggerExit.add((player) => {
            this.deactivateLights();
        });
    }
    
    private activateLights() {
        if (this.isActive) return;
        this.isActive = true;
        
        this.lights.forEach((light, index) => {
            // Staggered activation for dramatic effect
            setTimeout(() => {
                light.setIntensity(light.getIntensity() * 2);
                light.setColor(255, 255, 200); // Warm activation color
            }, index * 200);
        });
    }
    
    private deactivateLights() {
        if (!this.isActive) return;
        this.isActive = false;
        
        this.lights.forEach((light) => {
            light.setIntensity(light.getIntensity() * 0.5);
            light.setColor(255, 255, 255); // Return to normal
        });
    }
}
```

### Event-Driven Lighting

**Lighting for Game Events:**
```typescript
class EventLightingController extends Horizon.Component {
    private mainLight: Horizon.DirectionalLightGizmo;
    private emergencyLights: Horizon.SpotLightGizmo[] = [];
    private currentEvent: string = "normal";
    
    override start() {
        this.mainLight = this.props.mainLight.as(Horizon.DirectionalLightGizmo);
        this.emergencyLights = this.getEmergencyLights();
        this.setupEventListeners();
    }
    
    private setupEventListeners() {
        // Listen for game events
        Horizon.NetworkEvent.on("gameEvent").add((eventData) => {
            this.handleGameEvent(eventData.eventType);
        });
    }
    
    private handleGameEvent(eventType: string) {
        switch (eventType) {
            case "danger":
                this.activateDangerLighting();
                break;
            case "victory":
                this.activateVictoryLighting();
                break;
            case "mystery":
                this.activateMysteryLighting();
                break;
            default:
                this.resetToNormalLighting();
        }
    }
    
    private activateDangerLighting() {
        // Red emergency lighting
        this.mainLight.setIntensity(0.5);
        this.mainLight.setColor(255, 100, 100);
        
        this.emergencyLights.forEach((light, index) => {
            setTimeout(() => {
                light.setIntensity(3.0);
                light.setColor(255, 0, 0);
                light.setAngle(45);
            }, index * 100);
        });
    }
    
    private activateVictoryLighting() {
        // Bright, celebratory lighting
        this.mainLight.setIntensity(8.0);
        this.mainLight.setColor(255, 255, 200);
        
        this.emergencyLights.forEach((light, index) => {
            setTimeout(() => {
                light.setIntensity(2.0);
                light.setColor(255, 255, 0); // Golden celebration
                light.setAngle(60);
            }, index * 150);
        });
    }
}
```

## Day/Night Cycle Implementation

Create dynamic day/night cycles that affect your entire world's lighting system.

### Complete Day/Night System

```typescript
class DayNightCycle extends Horizon.Component {
    private sunLight: Horizon.DirectionalLightGizmo;
    private moonLight: Horizon.DirectionalLightGizmo;
    private ambientLights: Horizon.LightGizmo[] = [];
    private timeOfDay: number = 0.5; // 0 = midnight, 0.5 = noon, 1 = midnight
    private cycleSpeed: number = 0.001; // How fast time progresses
    
    override start() {
        this.sunLight = this.props.sunLight.as(Horizon.DirectionalLightGizmo);
        this.moonLight = this.props.moonLight.as(Horizon.DirectionalLightGizmo);
        this.ambientLights = this.getAmbientLights();
        this.startDayNightCycle();
    }
    
    private startDayNightCycle() {
        setInterval(() => {
            this.timeOfDay += this.cycleSpeed;
            if (this.timeOfDay >= 1.0) this.timeOfDay = 0.0;
            
            this.updateLighting();
        }, 100);
    }
    
    private updateLighting() {
        // Calculate sun and moon positions
        const sunAngle = Math.sin(this.timeOfDay * Math.PI * 2) * 45;
        const moonAngle = Math.sin((this.timeOfDay + 0.5) * Math.PI * 2) * 45;
        
        // Update sun lighting
        this.updateSunLight(sunAngle);
        
        // Update moon lighting
        this.updateMoonLight(moonAngle);
        
        // Update ambient lighting
        this.updateAmbientLighting();
        
        // Update atmospheric effects
        this.updateAtmosphericEffects();
    }
    
    private updateSunLight(sunAngle: number) {
        this.sunLight.setRotation(0, sunAngle, 0);
        
        if (sunAngle > -10 && sunAngle < 10) { // Daytime
            const intensity = Math.max(0.1, Math.cos(sunAngle * Math.PI / 180) * 5);
            this.sunLight.setIntensity(intensity);
            
            if (sunAngle > -5 && sunAngle < 5) { // Peak daylight
                this.sunLight.setColor(255, 255, 255);
            } else { // Morning/evening
                this.sunLight.setColor(255, 200, 150);
            }
        } else { // Nighttime
            this.sunLight.setIntensity(0.1);
            this.sunLight.setColor(100, 100, 150);
        }
    }
    
    private updateMoonLight(moonAngle: number) {
        this.moonLight.setRotation(0, moonAngle, 0);
        
        if (moonAngle > -10 && moonAngle < 10) { // Moon visible
            const intensity = Math.max(0.05, Math.cos(moonAngle * Math.PI / 180) * 0.5);
            this.moonLight.setIntensity(intensity);
            this.moonLight.setColor(150, 150, 255);
        } else {
            this.moonLight.setIntensity(0.05);
        }
    }
    
    private updateAmbientLighting() {
        const isDay = this.timeOfDay > 0.25 && this.timeOfDay < 0.75;
        
        this.ambientLights.forEach(light => {
            if (isDay) {
                light.setIntensity(light.getIntensity() * 1.5);
                light.setColor(255, 255, 255);
            } else {
                light.setIntensity(light.getIntensity() * 0.3);
                light.setColor(100, 100, 200);
            }
        });
    }
    
    private updateAtmosphericEffects() {
        const worldSettings = Horizon.WorldSettings.getInstance();
        
        if (this.timeOfDay > 0.25 && this.timeOfDay < 0.75) { // Day
            worldSettings.setAtmosphericFogDensity(0.05);
            worldSettings.setAtmosphericFogColor(200, 220, 255);
        } else { // Night
            worldSettings.setAtmosphericFogDensity(0.15);
            worldSettings.setAtmosphericFogColor(50, 50, 100);
        }
    }
}
```

### Weather-Based Lighting

**Dynamic Weather Effects:**
```typescript
class WeatherLightingController extends Horizon.Component {
    private currentWeather: string = "clear";
    private weatherIntensity: number = 0.0; // 0-1
    private baseLighting: Horizon.DirectionalLightGizmo;
    
    override start() {
        this.baseLighting = this.props.baseLight.as(Horizon.DirectionalLightGizmo);
        this.startWeatherSystem();
    }
    
    private startWeatherSystem() {
        // Simulate weather changes
        setInterval(() => {
            if (Math.random() < 0.01) { // 1% chance per second
                this.changeWeather();
            }
        }, 1000);
    }
    
    private changeWeather() {
        const weatherTypes = ["clear", "cloudy", "rainy", "stormy"];
        this.currentWeather = weatherTypes[Math.floor(Math.random() * weatherTypes.length)];
        this.weatherIntensity = Math.random();
        
        this.updateWeatherLighting();
    }
    
    private updateWeatherLighting() {
        switch (this.currentWeather) {
            case "clear":
                this.baseLighting.setIntensity(5.0);
                this.baseLighting.setColor(255, 255, 255);
                break;
                
            case "cloudy":
                const cloudIntensity = 5.0 - (this.weatherIntensity * 2.0);
                this.baseLighting.setIntensity(cloudIntensity);
                this.baseLighting.setColor(200, 200, 220);
                break;
                
            case "rainy":
                this.baseLighting.setIntensity(2.0);
                this.baseLighting.setColor(150, 150, 180);
                break;
                
            case "stormy":
                this.baseLighting.setIntensity(1.0);
                this.baseLighting.setColor(100, 100, 120);
                break;
        }
    }
}
```

## Performance Optimization

Lighting can significantly impact performance. Here are essential optimization techniques.

### Light Count Management

**Recommended Limits:**
- **Directional Lights**: 1-2 per scene
- **Point Lights**: 5-10 per scene
- **Spot Lights**: 3-8 per scene
- **Area Lights**: 2-5 per scene

![Light Count Dashboard - Screenshot showing performance metrics with current light count and recommendations]

### Shadow Optimization

1. **Limit Shadow-Casting Lights**
   - Only enable shadows on primary light sources
   - Use **Shadow Distance** to limit shadow rendering
   - Consider **Shadow Resolution** settings for performance

2. **Optimize Shadow Settings**
   - **Soft Shadows**: Disable for better performance
   - **Shadow Distance**: Keep under 100 meters
   - **Shadow Resolution**: Use lower settings for distant objects

![Shadow Settings Panel - Screenshot showing shadow configuration options with performance recommendations]

### Mobile Optimization

1. **Reduce Light Complexity**
   - Use fewer lights on mobile-optimized worlds
   - Disable atmospheric effects on mobile
   - Simplify lighting setups for better performance

2. **LOD for Lighting**
   - Create different lighting setups for different detail levels
   - Use **World Streaming** to load lights progressively
   - Optimize lighting based on player distance

![Mobile Lighting Comparison - Screenshot showing desktop vs mobile lighting setups with performance differences]

### Advanced Performance Techniques

**Light Culling System:**
```typescript
class LightCullingSystem extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private player: Horizon.Player;
    private cullingDistance: number = 50.0;
    
    override start() {
        this.lights = this.getLightsInScene();
        this.player = Horizon.Player.getLocalPlayer();
        this.startCulling();
    }
    
    private startCulling() {
        setInterval(() => {
            this.updateLightCulling();
        }, 500); // Check every 500ms
    }
    
    private updateLightCulling() {
        if (!this.player) return;
        
        const playerPos = this.player.getPosition();
        
        this.lights.forEach(light => {
            const lightPos = light.getPosition();
            const distance = playerPos.distance(lightPos);
            
            if (distance > this.cullingDistance) {
                // Disable distant lights
                light.setIntensity(0);
            } else {
                // Enable nearby lights with distance-based intensity
                const intensityFactor = 1.0 - (distance / this.cullingDistance);
                light.setIntensity(light.getIntensity() * intensityFactor);
            }
        });
    }
}
```

## Cinematic Lighting Scenarios

Apply these lighting setups to create specific moods and atmospheres.

### Cozy Interior Lighting

**Perfect For**: Homes, cafes, intimate spaces

**Setup:**
1. **Warm Point Lights** (intensity 2-3) for lamps and fixtures
2. **Soft Fill Light** (intensity 1) from windows
3. **Atmospheric Fog** (density 0.1) for warmth
4. **Color Temperature**: Warm whites (2500K-3500K)

![Cozy Interior Example - Screenshot showing warm, intimate interior lighting setup]

### Dramatic Night Scene

**Perfect For**: Mystery, horror, dramatic storytelling

**Setup:**
1. **Single Spot Light** (intensity 3-4) for dramatic key light
2. **Minimal Fill Light** (intensity 0.5) for shadow detail
3. **Cool Color Temperature** (4000K-5000K)
4. **High Contrast** with deep shadows

![Dramatic Night Scene - Screenshot showing high-contrast night lighting with dramatic shadows]

### Bright Outdoor Daylight

**Perfect For**: Adventure, exploration, cheerful environments

**Setup:**
1. **Strong Directional Light** (intensity 5-7) for sun
2. **Sky Fill Light** (intensity 2-3) for ambient illumination
3. **Neutral Color Temperature** (5500K-6500K)
4. **Atmospheric Fog** (density 0.05) for depth

![Bright Outdoor Scene - Screenshot showing natural daylight lighting with sky illumination]

### Sci-Fi/Neon Lighting

**Perfect For**: Futuristic, cyberpunk, high-tech environments

**Setup:**
1. **Colored Spot Lights** (various colors) for neon effects
2. **High Contrast** with dark backgrounds
3. **Bloom Effects** for glowing elements
4. **Cool Color Palette** (blues, purples, cyans)

![Sci-Fi Neon Lighting - Screenshot showing colorful neon lighting with bloom effects]

## Troubleshooting Common Issues

### Light Not Visible

**Possible Causes:**
- Light intensity too low
- Light positioned outside scene bounds
- Light gizmo not properly configured

**Solutions:**
1. Check light intensity (should be 0.1-10)
2. Verify light position within scene
3. Ensure gizmo is properly attached to entity

![Light Visibility Debug - Screenshot showing light gizmo properties with troubleshooting checklist]

### Shadows Too Harsh

**Possible Causes:**
- Single light source without fill
- High contrast between light and shadow
- Shadow distance too short

**Solutions:**
1. Add fill light to soften shadows
2. Reduce key light intensity
3. Increase shadow distance
4. Use area lights for softer shadows

![Shadow Softening Setup - Screenshot showing before/after comparison of harsh vs soft shadows]

### Performance Issues

**Possible Causes:**
- Too many lights in scene
- Shadows enabled on all lights
- High atmospheric effects

**Solutions:**
1. Reduce total light count
2. Disable shadows on secondary lights
3. Lower atmospheric fog density
4. Use LOD for lighting complexity

![Performance Optimization Panel - Screenshot showing performance metrics and optimization recommendations]

### Color Temperature Issues

**Possible Causes:**
- Inconsistent color temperatures
- Wrong color for intended mood
- Poor color balance between lights

**Solutions:**
1. Use consistent color temperature across lights
2. Match color to intended atmosphere
3. Balance warm and cool lights appropriately
4. Test colors in VR for accurate perception

![Color Temperature Guide - Screenshot showing color temperature chart with mood associations]

## Best Practices Summary

### Do's ✅
- Start with three-point lighting as a foundation
- Use appropriate light types for your scene
- Optimize for performance from the beginning
- Test lighting in VR for accurate perception
- Use color temperature to enhance mood
- Enable shadows only on primary light sources

### Don'ts ❌
- Overuse bright lights (causes eye strain)
- Ignore performance implications
- Use too many shadow-casting lights
- Forget to test on mobile devices
- Use inconsistent color temperatures
- Neglect atmospheric effects for depth

## Resources and References

### Official Documentation
- **[Horizon Worlds Lighting Guide](https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/lighting)**
- **[Performance Best Practices](https://developers.meta.com/horizon-worlds/learn/documentation/performance-best-practices-and-tooling)**
- **[Gizmo Reference](https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/gizmos)**

### Additional Learning
- **Lighting Theory**: Study real-world lighting principles
- **Cinematography**: Learn from film and photography techniques
- **Color Theory**: Understand how colors affect mood and perception

### Community Resources
- **Horizon Worlds Creator Community**: Share lighting setups and techniques
- **Discord Servers**: Join creator communities for lighting discussions
- **YouTube Channels**: Watch lighting tutorials and showcases

---

**Ready to Illuminate Your Worlds?** 

Start with the basic three-point lighting setup and gradually incorporate advanced techniques. Remember, great lighting is often subtle - it should enhance your world without drawing attention to itself. Practice these techniques, experiment with different setups, and most importantly, test everything in VR to see how it feels to players.

**Next Steps:**
1. Set up a basic three-point lighting system in your world
2. Experiment with different light types and properties
3. Try creating one of the cinematic lighting scenarios
4. Optimize your lighting for performance
5. Share your lighting setups with the community!

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*

