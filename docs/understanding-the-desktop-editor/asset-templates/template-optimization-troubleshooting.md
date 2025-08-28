# Template Optimization and Troubleshooting: Performance and Problem Solving

**Optimize your templates for maximum performance and resolve common issues.** This guide covers advanced optimization techniques, performance monitoring, troubleshooting strategies, and best practices to ensure your templates run smoothly across all devices.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Completion of Asset Templates Fundamentals and Advanced Template Techniques tutorials.

**Estimated Time to Complete**
45-60 minutes for optimization, 30 minutes for troubleshooting

## Table of Contents

1. [Performance Monitoring](#performance-monitoring)
2. [Advanced Optimization Techniques](#advanced-optimization-techniques)
3. [Mobile-Specific Optimizations](#mobile-specific-optimizations)
4. [Troubleshooting Common Issues](#troubleshooting-common-issues)
5. [Best Practices](#best-practices)

## Performance Monitoring

Monitor and analyze template performance to identify bottlenecks and optimization opportunities.

### Performance Metrics

**Key Performance Indicators:**
- **Frame Rate**: Target 60+ FPS on desktop, 30+ FPS on mobile
- **Memory Usage**: Monitor memory allocation and garbage collection
- **Template Instance Count**: Track active template instances
- **Update Frequency**: Monitor property update frequency
- **Loading Time**: Track template instantiation time

### Performance Monitor

**Real-Time Performance Tracking:**
```typescript
class TemplatePerformanceMonitor extends Horizon.Component {
    private frameCount: number = 0;
    private lastFrameTime: number = Date.now();
    private fps: number = 60;
    private memoryUsage: number = 0;
    private activeInstances: number = 0;
    private updateTime: number = 0;
    private loadingTime: number = 0;
    
    // Performance thresholds
    private targetFPS: number = 60;
    private maxMemoryUsage: number = 100000; // 100MB
    private maxActiveInstances: number = 100;
    private maxUpdateTime: number = 16; // 16ms for 60 FPS
    
    override start() {
        this.startPerformanceMonitoring();
    }
    
    private startPerformanceMonitoring() {
        // Monitor frame rate
        setInterval(() => {
            this.updateFrameRate();
        }, 1000);
        
        // Monitor memory usage
        setInterval(() => {
            this.updateMemoryUsage();
        }, 5000);
        
        // Monitor template instances
        setInterval(() => {
            this.updateInstanceCount();
        }, 2000);
        
        // Log performance data
        setInterval(() => {
            this.logPerformanceData();
        }, 10000); // Every 10 seconds
    }
    
    private updateFrameRate() {
        const currentTime = Date.now();
        const deltaTime = currentTime - this.lastFrameTime;
        
        if (deltaTime > 0) {
            this.fps = 1000 / deltaTime;
        }
        
        this.lastFrameTime = currentTime;
        this.frameCount++;
    }
    
    private updateMemoryUsage() {
        // Estimate memory usage based on active instances
        this.memoryUsage = this.activeInstances * 1024; // 1KB per instance estimate
    }
    
    private updateInstanceCount() {
        this.activeInstances = this.getActiveTemplateInstances();
    }
    
    private logPerformanceData() {
        const performanceData = {
            fps: this.fps.toFixed(1),
            memoryUsage: this.memoryUsage,
            activeInstances: this.activeInstances,
            updateTime: this.updateTime.toFixed(2),
            loadingTime: this.loadingTime.toFixed(2),
            timestamp: new Date().toISOString()
        };
        
        console.log("Template Performance Data:", performanceData);
        
        // Check for performance issues
        this.checkPerformanceIssues(performanceData);
    }
    
    private checkPerformanceIssues(data: any) {
        const issues: string[] = [];
        
        if (parseFloat(data.fps) < this.targetFPS * 0.8) {
            issues.push(`Low FPS: ${data.fps}`);
        }
        
        if (data.memoryUsage > this.maxMemoryUsage) {
            issues.push(`High memory usage: ${data.memoryUsage}KB`);
        }
        
        if (data.activeInstances > this.maxActiveInstances) {
            issues.push(`Too many instances: ${data.activeInstances}`);
        }
        
        if (parseFloat(data.updateTime) > this.maxUpdateTime) {
            issues.push(`Slow updates: ${data.updateTime}ms`);
        }
        
        if (issues.length > 0) {
            console.warn("Template Performance Issues Detected:", issues);
            this.triggerOptimization(issues);
        }
    }
    
    private triggerOptimization(issues: string[]) {
        // Apply automatic optimizations based on detected issues
        issues.forEach(issue => {
            if (issue.includes("Low FPS")) {
                this.reduceInstanceCount();
            }
            if (issue.includes("High memory usage")) {
                this.cleanupMemory();
            }
            if (issue.includes("Too many instances")) {
                this.pauseInstanceCreation();
            }
            if (issue.includes("Slow updates")) {
                this.reduceUpdateFrequency();
            }
        });
    }
    
    public setActiveInstanceCount(count: number) {
        this.activeInstances = count;
    }
    
    public recordLoadingTime(time: number) {
        this.loadingTime = time;
    }
    
    public getPerformanceSummary(): PerformanceSummary {
        return {
            fps: this.fps,
            memoryUsage: this.memoryUsage,
            activeInstances: this.activeInstances,
            updateTime: this.updateTime,
            loadingTime: this.loadingTime
        };
    }
    
    private getActiveTemplateInstances(): number {
        // Count active template instances
        return this.getEntitiesWithTag("TemplateInstance").length;
    }
    
    private reduceInstanceCount() {
        console.log("Reducing template instance count for performance");
    }
    
    private cleanupMemory() {
        console.log("Cleaning up memory");
    }
    
    private pauseInstanceCreation() {
        console.log("Pausing template instance creation");
    }
    
    private reduceUpdateFrequency() {
        console.log("Reducing update frequency");
    }
}

interface PerformanceSummary {
    fps: number;
    memoryUsage: number;
    activeInstances: number;
    updateTime: number;
    loadingTime: number;
}
```

### Performance Debugger

**Visual Performance Debugging:**
```typescript
class TemplatePerformanceDebugger extends Horizon.Component {
    private debugUI: Horizon.UI | null = null;
    private monitor: TemplatePerformanceMonitor | null = null;
    private isVisible: boolean = false;
    
    override start() {
        this.setupDebugUI();
        this.monitor = this.getComponent(TemplatePerformanceMonitor);
    }
    
    private setupDebugUI() {
        this.debugUI = this.createUI();
        this.debugUI.setVisible(false);
        this.updateDebugDisplay();
    }
    
    private updateDebugDisplay() {
        if (!this.debugUI || !this.monitor) return;
        
        const performance = this.monitor.getPerformanceSummary();
        
        const debugText = `
Template Performance Debug:
FPS: ${performance.fps.toFixed(1)}
Memory: ${performance.memoryUsage}KB
Instances: ${performance.activeInstances}
Update Time: ${performance.updateTime.toFixed(2)}ms
Loading Time: ${performance.loadingTime.toFixed(2)}ms
        `.trim();
        
        this.debugUI.setText(debugText);
    }
    
    public toggleDebugUI() {
        if (!this.debugUI) return;
        
        this.isVisible = !this.isVisible;
        this.debugUI.setVisible(this.isVisible);
        
        if (this.isVisible) {
            this.startDebugUpdates();
        } else {
            this.stopDebugUpdates();
        }
    }
    
    private startDebugUpdates() {
        setInterval(() => {
            this.updateDebugDisplay();
        }, 1000);
    }
    
    private stopDebugUpdates() {
        // Stop debug updates
    }
}
```

## Advanced Optimization Techniques

Implement advanced optimization strategies for template systems.

### Template Pooling System

**Object Pooling for Templates:**
```typescript
class TemplatePool extends Horizon.Component {
    private pools: Map<string, Horizon.Entity[]> = new Map();
    private activeInstances: Horizon.Entity[] = [];
    private poolSizes: Map<string, number> = new Map();
    
    override start() {
        this.initializePools();
    }
    
    private initializePools() {
        this.poolSizes.set("DoorTemplate", 20);
        this.poolSizes.set("LightSwitchTemplate", 15);
        this.poolSizes.set("FurnitureTemplate", 30);
        
        this.poolSizes.forEach((size, type) => {
            this.createPool(type, size);
        });
    }
    
    private createPool(templateType: string, size: number) {
        const pool: Horizon.Entity[] = [];
        
        for (let i = 0; i < size; i++) {
            const instance = this.createPooledInstance(templateType);
            if (instance) {
                pool.push(instance);
            }
        }
        
        this.pools.set(templateType, pool);
    }
    
    private createPooledInstance(type: string): Horizon.Entity | null {
        const instance = this.createEntity(type);
        if (instance) {
            instance.setActive(false); // Start inactive
            instance.onDestroy.add(() => {
                this.returnToPool(instance, type);
            });
        }
        return instance;
    }
    
    public spawnTemplate(type: string, position: Horizon.Vector3): Horizon.Entity | null {
        const pool = this.pools.get(type);
        if (!pool || pool.length === 0) {
            // Pool is empty, create new instance
            const instance = this.createPooledInstance(type);
            if (instance) {
                this.activateInstance(instance, position);
                return instance;
            }
            return null;
        }
        
        const instance = pool.pop()!;
        this.activateInstance(instance, position);
        return instance;
    }
    
    private activateInstance(instance: Horizon.Entity, position: Horizon.Vector3) {
        instance.setActive(true);
        instance.setPosition(position);
        this.activeInstances.push(instance);
        
        // Reset instance state
        this.resetInstanceState(instance);
    }
    
    private resetInstanceState(instance: Horizon.Entity) {
        // Reset properties to default values
        instance.setHealth(100);
        // Add other reset logic as needed
    }
    
    private returnToPool(instance: Horizon.Entity, type: string) {
        const index = this.activeInstances.indexOf(instance);
        if (index > -1) {
            this.activeInstances.splice(index, 1);
        }
        
        instance.setActive(false);
        
        const pool = this.pools.get(type);
        if (pool) {
            pool.push(instance);
        }
    }
    
    public getActiveInstanceCount(): number {
        return this.activeInstances.length;
    }
    
    public getPoolStatus(): Map<string, number> {
        const status = new Map<string, number>();
        
        this.pools.forEach((pool, type) => {
            status.set(type, pool.length);
        });
        
        return status;
    }
}
```

### LOD System for Templates

**Level of Detail for Template Instances:**
```typescript
class TemplateLODSystem extends Horizon.Component {
    private lodLevels: Map<string, LODLevel> = new Map();
    private player: Horizon.Player | null = null;
    private updateInterval: number = 500; // Update every 500ms
    
    override start() {
        this.setupLODSystem();
    }
    
    private setupLODSystem() {
        this.defineLODLevels();
        this.startLODUpdates();
    }
    
    private defineLODLevels() {
        // High detail (close range)
        this.lodLevels.set("High", {
            distance: 10,
            updateRate: 100, // 10 FPS
            animationRate: 30, // 30 FPS
            particleEffects: true,
            shadows: true,
            soundEffects: true
        });
        
        // Medium detail (medium range)
        this.lodLevels.set("Medium", {
            distance: 25,
            updateRate: 200, // 5 FPS
            animationRate: 15, // 15 FPS
            particleEffects: false,
            shadows: false,
            soundEffects: true
        });
        
        // Low detail (far range)
        this.lodLevels.set("Low", {
            distance: 50,
            updateRate: 500, // 2 FPS
            animationRate: 5, // 5 FPS
            particleEffects: false,
            shadows: false,
            soundEffects: false
        });
        
        // Culled (very far range)
        this.lodLevels.set("Culled", {
            distance: 100,
            updateRate: 0, // No updates
            animationRate: 0, // No animations
            particleEffects: false,
            shadows: false,
            soundEffects: false
        });
    }
    
    private startLODUpdates() {
        setInterval(() => {
            this.updateLODForAllInstances();
        }, this.updateInterval);
    }
    
    private updateLODForAllInstances() {
        const instances = this.getEntitiesWithTag("TemplateInstance");
        const playerPos = this.getPlayerPosition();
        
        instances.forEach(instance => {
            const distance = instance.getPosition().distance(playerPos);
            const lodLevel = this.getLODLevel(distance);
            this.applyLODToInstance(instance, lodLevel);
        });
    }
    
    private getLODLevel(distance: number): LODLevel {
        if (distance <= 10) return this.lodLevels.get("High")!;
        if (distance <= 25) return this.lodLevels.get("Medium")!;
        if (distance <= 50) return this.lodLevels.get("Low")!;
        return this.lodLevels.get("Culled")!;
    }
    
    private applyLODToInstance(instance: Horizon.Entity, lodLevel: LODLevel) {
        const templateScript = instance.getComponent(TemplateScript);
        if (templateScript) {
            templateScript.setUpdateRate(lodLevel.updateRate);
        }
        
        // Apply animation LOD
        if (lodLevel.animationRate > 0) {
            instance.setAnimationRate(lodLevel.animationRate);
        } else {
            instance.pauseAnimation();
        }
        
        // Apply visual effects LOD
        this.applyVisualLOD(instance, lodLevel);
        
        // Apply audio LOD
        this.applyAudioLOD(instance, lodLevel);
    }
    
    private applyVisualLOD(instance: Horizon.Entity, lodLevel: LODLevel) {
        // Toggle particle effects
        const particleSystems = instance.getComponents(ParticleSystem);
        particleSystems.forEach(ps => {
            ps.setEnabled(lodLevel.particleEffects);
        });
        
        // Toggle shadows
        const renderer = instance.getComponent(Renderer);
        if (renderer) {
            renderer.setCastShadows(lodLevel.shadows);
            renderer.setReceiveShadows(lodLevel.shadows);
        }
    }
    
    private applyAudioLOD(instance: Horizon.Entity, lodLevel: LODLevel) {
        const audioSources = instance.getComponents(AudioSource);
        audioSources.forEach(audio => {
            audio.setEnabled(lodLevel.soundEffects);
        });
    }
    
    private getPlayerPosition(): Horizon.Vector3 {
        const players = Horizon.Player.getAllPlayers();
        if (players.length > 0) {
            return players[0].getPosition();
        }
        return new Horizon.Vector3(0, 0, 0);
    }
}

interface LODLevel {
    distance: number;
    updateRate: number;
    animationRate: number;
    particleEffects: boolean;
    shadows: boolean;
    soundEffects: boolean;
}
```

## Mobile-Specific Optimizations

Optimize templates for mobile VR devices with limited resources.

### Mobile Optimization Manager

**Mobile-Specific Optimizations:**
```typescript
class MobileTemplateOptimizer extends Horizon.Component {
    private isMobile: boolean = false;
    private devicePerformance: DevicePerformance = DevicePerformance.Medium;
    private optimizationLevel: OptimizationLevel = OptimizationLevel.Balanced;
    
    // Mobile-specific limits
    private maxActiveInstances: number = 20;
    private maxParticleEffects: number = 10;
    private maxShadowCasters: number = 5;
    private updateRate: number = 200; // 5 FPS on mobile
    
    override start() {
        this.detectDeviceCapabilities();
        this.applyMobileOptimizations();
    }
    
    private detectDeviceCapabilities() {
        this.isMobile = this.isMobileDevice();
        
        if (this.isMobile) {
            this.devicePerformance = this.assessDevicePerformance();
            this.setOptimizationLevel();
        }
    }
    
    private isMobileDevice(): boolean {
        return navigator.userAgent.includes("Quest") || 
               navigator.userAgent.includes("Mobile");
    }
    
    private assessDevicePerformance(): DevicePerformance {
        const memory = navigator.deviceMemory || 4; // GB
        const cores = navigator.hardwareConcurrency || 4;
        
        if (memory >= 8 && cores >= 8) {
            return DevicePerformance.High;
        } else if (memory >= 4 && cores >= 4) {
            return DevicePerformance.Medium;
        } else {
            return DevicePerformance.Low;
        }
    }
    
    private setOptimizationLevel() {
        switch (this.devicePerformance) {
            case DevicePerformance.High:
                this.optimizationLevel = OptimizationLevel.Balanced;
                this.maxActiveInstances = 25;
                this.maxParticleEffects = 15;
                this.maxShadowCasters = 8;
                this.updateRate = 150;
                break;
            case DevicePerformance.Medium:
                this.optimizationLevel = OptimizationLevel.Performance;
                this.maxActiveInstances = 15;
                this.maxParticleEffects = 8;
                this.maxShadowCasters = 3;
                this.updateRate = 250;
                break;
            case DevicePerformance.Low:
                this.optimizationLevel = OptimizationLevel.Maximum;
                this.maxActiveInstances = 10;
                this.maxParticleEffects = 5;
                this.maxShadowCasters = 2;
                this.updateRate = 500;
                break;
        }
    }
    
    private applyMobileOptimizations() {
        if (!this.isMobile) return;
        
        console.log(`Applying mobile optimizations for ${DevicePerformance[this.devicePerformance]} device`);
        
        this.applyInstanceLimits();
        this.applyVisualOptimizations();
        this.applyAudioOptimizations();
        this.applyUpdateOptimizations();
    }
    
    private applyInstanceLimits() {
        const templateManager = this.getComponent(TemplateManager);
        if (templateManager) {
            templateManager.setMaxActiveInstances(this.maxActiveInstances);
        }
    }
    
    private applyVisualOptimizations() {
        // Reduce particle effects
        const particleSystems = this.getComponents(ParticleSystem);
        let activeParticles = 0;
        
        particleSystems.forEach(ps => {
            if (activeParticles < this.maxParticleEffects) {
                ps.setEnabled(true);
                activeParticles++;
            } else {
                ps.setEnabled(false);
            }
        });
        
        // Reduce shadow casters
        const renderers = this.getComponents(Renderer);
        let shadowCasters = 0;
        
        renderers.forEach(renderer => {
            if (shadowCasters < this.maxShadowCasters) {
                renderer.setCastShadows(true);
                shadowCasters++;
            } else {
                renderer.setCastShadows(false);
            }
        });
    }
    
    private applyAudioOptimizations() {
        // Reduce audio sources
        const audioSources = this.getComponents(AudioSource);
        audioSources.forEach((audio, index) => {
            if (index > 5) { // Limit to 5 audio sources
                audio.setEnabled(false);
            }
        });
    }
    
    private applyUpdateOptimizations() {
        // Reduce update frequency
        const instances = this.getEntitiesWithTag("TemplateInstance");
        instances.forEach(instance => {
            const script = instance.getComponent(TemplateScript);
            if (script) {
                script.setUpdateRate(this.updateRate);
            }
        });
    }
    
    public getOptimizationSettings(): MobileOptimizationSettings {
        return {
            isMobile: this.isMobile,
            devicePerformance: this.devicePerformance,
            optimizationLevel: this.optimizationLevel,
            maxActiveInstances: this.maxActiveInstances,
            maxParticleEffects: this.maxParticleEffects,
            maxShadowCasters: this.maxShadowCasters,
            updateRate: this.updateRate
        };
    }
}

enum DevicePerformance {
    Low,
    Medium,
    High
}

enum OptimizationLevel {
    Maximum,
    Performance,
    Balanced,
    Quality
}

interface MobileOptimizationSettings {
    isMobile: boolean;
    devicePerformance: DevicePerformance;
    optimizationLevel: OptimizationLevel;
    maxActiveInstances: number;
    maxParticleEffects: number;
    maxShadowCasters: number;
    updateRate: number;
}
```

## Troubleshooting Common Issues

Identify and resolve common template performance and functionality issues.

### Common Issues and Solutions

**Issue: Low Frame Rate**
```typescript
class FrameRateOptimizer extends Horizon.Component {
    private targetFPS: number = 60;
    private currentFPS: number = 60;
    private optimizationSteps: OptimizationStep[] = [];
    
    override start() {
        this.setupOptimizationSteps();
        this.startFrameRateMonitoring();
    }
    
    private setupOptimizationSteps() {
        this.optimizationSteps = [
            {
                name: "Reduce Instance Count",
                action: () => this.reduceInstanceCount(),
                priority: 1
            },
            {
                name: "Disable Particle Effects",
                action: () => this.disableParticleEffects(),
                priority: 2
            },
            {
                name: "Reduce Update Rate",
                action: () => this.reduceUpdateRate(),
                priority: 3
            },
            {
                name: "Disable Shadows",
                action: () => this.disableShadows(),
                priority: 4
            },
            {
                name: "Reduce Animation Quality",
                action: () => this.reduceAnimationQuality(),
                priority: 5
            }
        ];
    }
    
    private startFrameRateMonitoring() {
        setInterval(() => {
            this.checkFrameRate();
        }, 2000); // Check every 2 seconds
    }
    
    private checkFrameRate() {
        if (this.currentFPS < this.targetFPS * 0.8) {
            this.applyOptimization();
        }
    }
    
    private applyOptimization() {
        for (const step of this.optimizationSteps) {
            if (!step.applied) {
                step.action();
                step.applied = true;
                console.log(`Applied optimization: ${step.name}`);
                break;
            }
        }
    }
    
    private reduceInstanceCount() {
        const templateManager = this.getComponent(TemplateManager);
        if (templateManager) {
            templateManager.setMaxActiveInstances(10);
        }
    }
    
    private disableParticleEffects() {
        const particleSystems = this.getComponents(ParticleSystem);
        particleSystems.forEach(ps => {
            ps.setEnabled(false);
        });
    }
    
    private reduceUpdateRate() {
        const instances = this.getEntitiesWithTag("TemplateInstance");
        instances.forEach(instance => {
            const script = instance.getComponent(TemplateScript);
            if (script) {
                script.setUpdateRate(500); // 2 FPS
            }
        });
    }
    
    private disableShadows() {
        const renderers = this.getComponents(Renderer);
        renderers.forEach(renderer => {
            renderer.setCastShadows(false);
            renderer.setReceiveShadows(false);
        });
    }
    
    private reduceAnimationQuality() {
        const instances = this.getEntitiesWithTag("TemplateInstance");
        instances.forEach(instance => {
            instance.setAnimationRate(10); // 10 FPS
        });
    }
}

interface OptimizationStep {
    name: string;
    action: () => void;
    priority: number;
    applied?: boolean;
}
```

**Issue: Memory Leaks**
```typescript
class MemoryLeakDetector extends Horizon.Component {
    private instanceCount: number = 0;
    private lastInstanceCount: number = 0;
    private leakThreshold: number = 10; // 10 instances increase per check
    
    override start() {
        this.startMemoryMonitoring();
    }
    
    private startMemoryMonitoring() {
        setInterval(() => {
            this.checkForMemoryLeaks();
        }, 10000); // Check every 10 seconds
    }
    
    private checkForMemoryLeaks() {
        this.lastInstanceCount = this.instanceCount;
        this.instanceCount = this.countActiveInstances();
        
        const increase = this.instanceCount - this.lastInstanceCount;
        
        if (increase > this.leakThreshold) {
            console.warn(`Potential memory leak detected: ${increase} new instances`);
            this.cleanupOrphanedInstances();
        }
    }
    
    private countActiveInstances(): number {
        const instances = this.getEntitiesWithTag("TemplateInstance");
        const effects = this.getEntitiesWithTag("Effect");
        const audio = this.getEntitiesWithTag("Audio");
        
        return instances.length + effects.length + audio.length;
    }
    
    private cleanupOrphanedInstances() {
        // Clean up instances that should have been destroyed
        const instances = this.getEntitiesWithTag("TemplateInstance");
        instances.forEach(instance => {
            if (this.isOrphaned(instance)) {
                instance.destroy();
            }
        });
    }
    
    private isOrphaned(instance: Horizon.Entity): boolean {
        // Check if instance is outside world bounds or has invalid state
        const pos = instance.getPosition();
        const health = instance.getHealth();
        
        return pos.y < -100 || health <= 0;
    }
}
```

## Best Practices

Follow these best practices for optimal template performance and maintainability.

### Performance Best Practices

**1. Instance Management**
- Limit active instances to 20-100 based on device capabilities
- Use object pooling for frequently created templates
- Implement LOD systems for distant instances

**2. Update Optimization**
- Reduce update frequency for distant instances
- Use simplified updates for basic templates
- Implement culling for off-screen instances

**3. Visual Optimization**
- Limit particle effects and animations
- Use LOD for complex visual effects
- Optimize shadow casting and receiving

**4. Memory Management**
- Clean up destroyed instances promptly
- Reuse objects when possible
- Monitor memory usage regularly

**5. Mobile Considerations**
- Reduce instance count on mobile devices
- Simplify visual effects
- Test on actual mobile hardware

### Code Organization Best Practices

**1. Modular Design**
- Separate template logic, optimization, and monitoring systems
- Use interfaces for system communication
- Implement event-driven architecture

**2. Configuration Management**
- Use configuration files for optimization settings
- Implement runtime performance adjustment
- Provide debugging tools

**3. Error Handling**
- Implement proper error handling for template systems
- Add fallback behaviors for failed operations
- Log performance issues for analysis

### Template Design Best Practices

**1. Efficient Properties**
- Use efficient data types for properties
- Minimize property update frequency
- Implement property change notifications

**2. Resource Management**
- Share resources between instances when possible
- Implement proper cleanup methods
- Monitor resource usage

**3. Scalability**
- Design templates for scalability from the start
- Implement performance monitoring early
- Plan for mobile optimization

---

**Optimization Complete**

You now have comprehensive tools and techniques to optimize your template systems for maximum performance across all devices.

**Next Steps:**
1. Implement performance monitoring in your template systems
2. Apply mobile-specific optimizations
3. Set up automated performance testing
4. Monitor and tune your systems based on real-world usage

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
