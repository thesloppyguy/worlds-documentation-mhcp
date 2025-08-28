# Advanced Template Techniques: Script Integration and Team Collaboration

**Master advanced template techniques, script integration, and team collaboration workflows.** This guide covers sophisticated template systems, script integration, team collaboration, and advanced optimization strategies for professional template development.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Completion of Asset Templates Fundamentals and basic TypeScript scripting experience.

**Estimated Time to Complete**
60-90 minutes for advanced concepts, 45 minutes for implementation

## Table of Contents

1. [Advanced Template Systems](#advanced-template-systems)
2. [Script Integration with Templates](#script-integration-with-templates)
3. [Team Collaboration Workflows](#team-collaboration-workflows)
4. [Template Versioning and Updates](#template-versioning-and-updates)
5. [Advanced Optimization](#advanced-optimization)

## Advanced Template Systems

Create sophisticated template systems with advanced features and complex behaviors.

### Template Variants System

**Dynamic Template Variants:**
```typescript
class TemplateVariantManager extends Horizon.Component {
    private variants: Map<string, TemplateVariant> = new Map();
    private currentVariant: string = "default";
    
    override start() {
        this.setupVariantSystem();
    }
    
    private setupVariantSystem() {
        this.defineVariants();
        this.setupVariantSwitching();
    }
    
    private defineVariants() {
        // Define different template variants
        this.variants.set("default", {
            name: "Default",
            properties: {
                color: new Horizon.Color(0.8, 0.6, 0.4),
                scale: new Horizon.Vector3(1, 1, 1),
                material: "wood",
                behavior: "standard"
            }
        });
        
        this.variants.set("premium", {
            name: "Premium",
            properties: {
                color: new Horizon.Color(0.9, 0.7, 0.5),
                scale: new Horizon.Vector3(1.1, 1.1, 1.1),
                material: "premium_wood",
                behavior: "enhanced"
            }
        });
    }
    
    public switchVariant(variantName: string) {
        const variant = this.variants.get(variantName);
        if (!variant) return;
        
        this.currentVariant = variantName;
        this.applyVariant(variant);
    }
    
    private applyVariant(variant: TemplateVariant) {
        this.setColor(variant.properties.color);
        this.setScale(variant.properties.scale);
        this.setMaterial(variant.properties.material);
        this.updateBehavior(variant.properties.behavior);
    }
}

interface TemplateVariant {
    name: string;
    properties: {
        color: Horizon.Color;
        scale: Horizon.Vector3;
        material: string;
        behavior: string;
    };
}
```

### Template Composition System

**Composable Template Architecture:**
```typescript
class ComposableTemplate extends Horizon.Component {
    private components: Map<string, TemplateComponent> = new Map();
    private componentOrder: string[] = [];
    
    override start() {
        this.setupComposition();
    }
    
    private setupComposition() {
        this.defineComponents();
        this.composeTemplate();
    }
    
    private defineComponents() {
        this.components.set("base", {
            name: "Base",
            required: true,
            properties: {
                position: new Horizon.Vector3(0, 0, 0),
                scale: new Horizon.Vector3(1, 1, 1),
                material: "base_material"
            }
        });
        
        this.components.set("interactive", {
            name: "Interactive",
            required: false,
            properties: {
                clickable: true,
                hoverable: true,
                soundEnabled: true
            }
        });
    }
    
    private composeTemplate() {
        this.componentOrder = ["base", "interactive"];
        this.applyComposition();
    }
    
    private applyComposition() {
        this.componentOrder.forEach(componentName => {
            const component = this.components.get(componentName);
            if (component) {
                this.applyComponent(component);
            }
        });
    }
    
    private applyComponent(component: TemplateComponent) {
        Object.entries(component.properties).forEach(([key, value]) => {
            this.setComponentProperty(key, value);
        });
    }
}

interface TemplateComponent {
    name: string;
    required: boolean;
    properties: Record<string, any>;
}
```

## Script Integration with Templates

Integrate sophisticated scripts with templates for dynamic behavior and advanced functionality.

### Template-Aware Scripts

**Scripts that Understand Templates:**
```typescript
class TemplateAwareScript extends Horizon.Component {
    private templateId: string;
    private templateProperties: Map<string, any> = new Map();
    
    override start() {
        this.initializeTemplateAwareness();
    }
    
    private initializeTemplateAwareness() {
        this.templateId = this.getTemplateId();
        this.loadTemplateProperties();
    }
    
    private loadTemplateProperties() {
        this.templateProperties.set("interactionRange", 2.0);
        this.templateProperties.set("animationSpeed", 1.0);
        this.templateProperties.set("soundEnabled", true);
    }
    
    public getTemplateProperty(propertyName: string): any {
        return this.templateProperties.get(propertyName);
    }
    
    public setTemplateProperty(propertyName: string, value: any) {
        this.templateProperties.set(propertyName, value);
        this.onPropertyChanged(propertyName, value);
    }
    
    private onPropertyChanged(propertyName: string, value: any) {
        switch (propertyName) {
            case "interactionRange":
                this.updateInteractionRange(value);
                break;
            case "animationSpeed":
                this.updateAnimationSpeed(value);
                break;
        }
    }
}
```

### Template Event System

**Event-Driven Template Architecture:**
```typescript
class TemplateEventManager extends Horizon.Component {
    private eventHandlers: Map<string, Function[]> = new Map();
    
    override start() {
        this.setupEventSystem();
    }
    
    private setupEventSystem() {
        this.registerDefaultEvents();
        this.setupEventListeners();
    }
    
    private registerDefaultEvents() {
        this.registerEvent("template_created");
        this.registerEvent("template_updated");
        this.registerEvent("property_changed");
        this.registerEvent("interaction_started");
    }
    
    public registerEvent(eventName: string) {
        if (!this.eventHandlers.has(eventName)) {
            this.eventHandlers.set(eventName, []);
        }
    }
    
    public addEventHandler(eventName: string, handler: Function) {
        if (!this.eventHandlers.has(eventName)) {
            this.registerEvent(eventName);
        }
        this.eventHandlers.get(eventName)!.push(handler);
    }
    
    public triggerEvent(eventName: string, data: any = {}) {
        const handlers = this.eventHandlers.get(eventName);
        if (handlers) {
            const event = {
                name: eventName,
                data: data,
                timestamp: Date.now(),
                templateId: this.getTemplateId()
            };
            
            handlers.forEach(handler => {
                try {
                    handler(event);
                } catch (error) {
                    console.error(`Error in event handler for ${eventName}:`, error);
                }
            });
        }
    }
}
```

## Team Collaboration Workflows

Implement effective team collaboration workflows for template development and management.

### Template Sharing System

**Team Template Sharing:**
```typescript
class TemplateSharingManager extends Horizon.Component {
    private sharedTemplates: Map<string, SharedTemplate> = new Map();
    private teamMembers: Horizon.Player[] = [];
    private permissions: Map<string, PermissionLevel> = new Map();
    
    override start() {
        this.setupSharingSystem();
    }
    
    private setupSharingSystem() {
        this.setupTeamManagement();
        this.setupPermissionSystem();
    }
    
    public shareTemplate(template: any) {
        const sharedTemplate: SharedTemplate = {
            id: template.id,
            name: template.name,
            version: template.version,
            owner: template.owner,
            sharedBy: this.getCurrentUser(),
            sharedAt: Date.now(),
            permissions: new Map()
        };
        
        this.teamMembers.forEach(member => {
            const permission = this.permissions.get(member.getUserId()) || PermissionLevel.Viewer;
            sharedTemplate.permissions.set(member.getUserId(), permission);
        });
        
        this.sharedTemplates.set(template.id, sharedTemplate);
    }
    
    public setTemplatePermission(templateId: string, userId: string, permission: PermissionLevel) {
        const template = this.sharedTemplates.get(templateId);
        if (template) {
            template.permissions.set(userId, permission);
        }
    }
    
    public canEditTemplate(templateId: string, userId: string): boolean {
        const permission = this.getTemplatePermission(templateId, userId);
        return permission >= PermissionLevel.Editor;
    }
}

interface SharedTemplate {
    id: string;
    name: string;
    version: string;
    owner: string;
    sharedBy: Horizon.Player;
    sharedAt: number;
    permissions: Map<string, PermissionLevel>;
}

enum PermissionLevel {
    Viewer = 0,
    Editor = 1,
    Admin = 2,
    Owner = 3
}
```

## Template Versioning and Updates

Implement robust versioning and update systems for template management.

### Template Version Control

**Semantic Versioning System:**
```typescript
class TemplateVersionControl extends Horizon.Component {
    private versionHistory: TemplateVersion[] = [];
    private currentVersion: string = "1.0.0";
    
    override start() {
        this.setupVersionControl();
    }
    
    private setupVersionControl() {
        this.initializeVersionHistory();
    }
    
    public createNewVersion(versionType: VersionType, changes: string[], breakingChanges: boolean = false) {
        const newVersion = this.calculateNewVersion(versionType);
        
        const version: TemplateVersion = {
            version: newVersion,
            timestamp: Date.now(),
            author: this.getCurrentUser(),
            changes: changes,
            breakingChanges: breakingChanges,
            templateData: this.getTemplateData()
        };
        
        this.versionHistory.push(version);
        this.currentVersion = newVersion;
        
        return version;
    }
    
    private calculateNewVersion(versionType: VersionType): string {
        const [major, minor, patch] = this.currentVersion.split('.').map(Number);
        
        switch (versionType) {
            case VersionType.Major:
                return `${major + 1}.0.0`;
            case VersionType.Minor:
                return `${major}.${minor + 1}.0`;
            case VersionType.Patch:
                return `${major}.${minor}.${patch + 1}`;
            default:
                return `${major}.${minor}.${patch + 1}`;
        }
    }
    
    public getVersionHistory(): TemplateVersion[] {
        return [...this.versionHistory];
    }
    
    public rollbackToVersion(version: string) {
        const targetVersion = this.getVersion(version);
        if (!targetVersion) {
            throw new Error(`Version ${version} not found`);
        }
        
        this.applyTemplateData(targetVersion.templateData);
        this.createNewVersion(VersionType.Patch, [`Rollback to version ${version}`], false);
    }
}

interface TemplateVersion {
    version: string;
    timestamp: number;
    author: Horizon.Player;
    changes: string[];
    breakingChanges: boolean;
    templateData: any;
}

enum VersionType {
    Major,
    Minor,
    Patch
}
```

## Advanced Optimization

Optimize templates for performance and scalability.

### Template Performance Optimization

**Performance Optimization Techniques:**
```typescript
class OptimizedTemplate extends Horizon.Component {
    private optimizationLevel: OptimizationLevel = OptimizationLevel.Balanced;
    private performanceMetrics: PerformanceMetrics;
    
    override start() {
        this.setupOptimization();
    }
    
    private setupOptimization() {
        this.detectDeviceCapabilities();
        this.applyOptimizations();
        this.startPerformanceMonitoring();
    }
    
    private detectDeviceCapabilities() {
        const isMobile = this.isMobileDevice();
        const memory = navigator.deviceMemory || 4;
        
        if (isMobile && memory < 4) {
            this.optimizationLevel = OptimizationLevel.Maximum;
        } else if (isMobile) {
            this.optimizationLevel = OptimizationLevel.High;
        } else {
            this.optimizationLevel = OptimizationLevel.Balanced;
        }
    }
    
    private applyOptimizations() {
        switch (this.optimizationLevel) {
            case OptimizationLevel.Maximum:
                this.applyMaximumOptimizations();
                break;
            case OptimizationLevel.High:
                this.applyHighOptimizations();
                break;
            case OptimizationLevel.Balanced:
                this.applyBalancedOptimizations();
                break;
        }
    }
    
    private applyMaximumOptimizations() {
        this.setMaxPolygonCount(500);
        this.setMaxTextureSize(512);
        this.setMaxAnimationCount(2);
        this.setMaxAudioSources(1);
        this.disableAdvancedFeatures();
    }
    
    private applyHighOptimizations() {
        this.setMaxPolygonCount(1000);
        this.setMaxTextureSize(1024);
        this.setMaxAnimationCount(3);
        this.setMaxAudioSources(2);
        this.limitAdvancedFeatures();
    }
    
    private applyBalancedOptimizations() {
        this.setMaxPolygonCount(2000);
        this.setMaxTextureSize(2048);
        this.setMaxAnimationCount(5);
        this.setMaxAudioSources(3);
        this.enableAllFeatures();
    }
    
    private startPerformanceMonitoring() {
        setInterval(() => {
            this.updatePerformanceMetrics();
        }, 1000);
    }
    
    private updatePerformanceMetrics() {
        this.performanceMetrics = {
            frameRate: this.getFrameRate(),
            memoryUsage: this.getMemoryUsage(),
            activeInstances: this.getActiveInstanceCount(),
            updateTime: this.getUpdateTime()
        };
        
        this.checkPerformanceThresholds();
    }
    
    private checkPerformanceThresholds() {
        if (this.performanceMetrics.frameRate < 30) {
            this.increaseOptimization();
        }
    }
    
    private increaseOptimization() {
        if (this.optimizationLevel < OptimizationLevel.Maximum) {
            this.optimizationLevel++;
            this.applyOptimizations();
        }
    }
    
    public getPerformanceMetrics(): PerformanceMetrics {
        return this.performanceMetrics;
    }
}

interface PerformanceMetrics {
    frameRate: number;
    memoryUsage: number;
    activeInstances: number;
    updateTime: number;
}

enum OptimizationLevel {
    Maximum,
    High,
    Balanced,
    Quality
}
```

---

**Advanced Template Implementation Complete**

You now have a comprehensive understanding of advanced template techniques, script integration, team collaboration, and version control systems. These systems enable professional template development workflows and team collaboration.

**Next Steps:**
1. Implement template variants and composition systems
2. Create template-aware scripts for dynamic behavior
3. Set up team collaboration and review workflows
4. Establish version control and update systems

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
