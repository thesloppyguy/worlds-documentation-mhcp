# Enemy Wave Performance Optimization: Maximizing Quality and Frame Rate

**Optimize your enemy wave systems for smooth performance and engaging gameplay.** This guide covers advanced optimization techniques, performance monitoring, and troubleshooting strategies to ensure your wave-based games run smoothly on all devices.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Completion of Enemy Wave Fundamentals and Advanced AI Behaviors tutorials.

**Estimated Time to Complete**
45-60 minutes for optimization, 30 minutes for troubleshooting

## Table of Contents

1. [Performance Monitoring](#performance-monitoring)
2. [Advanced Optimization Techniques](#advanced-optimization-techniques)
3. [Mobile-Specific Optimizations](#mobile-specific-optimizations)
4. [Troubleshooting Common Issues](#troubleshooting-common-issues)
5. [Best Practices](#best-practices)

## Performance Monitoring

Monitor and analyze your wave system performance to identify bottlenecks.

### Performance Metrics

**Key Performance Indicators:**
- **Frame Rate**: Target 60+ FPS on desktop, 30+ FPS on mobile
- **Active Enemy Count**: Monitor total active enemies
- **AI Update Frequency**: Track AI processing time
- **Memory Usage**: Monitor memory allocation
- **Spawn Rate**: Track enemy spawning frequency

### Performance Monitor

**Real-Time Performance Tracking:**
```typescript
class WavePerformanceMonitor extends Horizon.Component {
    private frameCount: number = 0;
    private lastFrameTime: number = Date.now();
    private fps: number = 60;
    private activeEnemies: number = 0;
    private aiUpdateTime: number = 0;
    private memoryUsage: number = 0;
    private spawnRate: number = 0;
    private lastSpawnTime: number = 0;
    private spawnCount: number = 0;
    
    // Performance thresholds
    private maxActiveEnemies: number = 50;
    private maxAIUpdateTime: number = 16; // 16ms for 60 FPS
    private targetFPS: number = 60;
    
    override start() {
        this.startPerformanceMonitoring();
    }
    
    private startPerformanceMonitoring() {
        // Monitor frame rate
        setInterval(() => {
            this.updateFrameRate();
        }, 1000);
        
        // Monitor AI performance
        setInterval(() => {
            this.updateAIPerformance();
        }, 100);
        
        // Monitor memory usage
        setInterval(() => {
            this.updateMemoryUsage();
        }, 5000);
        
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
    
    private updateAIPerformance() {
        const startTime = performance.now();
        
        // Simulate AI update time measurement
        this.aiUpdateTime = performance.now() - startTime;
    }
    
    private updateMemoryUsage() {
        // Estimate memory usage based on active entities
        this.memoryUsage = this.activeEnemies * 1024; // 1KB per enemy estimate
    }
    
    private updateSpawnRate() {
        const currentTime = Date.now();
        const timeSinceLastSpawn = currentTime - this.lastSpawnTime;
        
        if (timeSinceLastSpawn > 0) {
            this.spawnRate = 60000 / timeSinceLastSpawn; // Spawns per minute
        }
        
        this.lastSpawnTime = currentTime;
        this.spawnCount++;
    }
    
    private logPerformanceData() {
        const performanceData = {
            fps: this.fps.toFixed(1),
            activeEnemies: this.activeEnemies,
            aiUpdateTime: this.aiUpdateTime.toFixed(2),
            memoryUsage: this.memoryUsage,
            spawnRate: this.spawnRate.toFixed(1),
            timestamp: new Date().toISOString()
        };
        
        console.log("Performance Data:", performanceData);
        
        // Check for performance issues
        this.checkPerformanceIssues(performanceData);
    }
    
    private checkPerformanceIssues(data: any) {
        const issues: string[] = [];
        
        if (parseFloat(data.fps) < this.targetFPS * 0.8) {
            issues.push(`Low FPS: ${data.fps}`);
        }
        
        if (data.activeEnemies > this.maxActiveEnemies) {
            issues.push(`Too many enemies: ${data.activeEnemies}`);
        }
        
        if (parseFloat(data.aiUpdateTime) > this.maxAIUpdateTime) {
            issues.push(`Slow AI updates: ${data.aiUpdateTime}ms`);
        }
        
        if (data.memoryUsage > 50000) { // 50MB threshold
            issues.push(`High memory usage: ${data.memoryUsage}KB`);
        }
        
        if (issues.length > 0) {
            console.warn("Performance Issues Detected:", issues);
            this.triggerOptimization(issues);
        }
    }
    
    private triggerOptimization(issues: string[]) {
        // Apply automatic optimizations based on detected issues
        issues.forEach(issue => {
            if (issue.includes("Low FPS")) {
                this.reduceEnemyCount();
            }
            if (issue.includes("Too many enemies")) {
                this.pauseSpawning();
            }
            if (issue.includes("Slow AI updates")) {
                this.reduceAIUpdateFrequency();
            }
            if (issue.includes("High memory usage")) {
                this.cleanupMemory();
            }
        });
    }
    
    public setActiveEnemyCount(count: number) {
        this.activeEnemies = count;
    }
    
    public recordSpawn() {
        this.updateSpawnRate();
    }
    
    public getPerformanceSummary(): PerformanceSummary {
        return {
            fps: this.fps,
            activeEnemies: this.activeEnemies,
            aiUpdateTime: this.aiUpdateTime,
            memoryUsage: this.memoryUsage,
            spawnRate: this.spawnRate
        };
    }
    
    private reduceEnemyCount() {
        // Reduce active enemy count
        console.log("Reducing enemy count for performance");
    }
    
    private pauseSpawning() {
        // Pause enemy spawning
        console.log("Pausing enemy spawning");
    }
    
    private reduceAIUpdateFrequency() {
        // Reduce AI update frequency
        console.log("Reducing AI update frequency");
    }
    
    private cleanupMemory() {
        // Clean up unused resources
        console.log("Cleaning up memory");
    }
}

interface PerformanceSummary {
    fps: number;
    activeEnemies: number;
    aiUpdateTime: number;
    memoryUsage: number;
    spawnRate: number;
}
```

### Performance Debugger

**Visual Performance Debugging:**
```typescript
class PerformanceDebugger extends Horizon.Component {
    private debugUI: Horizon.UI | null = null;
    private monitor: WavePerformanceMonitor | null = null;
    private isVisible: boolean = false;
    
    override start() {
        this.setupDebugUI();
        this.monitor = this.getComponent(WavePerformanceMonitor);
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
Performance Debug Info:
FPS: ${performance.fps.toFixed(1)}
Active Enemies: ${performance.activeEnemies}
AI Update Time: ${performance.aiUpdateTime.toFixed(2)}ms
Memory Usage: ${performance.memoryUsage}KB
Spawn Rate: ${performance.spawnRate.toFixed(1)}/min
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

Implement advanced optimization strategies for large-scale wave systems.

### Level of Detail (LOD) System

**Enemy LOD Implementation:**
```typescript
class EnemyLODSystem extends Horizon.Component {
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
            aiUpdateRate: 100, // 10 FPS
            animationRate: 30, // 30 FPS
            particleEffects: true,
            shadows: true,
            soundEffects: true
        });
        
        // Medium detail (medium range)
        this.lodLevels.set("Medium", {
            distance: 25,
            aiUpdateRate: 200, // 5 FPS
            animationRate: 15, // 15 FPS
            particleEffects: false,
            shadows: false,
            soundEffects: true
        });
        
        // Low detail (far range)
        this.lodLevels.set("Low", {
            distance: 50,
            aiUpdateRate: 500, // 2 FPS
            animationRate: 5, // 5 FPS
            particleEffects: false,
            shadows: false,
            soundEffects: false
        });
        
        // Culled (very far range)
        this.lodLevels.set("Culled", {
            distance: 100,
            aiUpdateRate: 0, // No AI updates
            animationRate: 0, // No animations
            particleEffects: false,
            shadows: false,
            soundEffects: false
        });
    }
    
    private startLODUpdates() {
        setInterval(() => {
            this.updateLODForAllEnemies();
        }, this.updateInterval);
    }
    
    private updateLODForAllEnemies() {
        const enemies = this.getEntitiesWithTag("Enemy");
        const playerPos = this.getPlayerPosition();
        
        enemies.forEach(enemy => {
            const distance = enemy.getPosition().distance(playerPos);
            const lodLevel = this.getLODLevel(distance);
            this.applyLODToEnemy(enemy, lodLevel);
        });
    }
    
    private getLODLevel(distance: number): LODLevel {
        if (distance <= 10) return this.lodLevels.get("High")!;
        if (distance <= 25) return this.lodLevels.get("Medium")!;
        if (distance <= 50) return this.lodLevels.get("Low")!;
        return this.lodLevels.get("Culled")!;
    }
    
    private applyLODToEnemy(enemy: Horizon.Entity, lodLevel: LODLevel) {
        const enemyAI = enemy.getComponent(EnemyAI);
        if (enemyAI) {
            enemyAI.setUpdateRate(lodLevel.aiUpdateRate);
        }
        
        // Apply animation LOD
        if (lodLevel.animationRate > 0) {
            enemy.setAnimationRate(lodLevel.animationRate);
        } else {
            enemy.pauseAnimation();
        }
        
        // Apply visual effects LOD
        this.applyVisualLOD(enemy, lodLevel);
        
        // Apply audio LOD
        this.applyAudioLOD(enemy, lodLevel);
    }
    
    private applyVisualLOD(enemy: Horizon.Entity, lodLevel: LODLevel) {
        // Toggle particle effects
        const particleSystems = enemy.getComponents(ParticleSystem);
        particleSystems.forEach(ps => {
            ps.setEnabled(lodLevel.particleEffects);
        });
        
        // Toggle shadows
        const renderer = enemy.getComponent(Renderer);
        if (renderer) {
            renderer.setCastShadows(lodLevel.shadows);
            renderer.setReceiveShadows(lodLevel.shadows);
        }
    }
    
    private applyAudioLOD(enemy: Horizon.Entity, lodLevel: LODLevel) {
        const audioSources = enemy.getComponents(AudioSource);
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
    aiUpdateRate: number;
    animationRate: number;
    particleEffects: boolean;
    shadows: boolean;
    soundEffects: boolean;
}
```

### Culling System

**Advanced Culling Implementation:**
```typescript
class EnemyCullingSystem extends Horizon.Component {
    private cullingZones: CullingZone[] = [];
    private player: Horizon.Player | null = null;
    private cullingRadius: number = 100;
    private occlusionCulling: boolean = true;
    
    override start() {
        this.setupCullingSystem();
    }
    
    private setupCullingSystem() {
        this.createCullingZones();
        this.startCullingUpdates();
    }
    
    private createCullingZones() {
        // Create multiple culling zones for different areas
        this.cullingZones = [
            {
                center: new Horizon.Vector3(0, 0, 0),
                radius: 50,
                type: CullingType.Distance
            },
            {
                center: new Horizon.Vector3(100, 0, 100),
                radius: 30,
                type: CullingType.Occlusion
            }
        ];
    }
    
    private startCullingUpdates() {
        setInterval(() => {
            this.updateCulling();
        }, 1000); // Update every second
    }
    
    private updateCulling() {
        const enemies = this.getEntitiesWithTag("Enemy");
        const playerPos = this.getPlayerPosition();
        
        enemies.forEach(enemy => {
            const shouldCull = this.shouldCullEnemy(enemy, playerPos);
            this.setEnemyVisibility(enemy, !shouldCull);
        });
    }
    
    private shouldCullEnemy(enemy: Horizon.Entity, playerPos: Horizon.Vector3): boolean {
        const enemyPos = enemy.getPosition();
        const distance = enemyPos.distance(playerPos);
        
        // Distance-based culling
        if (distance > this.cullingRadius) {
            return true;
        }
        
        // Zone-based culling
        for (const zone of this.cullingZones) {
            if (this.isEnemyInCullingZone(enemy, zone)) {
                return true;
            }
        }
        
        // Occlusion culling
        if (this.occlusionCulling && this.isEnemyOccluded(enemy, playerPos)) {
            return true;
        }
        
        return false;
    }
    
    private isEnemyInCullingZone(enemy: Horizon.Entity, zone: CullingZone): boolean {
        const enemyPos = enemy.getPosition();
        const distance = enemyPos.distance(zone.center);
        
        switch (zone.type) {
            case CullingType.Distance:
                return distance > zone.radius;
            case CullingType.Occlusion:
                return this.isEnemyOccluded(enemy, zone.center);
            default:
                return false;
        }
    }
    
    private isEnemyOccluded(enemy: Horizon.Entity, viewPoint: Horizon.Vector3): boolean {
        const enemyPos = enemy.getPosition();
        const direction = enemyPos.subtract(viewPoint).normalize();
        const distance = enemyPos.distance(viewPoint);
        
        // Simple raycast for occlusion
        const hit = this.raycast(viewPoint, direction, distance);
        return hit && hit !== enemy;
    }
    
    private setEnemyVisibility(enemy: Horizon.Entity, visible: boolean) {
        // Set enemy visibility
        enemy.setVisible(visible);
        
        // Disable AI when culled
        const ai = enemy.getComponent(EnemyAI);
        if (ai) {
            ai.setEnabled(visible);
        }
        
        // Disable animations when culled
        if (!visible) {
            enemy.pauseAnimation();
        }
    }
    
    private getPlayerPosition(): Horizon.Vector3 {
        const players = Horizon.Player.getAllPlayers();
        if (players.length > 0) {
            return players[0].getPosition();
        }
        return new Horizon.Vector3(0, 0, 0);
    }
}

interface CullingZone {
    center: Horizon.Vector3;
    radius: number;
    type: CullingType;
}

enum CullingType {
    Distance,
    Occlusion
}
```

## Mobile-Specific Optimizations

Optimize wave systems for mobile VR devices with limited resources.

### Mobile Optimization Manager

**Mobile-Specific Optimizations:**
```typescript
class MobileOptimizationManager extends Horizon.Component {
    private isMobile: boolean = false;
    private devicePerformance: DevicePerformance = DevicePerformance.Medium;
    private optimizationLevel: OptimizationLevel = OptimizationLevel.Balanced;
    
    // Mobile-specific limits
    private maxActiveEnemies: number = 20;
    private maxParticleEffects: number = 10;
    private maxShadowCasters: number = 5;
    private aiUpdateRate: number = 200; // 5 FPS on mobile
    
    override start() {
        this.detectDeviceCapabilities();
        this.applyMobileOptimizations();
    }
    
    private detectDeviceCapabilities() {
        // Detect if running on mobile
        this.isMobile = this.isMobileDevice();
        
        if (this.isMobile) {
            this.devicePerformance = this.assessDevicePerformance();
            this.setOptimizationLevel();
        }
    }
    
    private isMobileDevice(): boolean {
        // Check if running on Quest or mobile device
        return navigator.userAgent.includes("Quest") || 
               navigator.userAgent.includes("Mobile");
    }
    
    private assessDevicePerformance(): DevicePerformance {
        // Assess device performance based on available memory and processing power
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
                this.maxActiveEnemies = 25;
                this.maxParticleEffects = 15;
                this.maxShadowCasters = 8;
                this.aiUpdateRate = 150;
                break;
            case DevicePerformance.Medium:
                this.optimizationLevel = OptimizationLevel.Performance;
                this.maxActiveEnemies = 15;
                this.maxParticleEffects = 8;
                this.maxShadowCasters = 3;
                this.aiUpdateRate = 250;
                break;
            case DevicePerformance.Low:
                this.optimizationLevel = OptimizationLevel.Maximum;
                this.maxActiveEnemies = 10;
                this.maxParticleEffects = 5;
                this.maxShadowCasters = 2;
                this.aiUpdateRate = 500;
                break;
        }
    }
    
    private applyMobileOptimizations() {
        if (!this.isMobile) return;
        
        console.log(`Applying mobile optimizations for ${DevicePerformance[this.devicePerformance]} device`);
        
        this.applyEnemyLimits();
        this.applyVisualOptimizations();
        this.applyAudioOptimizations();
        this.applyAIOptimizations();
    }
    
    private applyEnemyLimits() {
        // Limit active enemies
        const waveManager = this.getComponent(WaveManager);
        if (waveManager) {
            waveManager.setMaxActiveEnemies(this.maxActiveEnemies);
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
    
    private applyAIOptimizations() {
        // Reduce AI update frequency
        const enemies = this.getEntitiesWithTag("Enemy");
        enemies.forEach(enemy => {
            const ai = enemy.getComponent(EnemyAI);
            if (ai) {
                ai.setUpdateRate(this.aiUpdateRate);
            }
        });
    }
    
    public getOptimizationSettings(): MobileOptimizationSettings {
        return {
            isMobile: this.isMobile,
            devicePerformance: this.devicePerformance,
            optimizationLevel: this.optimizationLevel,
            maxActiveEnemies: this.maxActiveEnemies,
            maxParticleEffects: this.maxParticleEffects,
            maxShadowCasters: this.maxShadowCasters,
            aiUpdateRate: this.aiUpdateRate
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
    maxActiveEnemies: number;
    maxParticleEffects: number;
    maxShadowCasters: number;
    aiUpdateRate: number;
}
```

## Troubleshooting Common Issues

Identify and resolve common performance and gameplay issues.

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
                name: "Reduce Enemy Count",
                action: () => this.reduceEnemyCount(),
                priority: 1
            },
            {
                name: "Disable Particle Effects",
                action: () => this.disableParticleEffects(),
                priority: 2
            },
            {
                name: "Reduce AI Update Rate",
                action: () => this.reduceAIUpdateRate(),
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
    
    private reduceEnemyCount() {
        const waveManager = this.getComponent(WaveManager);
        if (waveManager) {
            waveManager.setMaxActiveEnemies(10);
        }
    }
    
    private disableParticleEffects() {
        const particleSystems = this.getComponents(ParticleSystem);
        particleSystems.forEach(ps => {
            ps.setEnabled(false);
        });
    }
    
    private reduceAIUpdateRate() {
        const enemies = this.getEntitiesWithTag("Enemy");
        enemies.forEach(enemy => {
            const ai = enemy.getComponent(EnemyAI);
            if (ai) {
                ai.setUpdateRate(500); // 2 FPS
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
        const enemies = this.getEntitiesWithTag("Enemy");
        enemies.forEach(enemy => {
            enemy.setAnimationRate(10); // 10 FPS
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
    private entityCount: number = 0;
    private lastEntityCount: number = 0;
    private leakThreshold: number = 10; // 10 entities increase per check
    
    override start() {
        this.startMemoryMonitoring();
    }
    
    private startMemoryMonitoring() {
        setInterval(() => {
            this.checkForMemoryLeaks();
        }, 10000); // Check every 10 seconds
    }
    
    private checkForMemoryLeaks() {
        this.lastEntityCount = this.entityCount;
        this.entityCount = this.countActiveEntities();
        
        const increase = this.entityCount - this.lastEntityCount;
        
        if (increase > this.leakThreshold) {
            console.warn(`Potential memory leak detected: ${increase} new entities`);
            this.cleanupOrphanedEntities();
        }
    }
    
    private countActiveEntities(): number {
        const enemies = this.getEntitiesWithTag("Enemy");
        const projectiles = this.getEntitiesWithTag("Projectile");
        const effects = this.getEntitiesWithTag("Effect");
        
        return enemies.length + projectiles.length + effects.length;
    }
    
    private cleanupOrphanedEntities() {
        // Clean up entities that should have been destroyed
        const enemies = this.getEntitiesWithTag("Enemy");
        enemies.forEach(enemy => {
            if (this.isOrphaned(enemy)) {
                enemy.destroy();
            }
        });
    }
    
    private isOrphaned(entity: Horizon.Entity): boolean {
        // Check if entity is outside world bounds or has invalid state
        const pos = entity.getPosition();
        const health = entity.getHealth();
        
        return pos.y < -100 || health <= 0;
    }
}
```

## Best Practices

Follow these best practices for optimal wave system performance.

### Performance Best Practices

**1. Enemy Limits**
- Limit active enemies to 20-50 based on device capabilities
- Use object pooling for frequently spawned enemies
- Implement LOD systems for distant enemies

**2. AI Optimization**
- Reduce AI update frequency for distant enemies
- Use simplified AI for basic enemies
- Implement culling for off-screen enemies

**3. Visual Optimization**
- Limit particle effects and animations
- Use LOD for complex visual effects
- Optimize shadow casting and receiving

**4. Memory Management**
- Clean up destroyed entities promptly
- Reuse objects when possible
- Monitor memory usage regularly

**5. Mobile Considerations**
- Reduce enemy count on mobile devices
- Simplify AI and visual effects
- Test on actual mobile hardware

### Code Organization Best Practices

**1. Modular Design**
- Separate AI, spawning, and optimization systems
- Use interfaces for system communication
- Implement event-driven architecture

**2. Configuration Management**
- Use configuration files for tuning
- Implement runtime performance adjustment
- Provide debugging tools

**3. Error Handling**
- Implement proper error handling for AI systems
- Add fallback behaviors for failed operations
- Log performance issues for analysis

---

**Optimization Complete**

You now have comprehensive tools and techniques to optimize your enemy wave systems for maximum performance across all devices.

**Next Steps:**
1. Implement performance monitoring in your wave systems
2. Apply mobile-specific optimizations
3. Set up automated performance testing
4. Monitor and tune your systems based on real-world usage

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
