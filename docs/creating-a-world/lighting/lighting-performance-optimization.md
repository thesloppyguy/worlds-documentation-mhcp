# Lighting Performance Optimization: Maximizing Quality and Frame Rate

**Optimize your lighting systems for smooth performance across all devices while maintaining visual quality.** This guide covers advanced optimization techniques, performance monitoring, and troubleshooting common lighting issues.

**Creator Skill Level**
Advanced

**Recommended Background Knowledge**
Understanding of lighting fundamentals and basic optimization concepts.

**Estimated Time to Complete**
1-2 hours for optimization techniques, 30 minutes for troubleshooting

## Table of Contents

1. [Performance Monitoring](#performance-monitoring)
2. [Light Count Optimization](#light-count-optimization)
3. [Shadow Optimization](#shadow-optimization)
4. [Mobile Optimization](#mobile-optimization)
5. [Troubleshooting Common Issues](#troubleshooting-common-issues)

## Performance Monitoring

Monitor your lighting performance to identify bottlenecks and optimize effectively.

### Performance Metrics

**Key Metrics to Track:**
- **Frame Rate**: Target 60+ FPS on desktop, 30+ on mobile
- **Draw Calls**: Minimize lighting-related draw calls
- **Shadow Rendering**: Monitor shadow map generation time
- **Light Count**: Track active lights in scene
- **Memory Usage**: Monitor lighting system memory consumption

**Performance Monitoring Script:**
```typescript
class LightingPerformanceMonitor extends Horizon.Component {
    private frameCount: number = 0;
    private lastTime: number = Date.now();
    private fps: number = 0;
    private lightCount: number = 0;
    private shadowCount: number = 0;
    
    override start() {
        this.startPerformanceMonitoring();
    }
    
    private startPerformanceMonitoring() {
        setInterval(() => {
            this.updatePerformanceMetrics();
        }, 1000); // Update every second
    }
    
    private updatePerformanceMetrics() {
        // Calculate FPS
        const currentTime = Date.now();
        const deltaTime = currentTime - this.lastTime;
        this.fps = Math.round(1000 / deltaTime);
        this.lastTime = currentTime;
        
        // Count lights and shadows
        this.lightCount = this.countActiveLights();
        this.shadowCount = this.countShadowCastingLights();
        
        // Log performance data
        this.logPerformanceData();
    }
    
    private countActiveLights(): number {
        const lights = this.getLightsInScene();
        return lights.filter(light => light.getIntensity() > 0).length;
    }
    
    private countShadowCastingLights(): number {
        const lights = this.getLightsInScene();
        return lights.filter(light => light.getCastShadows()).length;
    }
    
    private logPerformanceData() {
        console.log(`Performance: FPS=${this.fps}, Lights=${this.lightCount}, Shadows=${this.shadowCount}`);
        
        // Warn if performance is poor
        if (this.fps < 30) {
            console.warn("Low frame rate detected. Consider reducing light count or shadow quality.");
        }
        
        if (this.lightCount > 20) {
            console.warn("High light count detected. Consider light culling or LOD.");
        }
    }
}
```

## Light Count Optimization

Manage the number of active lights to maintain performance.

### Light Culling System

**Distance-Based Culling:**
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

### Light LOD System

**Level of Detail for Lights:**
```typescript
class LightLODSystem extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private lodDistances: number[] = [20, 35, 50]; // Near, medium, far
    
    override start() {
        this.lights = this.getLightsInScene();
        this.startLODUpdates();
    }
    
    private startLODUpdates() {
        setInterval(() => {
            this.updateLightLOD();
        }, 1000); // Update every second
    }
    
    private updateLightLOD() {
        const player = Horizon.Player.getLocalPlayer();
        if (!player) return;
        
        const playerPos = player.getPosition();
        
        this.lights.forEach(light => {
            const distance = playerPos.distance(light.getPosition());
            const lodLevel = this.getLODLevel(distance);
            this.applyLODToLight(light, lodLevel);
        });
    }
    
    private getLODLevel(distance: number): number {
        if (distance <= this.lodDistances[0]) return 0; // High detail
        if (distance <= this.lodDistances[1]) return 1; // Medium detail
        if (distance <= this.lodDistances[2]) return 2; // Low detail
        return 3; // Disabled
    }
    
    private applyLODToLight(light: Horizon.LightGizmo, lodLevel: number) {
        switch (lodLevel) {
            case 0: // High detail
                light.setIntensity(light.getIntensity());
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(2048);
                    light.setShadowSoftness(0.5);
                }
                break;
                
            case 1: // Medium detail
                light.setIntensity(light.getIntensity() * 0.8);
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(1024);
                    light.setShadowSoftness(0.8);
                }
                break;
                
            case 2: // Low detail
                light.setIntensity(light.getIntensity() * 0.5);
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(512);
                    light.setShadowSoftness(1.0);
                }
                break;
                
            case 3: // Disabled
                light.setIntensity(0);
                break;
        }
    }
}
```

## Shadow Optimization

Shadows are often the most performance-intensive aspect of lighting.

### Shadow Quality Management

**Adaptive Shadow Quality:**
```typescript
class ShadowOptimizer extends Horizon.Component {
    private shadowCastingLights: Horizon.LightGizmo[] = [];
    private performanceMode: string = "balanced";
    
    override start() {
        this.shadowCastingLights = this.getShadowCastingLights();
        this.setupShadowOptimization();
    }
    
    private setupShadowOptimization() {
        // Monitor performance and adjust shadow quality
        setInterval(() => {
            this.updateShadowQuality();
        }, 2000); // Check every 2 seconds
    }
    
    private updateShadowQuality() {
        const fps = this.getCurrentFPS();
        
        if (fps < 30) {
            this.setLowQualityShadows();
        } else if (fps < 45) {
            this.setMediumQualityShadows();
        } else {
            this.setHighQualityShadows();
        }
    }
    
    private setLowQualityShadows() {
        this.shadowCastingLights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowResolution(512);
                light.setShadowDistance(30);
                light.setShadowSoftness(1.0);
            }
        });
    }
    
    private setMediumQualityShadows() {
        this.shadowCastingLights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowResolution(1024);
                light.setShadowDistance(50);
                light.setShadowSoftness(0.7);
            }
        });
    }
    
    private setHighQualityShadows() {
        this.shadowCastingLights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowResolution(2048);
                light.setShadowDistance(100);
                light.setShadowSoftness(0.3);
            }
        });
    }
    
    private getCurrentFPS(): number {
        // Simplified FPS calculation
        return 60; // Placeholder
    }
}
```

### Shadow Culling

**Smart Shadow Management:**
```typescript
class ShadowCullingSystem extends Horizon.Component {
    private shadowLights: Horizon.LightGizmo[] = [];
    private maxShadowLights: number = 4; // Limit shadow-casting lights
    
    override start() {
        this.shadowLights = this.getShadowCastingLights();
        this.optimizeShadowLights();
    }
    
    private optimizeShadowLights() {
        // Sort lights by importance (distance to player, intensity)
        this.shadowLights.sort((a, b) => {
            const aImportance = this.calculateLightImportance(a);
            const bImportance = this.calculateLightImportance(b);
            return bImportance - aImportance; // Descending order
        });
        
        // Enable shadows only on the most important lights
        this.shadowLights.forEach((light, index) => {
            if (index < this.maxShadowLights) {
                light.setCastShadows(true);
            } else {
                light.setCastShadows(false);
            }
        });
    }
    
    private calculateLightImportance(light: Horizon.LightGizmo): number {
        const player = Horizon.Player.getLocalPlayer();
        if (!player) return 0;
        
        const distance = player.getPosition().distance(light.getPosition());
        const intensity = light.getIntensity();
        
        // Closer and brighter lights are more important
        return intensity / (distance + 1);
    }
}
```

## Mobile Optimization

Mobile devices require special optimization strategies.

### Mobile-Specific Optimizations

**Mobile Lighting Controller:**
```typescript
class MobileLightingOptimizer extends Horizon.Component {
    private isMobileDevice: boolean;
    private lights: Horizon.LightGizmo[] = [];
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.isMobileDevice = this.detectMobileDevice();
        this.lights = this.getLightsInScene();
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.optimizeForDevice();
    }
    
    private detectMobileDevice(): boolean {
        // Detect mobile device (simplified)
        return Horizon.DeviceInfo.getDeviceType() === "mobile";
    }
    
    private optimizeForDevice() {
        if (this.isMobileDevice) {
            this.applyMobileOptimizations();
        } else {
            this.applyDesktopOptimizations();
        }
    }
    
    private applyMobileOptimizations() {
        // Reduce light count
        this.lights.forEach((light, index) => {
            if (index > 8) { // Limit to 8 lights on mobile
                light.setIntensity(0);
            } else {
                light.setIntensity(light.getIntensity() * 0.8);
            }
        });
        
        // Disable expensive effects
        this.worldSettings.setBloomEnabled(false);
        this.worldSettings.setAtmosphericFogDensity(0.05);
        
        // Reduce shadow quality
        this.lights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowResolution(512);
                light.setShadowDistance(25);
                light.setShadowSoftness(1.0);
            }
        });
    }
    
    private applyDesktopOptimizations() {
        // Full quality for desktop/VR
        this.worldSettings.setBloomEnabled(true);
        this.worldSettings.setBloomIntensity(0.8);
        
        this.lights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowResolution(2048);
                light.setShadowDistance(100);
                light.setShadowSoftness(0.3);
            }
        });
    }
}
```

### Dynamic Quality Adjustment

**Adaptive Quality System:**
```typescript
class AdaptiveQualityController extends Horizon.Component {
    private qualityLevel: string = "high";
    private targetFPS: number = 60;
    private frameTimeHistory: number[] = [];
    
    override start() {
        this.startQualityMonitoring();
    }
    
    private startQualityMonitoring() {
        setInterval(() => {
            this.updateQualityLevel();
        }, 1000);
    }
    
    private updateQualityLevel() {
        const currentFPS = this.calculateAverageFPS();
        
        if (currentFPS < this.targetFPS * 0.8) {
            this.reduceQuality();
        } else if (currentFPS > this.targetFPS * 1.1) {
            this.increaseQuality();
        }
    }
    
    private reduceQuality() {
        switch (this.qualityLevel) {
            case "high":
                this.qualityLevel = "medium";
                this.applyMediumQuality();
                break;
            case "medium":
                this.qualityLevel = "low";
                this.applyLowQuality();
                break;
        }
    }
    
    private increaseQuality() {
        switch (this.qualityLevel) {
            case "low":
                this.qualityLevel = "medium";
                this.applyMediumQuality();
                break;
            case "medium":
                this.qualityLevel = "high";
                this.applyHighQuality();
                break;
        }
    }
    
    private applyLowQuality() {
        // Disable most lighting effects
        this.worldSettings.setBloomEnabled(false);
        this.worldSettings.setAtmosphericFogDensity(0.02);
        this.reduceLightCount(4);
        this.setShadowQuality("low");
    }
    
    private applyMediumQuality() {
        // Balanced quality
        this.worldSettings.setBloomEnabled(true);
        this.worldSettings.setBloomIntensity(0.4);
        this.worldSettings.setAtmosphericFogDensity(0.1);
        this.reduceLightCount(8);
        this.setShadowQuality("medium");
    }
    
    private applyHighQuality() {
        // Full quality
        this.worldSettings.setBloomEnabled(true);
        this.worldSettings.setBloomIntensity(0.8);
        this.worldSettings.setAtmosphericFogDensity(0.2);
        this.reduceLightCount(15);
        this.setShadowQuality("high");
    }
}
```

## Troubleshooting Common Issues

Identify and fix common lighting performance problems.

### Common Performance Issues

**Light Not Visible:**
```typescript
class LightVisibilityDebugger extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    
    override start() {
        this.lights = this.getLightsInScene();
        this.debugLightVisibility();
    }
    
    private debugLightVisibility() {
        this.lights.forEach(light => {
            const intensity = light.getIntensity();
            const position = light.getPosition();
            const range = light instanceof Horizon.PointLightGizmo ? light.getRange() : 0;
            
            console.log(`Light Debug: Intensity=${intensity}, Position=${position}, Range=${range}`);
            
            if (intensity <= 0) {
                console.warn("Light has zero intensity - not visible");
            }
            
            if (light instanceof Horizon.PointLightGizmo && range <= 0) {
                console.warn("Point light has zero range - not visible");
            }
        });
    }
}
```

**Shadows Too Harsh:**
```typescript
class ShadowSoftnessController extends Horizon.Component {
    private shadowLights: Horizon.LightGizmo[] = [];
    
    override start() {
        this.shadowLights = this.getShadowCastingLights();
        this.softenShadows();
    }
    
    private softenShadows() {
        this.shadowLights.forEach(light => {
            if (light instanceof Horizon.DirectionalLightGizmo) {
                light.setShadowSoftness(0.8);
                light.setShadowBias(0.002);
                light.setShadowNormalBias(0.02);
            }
        });
    }
}
```

**Performance Issues:**
```typescript
class PerformanceTroubleshooter extends Horizon.Component {
    override start() {
        this.diagnosePerformanceIssues();
    }
    
    private diagnosePerformanceIssues() {
        const lightCount = this.countActiveLights();
        const shadowCount = this.countShadowCastingLights();
        const fps = this.getCurrentFPS();
        
        console.log(`Performance Diagnosis: FPS=${fps}, Lights=${lightCount}, Shadows=${shadowCount}`);
        
        if (fps < 30) {
            console.error("Low frame rate detected!");
            
            if (lightCount > 15) {
                console.warn("Too many lights - consider reducing light count");
            }
            
            if (shadowCount > 4) {
                console.warn("Too many shadow-casting lights - consider reducing shadow count");
            }
            
            this.suggestOptimizations();
        }
    }
    
    private suggestOptimizations() {
        console.log("Suggested optimizations:");
        console.log("1. Reduce light count to 10 or fewer");
        console.log("2. Limit shadow-casting lights to 2-3");
        console.log("3. Reduce shadow resolution");
        console.log("4. Disable bloom and atmospheric effects");
        console.log("5. Implement light culling");
    }
}
```

### Best Practices Summary

**Do's ✅**
- Monitor performance metrics regularly
- Use light culling for distant lights
- Implement LOD for lighting quality
- Limit shadow-casting lights
- Optimize for mobile devices
- Test on target hardware

**Don'ts ❌**
- Don't use too many lights simultaneously
- Don't enable shadows on all lights
- Don't ignore mobile performance
- Don't use high-quality effects on low-end devices
- Don't forget to test in VR

### Performance Checklist

**Before Publishing:**
- [ ] Frame rate is 60+ FPS on desktop, 30+ on mobile
- [ ] Light count is under 15 total
- [ ] Shadow-casting lights limited to 4 or fewer
- [ ] Bloom and atmospheric effects optimized
- [ ] Mobile-specific optimizations applied
- [ ] Performance tested on target devices

---

**Ready to Optimize Your Lighting?**

Use these optimization techniques to create beautiful lighting that performs well on all devices. Remember to test your optimizations thoroughly and monitor performance in real-world conditions.

**Next Steps:**
1. Implement light culling in your world
2. Add performance monitoring
3. Create mobile-specific optimizations
4. Test on various devices and hardware

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
