# Advanced Lighting Techniques: Atmospheric Effects and Cinematic Lighting

**Take your lighting to the next level with advanced techniques that create depth, mood, and cinematic impact.** This guide covers atmospheric effects, bloom, volumetric lighting, and other advanced techniques that will make your worlds stand out.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Understanding of basic lighting fundamentals and three-point lighting.

**Estimated Time to Complete**
1-2 hours for advanced techniques, 30-45 minutes for atmospheric effects

## Table of Contents

1. [Atmospheric Lighting Effects](#atmospheric-lighting-effects)
2. [Bloom and Glow Effects](#bloom-and-glow-effects)
3. [Volumetric Lighting](#volumetric-lighting)
4. [Advanced Light Properties](#advanced-light-properties)
5. [Cinematic Lighting Techniques](#cinematic-lighting-techniques)

## Atmospheric Lighting Effects

Atmospheric effects add depth, mood, and realism to your scenes. They can create everything from subtle depth cues to dramatic fog banks.

### Atmospheric Fog

Atmospheric fog is one of the most powerful tools for creating depth and mood in your worlds. It simulates particles in the air that scatter light.

**Setting Up Atmospheric Fog:**

1. **Access World Settings**
   - Go to **World Settings > Environment**
   - Enable **Atmospheric Fog**

2. **Configure Fog Properties**
   - **Density**: Controls how thick the fog is (0.0-1.0)
   - **Color**: The color of the fog particles
   - **Start Distance**: Where fog begins to appear
   - **End Distance**: Where fog becomes fully opaque
   - **Height**: How high the fog extends

![Atmospheric Fog Settings - Screenshot showing atmospheric fog configuration in world settings]

**Atmospheric Fog Implementation:**
```typescript
class AtmosphericFogController extends Horizon.Component {
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.setupAtmosphericFog();
    }
    
    private setupAtmosphericFog() {
        // Enable atmospheric fog
        this.worldSettings.setAtmosphericFogEnabled(true);
        
        // Configure for subtle depth
        this.worldSettings.setAtmosphericFogDensity(0.1);
        this.worldSettings.setAtmosphericFogColor(200, 220, 255);
        this.worldSettings.setAtmosphericFogStartDistance(20);
        this.worldSettings.setAtmosphericFogEndDistance(100);
        this.worldSettings.setAtmosphericFogHeight(50);
    }
    
    private setDramaticFog() {
        // Thick, dramatic fog
        this.worldSettings.setAtmosphericFogDensity(0.4);
        this.worldSettings.setAtmosphericFogColor(100, 100, 120);
        this.worldSettings.setAtmosphericFogStartDistance(5);
        this.worldSettings.setAtmosphericFogEndDistance(30);
    }
    
    private setSubtleFog() {
        // Subtle depth fog
        this.worldSettings.setAtmosphericFogDensity(0.05);
        this.worldSettings.setAtmosphericFogColor(220, 230, 255);
        this.worldSettings.setAtmosphericFogStartDistance(50);
        this.worldSettings.setAtmosphericFogEndDistance(200);
    }
}
```

### Fog Types and Applications

**Depth Fog:**
- **Purpose**: Creates depth perception and hides distant objects
- **Density**: 0.05-0.15
- **Color**: Slightly blue-tinted (220, 230, 255)
- **Use Case**: Large outdoor scenes, performance optimization

**Mood Fog:**
- **Purpose**: Creates atmosphere and mood
- **Density**: 0.2-0.4
- **Color**: Varies by mood (warm for cozy, cool for mysterious)
- **Use Case**: Horror games, dramatic scenes, mood setting

**Weather Fog:**
- **Purpose**: Simulates weather conditions
- **Density**: 0.3-0.6
- **Color**: Gray or white (180, 180, 200)
- **Use Case**: Rainy scenes, overcast weather, mystery

### Dynamic Atmospheric Effects

**Time-Based Fog Changes:**
```typescript
class DynamicAtmosphericController extends Horizon.Component {
    private worldSettings: Horizon.WorldSettings;
    private timeOfDay: number = 0.5;
    
    override start() {
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.startDynamicAtmosphere();
    }
    
    private startDynamicAtmosphere() {
        setInterval(() => {
            this.timeOfDay += 0.001;
            if (this.timeOfDay > 1.0) this.timeOfDay = 0.0;
            
            this.updateAtmosphericEffects();
        }, 1000);
    }
    
    private updateAtmosphericEffects() {
        if (this.timeOfDay < 0.25) { // Morning
            this.worldSettings.setAtmosphericFogDensity(0.15);
            this.worldSettings.setAtmosphericFogColor(255, 240, 220);
        } else if (this.timeOfDay < 0.75) { // Day
            this.worldSettings.setAtmosphericFogDensity(0.05);
            this.worldSettings.setAtmosphericFogColor(220, 230, 255);
        } else { // Evening/Night
            this.worldSettings.setAtmosphericFogDensity(0.25);
            this.worldSettings.setAtmosphericFogColor(100, 100, 150);
        }
    }
}
```

## Bloom and Glow Effects

Bloom effects create realistic light bleeding and glow around bright light sources, adding cinematic quality to your scenes.

### Understanding Bloom

**What is Bloom?**
- Bloom simulates how bright light sources appear to "bleed" into surrounding areas
- It mimics the way cameras and human eyes handle bright light
- Creates a more realistic and cinematic appearance

**Bloom Properties:**
- **Intensity**: How strong the bloom effect is
- **Threshold**: How bright a pixel needs to be to trigger bloom
- **Radius**: How far the bloom effect spreads
- **Color**: The color tint of the bloom effect

### Setting Up Bloom

1. **Access Post Processing**
   - Go to **World Settings > Post Processing**
   - Enable **Bloom**

2. **Configure Bloom Settings**
   - **Intensity**: Start with 0.5-1.0 for subtle effect
   - **Threshold**: Set to 0.8-1.0 to control what triggers bloom
   - **Radius**: Adjust based on your scene size

![Bloom Settings - Screenshot showing bloom configuration in post processing settings]

**Bloom Implementation:**
```typescript
class BloomController extends Horizon.Component {
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.setupBloom();
    }
    
    private setupBloom() {
        // Enable bloom
        this.worldSettings.setBloomEnabled(true);
        
        // Subtle bloom for natural lighting
        this.worldSettings.setBloomIntensity(0.6);
        this.worldSettings.setBloomThreshold(0.8);
        this.worldSettings.setBloomRadius(2.0);
    }
    
    private setCinematicBloom() {
        // Strong bloom for dramatic effect
        this.worldSettings.setBloomIntensity(1.2);
        this.worldSettings.setBloomThreshold(0.6);
        this.worldSettings.setBloomRadius(3.0);
    }
    
    private setNeonBloom() {
        // Colored bloom for neon effects
        this.worldSettings.setBloomIntensity(0.8);
        this.worldSettings.setBloomThreshold(0.7);
        this.worldSettings.setBloomRadius(2.5);
        this.worldSettings.setBloomColor(100, 150, 255); // Blue tint
    }
}
```

### Bloom Optimization

**Performance Considerations:**
- Higher bloom intensity and radius impact performance
- Use lower settings on mobile devices
- Consider disabling bloom for distant objects

**Mobile Optimization:**
```typescript
class MobileBloomOptimizer extends Horizon.Component {
    private worldSettings: Horizon.WorldSettings;
    private isMobileDevice: boolean;
    
    override start() {
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.isMobileDevice = this.detectMobileDevice();
        this.optimizeForDevice();
    }
    
    private optimizeForDevice() {
        if (this.isMobileDevice) {
            // Reduced bloom for mobile
            this.worldSettings.setBloomIntensity(0.3);
            this.worldSettings.setBloomRadius(1.0);
        } else {
            // Full bloom for desktop/VR
            this.worldSettings.setBloomIntensity(0.8);
            this.worldSettings.setBloomRadius(2.5);
        }
    }
}
```

## Volumetric Lighting

Volumetric lighting creates visible light beams and atmospheric effects that add dramatic impact to your scenes.

### Understanding Volumetric Lighting

**What is Volumetric Lighting?**
- Creates visible light beams in the air
- Simulates light scattering through particles
- Adds dramatic, cinematic quality to scenes

**Key Components:**
- **Light Source**: Usually a spot light with narrow angle
- **Atmospheric Particles**: Dust, fog, or smoke particles
- **Light Scattering**: How light interacts with particles

### Setting Up Volumetric Lighting

**Step 1: Create the Light Source**
```typescript
class VolumetricLightSource extends Horizon.Component {
    private spotLight: Horizon.SpotLightGizmo;
    
    override start() {
        this.spotLight = this.props.light.as(Horizon.SpotLightGizmo);
        this.setupVolumetricLight();
    }
    
    private setupVolumetricLight() {
        // Configure spot light for volumetric effect
        this.spotLight.setIntensity(3.0);
        this.spotLight.setRange(15.0);
        this.spotLight.setAngle(15); // Narrow beam
        this.spotLight.setInnerAngle(10); // Sharp edges
        this.spotLight.setColor(255, 255, 255);
        this.spotLight.setCastShadows(true);
        
        // Enable volumetric rendering
        this.spotLight.setVolumetricEnabled(true);
        this.spotLight.setVolumetricIntensity(0.8);
    }
}
```

**Step 2: Add Atmospheric Particles**
```typescript
class VolumetricParticleSystem extends Horizon.Component {
    private particleSystem: Horizon.ParticleSystemGizmo;
    
    override start() {
        this.particleSystem = this.props.particles.as(Horizon.ParticleSystemGizmo);
        this.setupVolumetricParticles();
    }
    
    private setupVolumetricParticles() {
        // Configure particles for volumetric lighting
        this.particleSystem.setParticleCount(1000);
        this.particleSystem.setParticleSize(0.1);
        this.particleSystem.setParticleColor(255, 255, 255);
        this.particleSystem.setParticleOpacity(0.3);
        
        // Enable light interaction
        this.particleSystem.setLightInteractionEnabled(true);
        this.particleSystem.setVolumetricScattering(0.5);
    }
}
```

### Advanced Volumetric Techniques

**Multiple Light Beams:**
```typescript
class MultiBeamVolumetric extends Horizon.Component {
    private lightBeams: Horizon.SpotLightGizmo[] = [];
    private particleSystems: Horizon.ParticleSystemGizmo[] = [];
    
    override start() {
        this.setupMultipleBeams();
    }
    
    private setupMultipleBeams() {
        // Create multiple volumetric light beams
        for (let i = 0; i < 5; i++) {
            const light = this.createVolumetricLight(i);
            const particles = this.createParticleSystem(i);
            
            this.lightBeams.push(light);
            this.particleSystems.push(particles);
        }
    }
    
    private createVolumetricLight(index: number): Horizon.SpotLightGizmo {
        const light = this.props[`light${index}`].as(Horizon.SpotLightGizmo);
        
        // Stagger light positions and angles
        const angle = (index * 72) + Math.random() * 20; // 72 degrees apart
        const height = 10 + (index * 2);
        
        light.setPosition(0, height, 0);
        light.setRotation(0, angle, 0);
        light.setIntensity(2.0 + Math.random() * 2.0);
        light.setAngle(20 + Math.random() * 10);
        light.setVolumetricEnabled(true);
        
        return light;
    }
}
```

**Dynamic Volumetric Effects:**
```typescript
class DynamicVolumetricController extends Horizon.Component {
    private volumetricLights: Horizon.SpotLightGizmo[] = [];
    private time: number = 0;
    
    override start() {
        this.volumetricLights = this.getVolumetricLights();
        this.startDynamicEffects();
    }
    
    private startDynamicEffects() {
        setInterval(() => {
            this.time += 0.1;
            this.updateVolumetricEffects();
        }, 100);
    }
    
    private updateVolumetricEffects() {
        this.volumetricLights.forEach((light, index) => {
            // Animate light intensity
            const intensity = 2.0 + Math.sin(this.time + index) * 0.5;
            light.setIntensity(intensity);
            
            // Animate light color
            const hue = (this.time * 50 + index * 60) % 360;
            const color = this.hsvToRgb(hue, 0.8, 1.0);
            light.setColor(color.r, color.g, color.b);
        });
    }
    
    private hsvToRgb(h: number, s: number, v: number) {
        // Convert HSV to RGB (simplified)
        const c = v * s;
        const x = c * (1 - Math.abs((h / 60) % 2 - 1));
        const m = v - c;
        
        let r = 0, g = 0, b = 0;
        if (h < 60) { r = c; g = x; b = 0; }
        else if (h < 120) { r = x; g = c; b = 0; }
        else if (h < 180) { r = 0; g = c; b = x; }
        else if (h < 240) { r = 0; g = x; b = c; }
        else if (h < 300) { r = x; g = 0; b = c; }
        else { r = c; g = 0; b = x; }
        
        return {
            r: Math.round((r + m) * 255),
            g: Math.round((g + m) * 255),
            b: Math.round((b + m) * 255)
        };
    }
}
```

## Advanced Light Properties

Beyond basic intensity and color, lights have many advanced properties that can create sophisticated effects.

### Shadow Properties

**Shadow Quality Settings:**
```typescript
class AdvancedShadowController extends Horizon.Component {
    private directionalLight: Horizon.DirectionalLightGizmo;
    
    override start() {
        this.directionalLight = this.props.light.as(Horizon.DirectionalLightGizmo);
        this.setupAdvancedShadows();
    }
    
    private setupAdvancedShadows() {
        // High-quality shadows
        this.directionalLight.setCastShadows(true);
        this.directionalLight.setShadowDistance(100);
        this.directionalLight.setShadowResolution(2048);
        this.directionalLight.setShadowBias(0.001);
        this.directionalLight.setShadowNormalBias(0.01);
        this.directionalLight.setShadowSoftness(0.5);
    }
    
    private setSoftShadows() {
        // Soft, natural shadows
        this.directionalLight.setShadowSoftness(1.0);
        this.directionalLight.setShadowResolution(1024);
    }
    
    private setHardShadows() {
        // Sharp, dramatic shadows
        this.directionalLight.setShadowSoftness(0.0);
        this.directionalLight.setShadowResolution(4096);
    }
}
```

### Light Cookies and Patterns

**Custom Light Patterns:**
```typescript
class LightCookieController extends Horizon.Component {
    private spotLight: Horizon.SpotLightGizmo;
    
    override start() {
        this.spotLight = this.props.light.as(Horizon.SpotLightGizmo);
        this.setupLightCookies();
    }
    
    private setupLightCookies() {
        // Set custom light pattern
        this.spotLight.setCookieTexture("window_pattern");
        this.spotLight.setCookieIntensity(1.0);
        this.spotLight.setCookieRotation(0);
        this.spotLight.setCookieScale(1.0, 1.0);
    }
    
    private setWindowPattern() {
        // Simulate light through window bars
        this.spotLight.setCookieTexture("window_bars");
        this.spotLight.setCookieIntensity(0.8);
    }
    
    private setTreePattern() {
        // Simulate light through tree leaves
        this.spotLight.setCookieTexture("tree_shadows");
        this.spotLight.setCookieIntensity(0.6);
    }
}
```

### Light Attenuation and Falloff

**Custom Light Falloff:**
```typescript
class CustomFalloffController extends Horizon.Component {
    private pointLight: Horizon.PointLightGizmo;
    
    override start() {
        this.pointLight = this.props.light.as(Horizon.PointLightGizmo);
        this.setupCustomFalloff();
    }
    
    private setupCustomFalloff() {
        // Linear falloff (natural)
        this.pointLight.setAttenuation(1.0);
        this.pointLight.setFalloffType(Horizon.LightFalloffType.Linear);
        
        // Custom falloff curve
        this.pointLight.setCustomFalloffCurve([
            { distance: 0, intensity: 1.0 },
            { distance: 2, intensity: 0.8 },
            { distance: 5, intensity: 0.5 },
            { distance: 10, intensity: 0.0 }
        ]);
    }
    
    private setInverseSquareFalloff() {
        // Realistic inverse square falloff
        this.pointLight.setFalloffType(Horizon.LightFalloffType.InverseSquare);
        this.pointLight.setAttenuation(2.0);
    }
}
```

## Cinematic Lighting Techniques

Create professional, cinematic lighting that tells stories and guides player attention.

### High-Key vs Low-Key Lighting

**High-Key Lighting (Bright, Low Contrast):**
```typescript
class HighKeyLighting extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    
    override start() {
        this.lights = this.getLightsInScene();
        this.setupHighKeyLighting();
    }
    
    private setupHighKeyLighting() {
        // Bright, even lighting with minimal shadows
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 1.5);
            
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowSoftness(1.0);
                light.setShadowDistance(50);
            }
        });
        
        // Add fill lights to reduce shadows
        this.addFillLights();
    }
    
    private addFillLights() {
        // Add multiple soft fill lights
        for (let i = 0; i < 4; i++) {
            const fillLight = this.createFillLight(i);
            fillLight.setIntensity(1.0);
            fillLight.setColor(255, 255, 255);
            fillLight.setCastShadows(false);
        }
    }
}
```

**Low-Key Lighting (Dark, High Contrast):**
```typescript
class LowKeyLighting extends Horizon.Component {
    private keyLight: Horizon.SpotLightGizmo;
    private ambientLight: Horizon.DirectionalLightGizmo;
    
    override start() {
        this.keyLight = this.props.keyLight.as(Horizon.SpotLightGizmo);
        this.ambientLight = this.props.ambientLight.as(Horizon.DirectionalLightGizmo);
        this.setupLowKeyLighting();
    }
    
    private setupLowKeyLighting() {
        // Strong key light with minimal fill
        this.keyLight.setIntensity(5.0);
        this.keyLight.setAngle(20);
        this.keyLight.setColor(255, 255, 255);
        this.keyLight.setCastShadows(true);
        
        // Very low ambient light
        this.ambientLight.setIntensity(0.2);
        this.ambientLight.setColor(50, 50, 100);
        this.ambientLight.setCastShadows(false);
    }
}
```

### Rembrandt Lighting

**Classic Rembrandt Triangle:**
```typescript
class RembrandtLighting extends Horizon.Component {
    private keyLight: Horizon.DirectionalLightGizmo;
    private fillLight: Horizon.PointLightGizmo;
    
    override start() {
        this.keyLight = this.props.keyLight.as(Horizon.DirectionalLightGizmo);
        this.fillLight = this.props.fillLight.as(Horizon.PointLightGizmo);
        this.setupRembrandtLighting();
    }
    
    private setupRembrandtLighting() {
        // Key light at 45-degree angle
        this.keyLight.setRotation(45, 45, 0);
        this.keyLight.setIntensity(4.0);
        this.keyLight.setColor(255, 248, 220);
        this.keyLight.setCastShadows(true);
        this.keyLight.setShadowSoftness(0.3);
        
        // Minimal fill light
        this.fillLight.setIntensity(0.8);
        this.fillLight.setColor(200, 210, 255);
        this.fillLight.setRange(8.0);
        this.fillLight.setCastShadows(false);
    }
}
```

### Split Lighting

**Dramatic Split Lighting:**
```typescript
class SplitLighting extends Horizon.Component {
    private leftLight: Horizon.SpotLightGizmo;
    private rightLight: Horizon.SpotLightGizmo;
    
    override start() {
        this.leftLight = this.props.leftLight.as(Horizon.SpotLightGizmo);
        this.rightLight = this.props.rightLight.as(Horizon.SpotLightGizmo);
        this.setupSplitLighting();
    }
    
    private setupSplitLighting() {
        // Left side lighting
        this.leftLight.setRotation(0, -90, 0);
        this.leftLight.setIntensity(3.0);
        this.leftLight.setColor(255, 200, 150); // Warm
        this.leftLight.setAngle(30);
        this.leftLight.setCastShadows(true);
        
        // Right side lighting
        this.rightLight.setRotation(0, 90, 0);
        this.rightLight.setIntensity(2.0);
        this.rightLight.setColor(150, 200, 255); // Cool
        this.rightLight.setAngle(30);
        this.rightLight.setCastShadows(true);
    }
}
```

### Practical Lighting Examples

**Film Noir Style:**
```typescript
class FilmNoirLighting extends Horizon.Component {
    private keyLight: Horizon.SpotLightGizmo;
    private rimLight: Horizon.SpotLightGizmo;
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.keyLight = this.props.keyLight.as(Horizon.SpotLightGizmo);
        this.rimLight = this.props.rimLight.as(Horizon.SpotLightGizmo);
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.setupFilmNoirLighting();
    }
    
    private setupFilmNoirLighting() {
        // Strong, dramatic key light
        this.keyLight.setIntensity(6.0);
        this.keyLight.setAngle(15);
        this.keyLight.setColor(255, 255, 255);
        this.keyLight.setCastShadows(true);
        this.keyLight.setShadowSoftness(0.0);
        
        // Subtle rim light
        this.rimLight.setIntensity(1.5);
        this.rimLight.setAngle(45);
        this.rimLight.setColor(100, 100, 150);
        this.rimLight.setCastShadows(false);
        
        // Dark atmospheric fog
        this.worldSettings.setAtmosphericFogEnabled(true);
        this.worldSettings.setAtmosphericFogDensity(0.3);
        this.worldSettings.setAtmosphericFogColor(50, 50, 80);
    }
}
```

**Sci-Fi Lighting:**
```typescript
class SciFiLighting extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.lights = this.getLightsInScene();
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.setupSciFiLighting();
    }
    
    private setupSciFiLighting() {
        // Colored accent lights
        this.lights.forEach((light, index) => {
            const colors = [
                [100, 150, 255], // Blue
                [255, 100, 150], // Pink
                [150, 255, 100], // Green
                [255, 200, 100]  // Orange
            ];
            
            const color = colors[index % colors.length];
            light.setColor(color[0], color[1], color[2]);
            light.setIntensity(2.0 + Math.random() * 2.0);
        });
        
        // Strong bloom effect
        this.worldSettings.setBloomEnabled(true);
        this.worldSettings.setBloomIntensity(1.2);
        this.worldSettings.setBloomThreshold(0.6);
        
        // Cool atmospheric fog
        this.worldSettings.setAtmosphericFogEnabled(true);
        this.worldSettings.setAtmosphericFogDensity(0.2);
        this.worldSettings.setAtmosphericFogColor(100, 120, 200);
    }
}
```

## Next Steps

Now that you've mastered advanced lighting techniques, explore:

1. **Dynamic Lighting Systems**: Interactive and responsive lighting
2. **Day/Night Cycles**: Creating dynamic time-based lighting
3. **Performance Optimization**: Advanced optimization techniques
4. **Lighting for Specific Genres**: Horror, adventure, puzzle games

---

**Ready to Create Cinematic Lighting?**

Experiment with these advanced techniques to create unique atmospheres. Remember to balance visual impact with performance, and always test your lighting in VR to ensure it works well for players.

**Practice Exercises:**
1. Create a dramatic scene using low-key lighting
2. Set up volumetric lighting with multiple beams
3. Implement atmospheric fog for mood
4. Design a sci-fi environment with colored lighting

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
