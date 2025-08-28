# Dynamic Lighting Systems: Interactive and Responsive Lighting

**Create lighting that responds to gameplay, player actions, and time to make your worlds feel alive and dynamic.** This guide covers interactive lighting triggers, event-driven lighting, and complete day/night cycle systems.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Understanding of basic lighting fundamentals and TypeScript scripting.

**Estimated Time to Complete**
1-2 hours for dynamic systems, 45-60 minutes for day/night cycles

## Table of Contents

1. [Interactive Lighting Triggers](#interactive-lighting-triggers)
2. [Event-Driven Lighting](#event-driven-lighting)
3. [Day/Night Cycle Systems](#daynight-cycle-systems)
4. [Weather-Based Lighting](#weather-based-lighting)
5. [Performance Considerations](#performance-considerations)

## Interactive Lighting Triggers

Create lighting that responds to player proximity, actions, and environmental changes.

### Proximity-Based Lighting

**Basic Proximity System:**
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

**Advanced Proximity with Distance Falloff:**
```typescript
class DistanceBasedLighting extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private player: Horizon.Player;
    private maxDistance: number = 20.0;
    
    override start() {
        this.lights = this.getLightsInScene();
        this.player = Horizon.Player.getLocalPlayer();
        this.startDistanceTracking();
    }
    
    private startDistanceTracking() {
        setInterval(() => {
            this.updateLightingBasedOnDistance();
        }, 100);
    }
    
    private updateLightingBasedOnDistance() {
        if (!this.player) return;
        
        const playerPos = this.player.getPosition();
        
        this.lights.forEach(light => {
            const lightPos = light.getPosition();
            const distance = playerPos.distance(lightPos);
            
            if (distance <= this.maxDistance) {
                // Calculate intensity based on distance
                const intensityFactor = 1.0 - (distance / this.maxDistance);
                const baseIntensity = light.getIntensity();
                light.setIntensity(baseIntensity * intensityFactor);
                
                // Add warm color when close
                if (distance < this.maxDistance * 0.5) {
                    light.setColor(255, 240, 200);
                } else {
                    light.setColor(255, 255, 255);
                }
            } else {
                // Return to base intensity when far
                light.setIntensity(light.getIntensity());
                light.setColor(255, 255, 255);
            }
        });
    }
}
```

### Action-Based Lighting

**Lighting for Player Actions:**
```typescript
class ActionBasedLighting extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private currentAction: string = "idle";
    
    override start() {
        this.lights = this.getLightsInScene();
        this.setupActionListeners();
    }
    
    private setupActionListeners() {
        // Listen for player actions
        Horizon.NetworkEvent.on("playerAction").add((eventData) => {
            this.handlePlayerAction(eventData.action);
        });
        
        // Listen for interaction events
        Horizon.NetworkEvent.on("interaction").add((eventData) => {
            this.handleInteraction(eventData.type);
        });
    }
    
    private handlePlayerAction(action: string) {
        switch (action) {
            case "jump":
                this.triggerJumpLighting();
                break;
            case "sprint":
                this.triggerSprintLighting();
                break;
            case "crouch":
                this.triggerCrouchLighting();
                break;
        }
    }
    
    private triggerJumpLighting() {
        // Bright flash effect
        this.lights.forEach(light => {
            const originalIntensity = light.getIntensity();
            light.setIntensity(originalIntensity * 3);
            
            setTimeout(() => {
                light.setIntensity(originalIntensity);
            }, 200);
        });
    }
    
    private triggerSprintLighting() {
        // Dynamic lighting for movement
        this.lights.forEach(light => {
            light.setColor(255, 255, 220); // Slight warm tint
        });
    }
    
    private triggerCrouchLighting() {
        // Dimmed lighting for stealth
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 0.5);
            light.setColor(200, 200, 220); // Cool tint
        });
    }
}
```

## Event-Driven Lighting

Create lighting systems that respond to game events, story progression, and environmental changes.

### Game Event Lighting

**Comprehensive Event System:**
```typescript
class EventLightingController extends Horizon.Component {
    private mainLight: Horizon.DirectionalLightGizmo;
    private emergencyLights: Horizon.SpotLightGizmo[] = [];
    private ambientLights: Horizon.LightGizmo[] = [];
    private currentEvent: string = "normal";
    
    override start() {
        this.mainLight = this.props.mainLight.as(Horizon.DirectionalLightGizmo);
        this.emergencyLights = this.getEmergencyLights();
        this.ambientLights = this.getAmbientLights();
        this.setupEventListeners();
    }
    
    private setupEventListeners() {
        // Listen for game events
        Horizon.NetworkEvent.on("gameEvent").add((eventData) => {
            this.handleGameEvent(eventData.eventType);
        });
        
        // Listen for story progression
        Horizon.NetworkEvent.on("storyProgress").add((eventData) => {
            this.handleStoryProgress(eventData.chapter);
        });
        
        // Listen for environmental changes
        Horizon.NetworkEvent.on("environmentChange").add((eventData) => {
            this.handleEnvironmentChange(eventData.type);
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
            case "calm":
                this.activateCalmLighting();
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
        
        // Flash effect
        this.startFlashingEffect();
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
        
        // Rainbow effect
        this.startRainbowEffect();
    }
    
    private activateMysteryLighting() {
        // Dark, mysterious lighting
        this.mainLight.setIntensity(0.3);
        this.mainLight.setColor(100, 100, 150);
        
        this.ambientLights.forEach(light => {
            light.setIntensity(light.getIntensity() * 0.3);
            light.setColor(150, 150, 200);
        });
    }
    
    private startFlashingEffect() {
        let flashCount = 0;
        const flashInterval = setInterval(() => {
            this.emergencyLights.forEach(light => {
                light.setIntensity(flashCount % 2 === 0 ? 3.0 : 0.5);
            });
            
            flashCount++;
            if (flashCount > 10) {
                clearInterval(flashInterval);
            }
        }, 200);
    }
    
    private startRainbowEffect() {
        let hue = 0;
        const rainbowInterval = setInterval(() => {
            this.emergencyLights.forEach((light, index) => {
                const lightHue = (hue + index * 60) % 360;
                const color = this.hsvToRgb(lightHue, 1.0, 1.0);
                light.setColor(color.r, color.g, color.b);
            });
            
            hue += 5;
            if (hue > 360) {
                clearInterval(rainbowInterval);
            }
        }, 100);
    }
    
    private hsvToRgb(h: number, s: number, v: number) {
        // Convert HSV to RGB
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

### Story-Driven Lighting

**Lighting for Narrative Progression:**
```typescript
class StoryLightingController extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private currentChapter: number = 1;
    private worldSettings: Horizon.WorldSettings;
    
    override start() {
        this.lights = this.getLightsInScene();
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.setupStoryLighting();
    }
    
    private setupStoryLighting() {
        // Set initial lighting for chapter 1
        this.setChapterLighting(1);
    }
    
    private setChapterLighting(chapter: number) {
        this.currentChapter = chapter;
        
        switch (chapter) {
            case 1: // Introduction - Bright and welcoming
                this.setBrightWelcomingLighting();
                break;
            case 2: // Rising action - Slightly darker
                this.setRisingActionLighting();
                break;
            case 3: // Climax - Dramatic lighting
                this.setClimaxLighting();
                break;
            case 4: // Resolution - Warm and peaceful
                this.setResolutionLighting();
                break;
        }
    }
    
    private setBrightWelcomingLighting() {
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 1.2);
            light.setColor(255, 255, 255);
        });
        
        this.worldSettings.setAtmosphericFogDensity(0.05);
        this.worldSettings.setAtmosphericFogColor(220, 230, 255);
    }
    
    private setRisingActionLighting() {
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 0.8);
            light.setColor(240, 240, 255);
        });
        
        this.worldSettings.setAtmosphericFogDensity(0.15);
        this.worldSettings.setAtmosphericFogColor(200, 210, 240);
    }
    
    private setClimaxLighting() {
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 0.5);
            light.setColor(200, 200, 220);
        });
        
        this.worldSettings.setAtmosphericFogDensity(0.3);
        this.worldSettings.setAtmosphericFogColor(150, 160, 200);
    }
    
    private setResolutionLighting() {
        this.lights.forEach(light => {
            light.setIntensity(light.getIntensity() * 1.0);
            light.setColor(255, 240, 220);
        });
        
        this.worldSettings.setAtmosphericFogDensity(0.1);
        this.worldSettings.setAtmosphericFogColor(255, 240, 220);
    }
}
```

## Day/Night Cycle Systems

Create dynamic day/night cycles that affect your entire world's lighting system.

### Complete Day/Night System

**Full Day/Night Implementation:**
```typescript
class DayNightCycle extends Horizon.Component {
    private sunLight: Horizon.DirectionalLightGizmo;
    private moonLight: Horizon.DirectionalLightGizmo;
    private ambientLights: Horizon.LightGizmo[] = [];
    private worldSettings: Horizon.WorldSettings;
    private timeOfDay: number = 0.5; // 0 = midnight, 0.5 = noon, 1 = midnight
    private cycleSpeed: number = 0.001; // How fast time progresses
    private isPaused: boolean = false;
    
    override start() {
        this.sunLight = this.props.sunLight.as(Horizon.DirectionalLightGizmo);
        this.moonLight = this.props.moonLight.as(Horizon.DirectionalLightGizmo);
        this.ambientLights = this.getAmbientLights();
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.startDayNightCycle();
    }
    
    private startDayNightCycle() {
        setInterval(() => {
            if (!this.isPaused) {
                this.timeOfDay += this.cycleSpeed;
                if (this.timeOfDay >= 1.0) this.timeOfDay = 0.0;
                
                this.updateLighting();
            }
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
        
        // Update sky color
        this.updateSkyColor();
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
        if (this.timeOfDay > 0.25 && this.timeOfDay < 0.75) { // Day
            this.worldSettings.setAtmosphericFogDensity(0.05);
            this.worldSettings.setAtmosphericFogColor(200, 220, 255);
        } else { // Night
            this.worldSettings.setAtmosphericFogDensity(0.15);
            this.worldSettings.setAtmosphericFogColor(50, 50, 100);
        }
    }
    
    private updateSkyColor() {
        // Update sky color based on time of day
        if (this.timeOfDay < 0.25) { // Dawn
            this.worldSettings.setSkyColor(255, 200, 150);
        } else if (this.timeOfDay < 0.75) { // Day
            this.worldSettings.setSkyColor(135, 206, 235);
        } else { // Dusk/Night
            this.worldSettings.setSkyColor(25, 25, 112);
        }
    }
    
    // Public methods for external control
    public setTimeOfDay(time: number) {
        this.timeOfDay = Math.max(0, Math.min(1, time));
        this.updateLighting();
    }
    
    public setCycleSpeed(speed: number) {
        this.cycleSpeed = speed;
    }
    
    public pauseCycle() {
        this.isPaused = true;
    }
    
    public resumeCycle() {
        this.isPaused = false;
    }
}
```

### Advanced Day/Night Features

**Seasonal Variations:**
```typescript
class SeasonalDayNightCycle extends DayNightCycle {
    private currentSeason: string = "summer";
    private seasonData: any = {
        summer: { dayLength: 0.6, sunIntensity: 1.2, temperature: "warm" },
        autumn: { dayLength: 0.5, sunIntensity: 1.0, temperature: "cool" },
        winter: { dayLength: 0.4, sunIntensity: 0.8, temperature: "cold" },
        spring: { dayLength: 0.5, sunIntensity: 1.0, temperature: "mild" }
    };
    
    override updateSunLight(sunAngle: number) {
        const season = this.seasonData[this.currentSeason];
        
        if (sunAngle > -10 && sunAngle < 10) {
            const intensity = Math.max(0.1, Math.cos(sunAngle * Math.PI / 180) * 5 * season.sunIntensity);
            this.sunLight.setIntensity(intensity);
            
            // Adjust color based on season
            switch (season.temperature) {
                case "warm":
                    this.sunLight.setColor(255, 240, 200);
                    break;
                case "cool":
                    this.sunLight.setColor(255, 255, 255);
                    break;
                case "cold":
                    this.sunLight.setColor(240, 240, 255);
                    break;
                case "mild":
                    this.sunLight.setColor(255, 248, 220);
                    break;
            }
        }
    }
    
    public setSeason(season: string) {
        if (this.seasonData[season]) {
            this.currentSeason = season;
        }
    }
}
```

## Weather-Based Lighting

Create dynamic weather systems that affect lighting conditions.

### Weather System Implementation

**Complete Weather Controller:**
```typescript
class WeatherLightingController extends Horizon.Component {
    private currentWeather: string = "clear";
    private weatherIntensity: number = 0.0; // 0-1
    private baseLighting: Horizon.DirectionalLightGizmo;
    private worldSettings: Horizon.WorldSettings;
    private weatherDuration: number = 0;
    private maxWeatherDuration: number = 300; // 5 minutes
    
    override start() {
        this.baseLighting = this.props.baseLight.as(Horizon.DirectionalLightGizmo);
        this.worldSettings = Horizon.WorldSettings.getInstance();
        this.startWeatherSystem();
    }
    
    private startWeatherSystem() {
        // Simulate weather changes
        setInterval(() => {
            this.weatherDuration++;
            
            if (this.weatherDuration >= this.maxWeatherDuration) {
                this.changeWeather();
                this.weatherDuration = 0;
            }
            
            this.updateWeatherLighting();
        }, 1000);
    }
    
    private changeWeather() {
        const weatherTypes = ["clear", "cloudy", "rainy", "stormy", "foggy"];
        const weights = [0.4, 0.3, 0.2, 0.05, 0.05]; // Probability weights
        
        const random = Math.random();
        let cumulativeWeight = 0;
        
        for (let i = 0; i < weatherTypes.length; i++) {
            cumulativeWeight += weights[i];
            if (random <= cumulativeWeight) {
                this.currentWeather = weatherTypes[i];
                this.weatherIntensity = Math.random();
                break;
            }
        }
    }
    
    private updateWeatherLighting() {
        switch (this.currentWeather) {
            case "clear":
                this.setClearWeather();
                break;
            case "cloudy":
                this.setCloudyWeather();
                break;
            case "rainy":
                this.setRainyWeather();
                break;
            case "stormy":
                this.setStormyWeather();
                break;
            case "foggy":
                this.setFoggyWeather();
                break;
        }
    }
    
    private setClearWeather() {
        this.baseLighting.setIntensity(5.0);
        this.baseLighting.setColor(255, 255, 255);
        
        this.worldSettings.setAtmosphericFogDensity(0.05);
        this.worldSettings.setAtmosphericFogColor(220, 230, 255);
        this.worldSettings.setBloomIntensity(0.6);
    }
    
    private setCloudyWeather() {
        const cloudIntensity = 5.0 - (this.weatherIntensity * 2.0);
        this.baseLighting.setIntensity(cloudIntensity);
        this.baseLighting.setColor(200, 200, 220);
        
        this.worldSettings.setAtmosphericFogDensity(0.1 + this.weatherIntensity * 0.1);
        this.worldSettings.setAtmosphericFogColor(180, 190, 220);
        this.worldSettings.setBloomIntensity(0.4);
    }
    
    private setRainyWeather() {
        this.baseLighting.setIntensity(2.0);
        this.baseLighting.setColor(150, 150, 180);
        
        this.worldSettings.setAtmosphericFogDensity(0.2 + this.weatherIntensity * 0.2);
        this.worldSettings.setAtmosphericFogColor(120, 130, 160);
        this.worldSettings.setBloomIntensity(0.3);
    }
    
    private setStormyWeather() {
        this.baseLighting.setIntensity(1.0);
        this.baseLighting.setColor(100, 100, 120);
        
        this.worldSettings.setAtmosphericFogDensity(0.4 + this.weatherIntensity * 0.3);
        this.worldSettings.setAtmosphericFogColor(80, 80, 100);
        this.worldSettings.setBloomIntensity(0.2);
        
        // Add lightning effects
        this.addLightningEffect();
    }
    
    private setFoggyWeather() {
        this.baseLighting.setIntensity(3.0);
        this.baseLighting.setColor(180, 180, 200);
        
        this.worldSettings.setAtmosphericFogDensity(0.3 + this.weatherIntensity * 0.4);
        this.worldSettings.setAtmosphericFogColor(180, 180, 200);
        this.worldSettings.setBloomIntensity(0.5);
    }
    
    private addLightningEffect() {
        if (Math.random() < 0.1) { // 10% chance per second
            const originalIntensity = this.baseLighting.getIntensity();
            this.baseLighting.setIntensity(originalIntensity * 5);
            
            setTimeout(() => {
                this.baseLighting.setIntensity(originalIntensity);
            }, 100);
        }
    }
}
```

## Performance Considerations

Dynamic lighting systems can be performance-intensive. Here are optimization strategies.

### Light Culling and LOD

**Advanced Light Culling:**
```typescript
class LightCullingSystem extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private player: Horizon.Player;
    private cullingDistance: number = 50.0;
    private lodDistances: number[] = [20, 35, 50]; // Near, medium, far
    
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
            } else if (distance > this.lodDistances[1]) {
                // Far LOD - reduced quality
                light.setIntensity(light.getIntensity() * 0.5);
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(512);
                }
            } else if (distance > this.lodDistances[0]) {
                // Medium LOD - normal quality
                light.setIntensity(light.getIntensity() * 0.8);
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(1024);
                }
            } else {
                // Near LOD - full quality
                light.setIntensity(light.getIntensity());
                if (light instanceof Horizon.DirectionalLightGizmo) {
                    light.setShadowResolution(2048);
                }
            }
        });
    }
}
```

### Update Frequency Optimization

**Smart Update Scheduling:**
```typescript
class OptimizedLightingController extends Horizon.Component {
    private lights: Horizon.LightGizmo[] = [];
    private updateIntervals: Map<Horizon.LightGizmo, number> = new Map();
    private baseUpdateInterval: number = 100; // Base update frequency
    
    override start() {
        this.lights = this.getLightsInScene();
        this.setupOptimizedUpdates();
    }
    
    private setupOptimizedUpdates() {
        this.lights.forEach(light => {
            // Determine update frequency based on light type and importance
            const updateInterval = this.calculateUpdateInterval(light);
            this.updateIntervals.set(light, updateInterval);
            
            // Start individual update loops
            this.startLightUpdate(light, updateInterval);
        });
    }
    
    private calculateUpdateInterval(light: Horizon.LightGizmo): number {
        // Static lights update less frequently
        if (light.getIsStatic()) {
            return this.baseUpdateInterval * 5;
        }
        
        // Dynamic lights update more frequently
        if (light instanceof Horizon.SpotLightGizmo) {
            return this.baseUpdateInterval * 2; // Spot lights are often dynamic
        }
        
        // Ambient lights update least frequently
        if (light.getIntensity() < 1.0) {
            return this.baseUpdateInterval * 3;
        }
        
        return this.baseUpdateInterval;
    }
    
    private startLightUpdate(light: Horizon.LightGizmo, interval: number) {
        setInterval(() => {
            this.updateLight(light);
        }, interval);
    }
    
    private updateLight(light: Horizon.LightGizmo) {
        // Perform light-specific updates
        if (light instanceof Horizon.SpotLightGizmo) {
            this.updateSpotLight(light);
        } else if (light instanceof Horizon.PointLightGizmo) {
            this.updatePointLight(light);
        }
    }
}
```

## Next Steps

Now that you've mastered dynamic lighting systems, explore:

1. **Performance Optimization**: Advanced optimization techniques
2. **Lighting for Specific Genres**: Horror, adventure, puzzle games
3. **Multiplayer Lighting**: Synchronized lighting across players
4. **Lighting Scripts Library**: Reusable lighting components

---

**Ready to Create Dynamic Worlds?**

Experiment with these dynamic lighting techniques to create responsive, living environments. Remember to balance visual impact with performance, and always test your systems with multiple players.

**Practice Exercises:**
1. Create a proximity-based lighting system
2. Implement a complete day/night cycle
3. Build a weather system with multiple conditions
4. Design event-driven lighting for a specific game scenario

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
