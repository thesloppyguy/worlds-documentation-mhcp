# Asset Templates Fundamentals: Understanding Templates in Horizon Worlds

**Master the foundational principles of asset templates to create reusable, efficient content.** This guide covers the essential concepts of template systems, their benefits, and basic implementation that form the foundation for all template-based workflows in Meta Horizon Worlds.

**Creator Skill Level**
Beginner to Intermediate

**Recommended Background Knowledge**
Basic understanding of the Horizon Worlds Desktop Editor and TypeScript scripting.

**Estimated Time to Complete**
45-60 minutes for fundamentals, 30 minutes for basic setup

## Table of Contents

1. [Understanding Asset Templates](#understanding-asset-templates)
2. [Template vs Unity Prefab Comparison](#template-vs-unity-prefab-comparison)
3. [Basic Template Creation](#basic-template-creation)
4. [Template Properties and Inheritance](#template-properties-and-inheritance)
5. [Template Best Practices](#template-best-practices)

## Understanding Asset Templates

Asset templates are the cornerstone of efficient content creation in Horizon Worlds. They allow you to create reusable components that maintain consistency while enabling customization and optimization.

### What are Asset Templates?

**Template Definition:**
- Reusable asset configurations that can be instantiated multiple times
- Centralized asset management with shared properties and behaviors
- Database-driven system for automatic synchronization
- Foundation for team collaboration and content scaling

### Template System Components

**Core Elements:**
- **Template Definition**: Base configuration and properties
- **Template Instances**: Individual copies of templates in worlds
- **Shared Properties**: Properties that affect all instances
- **Instance Properties**: Properties unique to each instance
- **Template Inheritance**: Hierarchical template relationships

### Template System Benefits

**Development Efficiency:**
- **Rapid Prototyping**: Quick iteration with template-based workflows
- **Consistency**: Maintain visual and behavioral consistency across worlds
- **Reusability**: Create once, use everywhere
- **Scalability**: Efficiently manage large content libraries

**Team Collaboration:**
- **Shared Assets**: Team members can use the same templates
- **Version Control**: Automatic updates across all instances
- **Standardization**: Establish team-wide asset standards
- **Parallel Development**: Multiple creators can work simultaneously

**Performance Optimization:**
- **Memory Efficiency**: Shared resources reduce memory usage
- **Loading Optimization**: Templates can be cached and optimized
- **Batch Updates**: Update multiple instances simultaneously
- **Resource Management**: Centralized control over asset resources

## Template vs Unity Prefab Comparison

Understanding the differences between Horizon Worlds templates and Unity prefabs helps you leverage the unique advantages of the template system.

### Key Differences

**File System vs Database:**
- **Unity Prefabs**: File-based system with local storage
- **Horizon Templates**: Database-driven with cloud synchronization

**Version Control:**
- **Unity Prefabs**: Manual version control and merge conflicts
- **Horizon Templates**: Automatic synchronization and conflict resolution

**Collaboration:**
- **Unity Prefabs**: Requires manual sharing and synchronization
- **Horizon Templates**: Real-time collaboration with automatic updates

**Performance:**
- **Unity Prefabs**: Local optimization and caching
- **Horizon Templates**: Cloud-optimized with automatic LOD

### Template Advantages

**Automatic Synchronization:**
```typescript
// Template changes automatically propagate to all instances
class TemplateSyncExample extends Horizon.Component {
    private templateId: string = "door_template_v1";
    private instanceId: string;
    
    override start() {
        this.setupTemplateSync();
    }
    
    private setupTemplateSync() {
        // Template changes automatically update all instances
        this.onTemplateUpdate.add((templateData) => {
            this.applyTemplateUpdate(templateData);
        });
    }
    
    private applyTemplateUpdate(templateData: any) {
        // Automatically apply template changes
        this.updateVisualProperties(templateData.visual);
        this.updateBehaviorProperties(templateData.behavior);
        this.updateScriptProperties(templateData.scripts);
    }
}
```

**Real-time Collaboration:**
```typescript
// Multiple creators can work on the same template simultaneously
class CollaborativeTemplate extends Horizon.Component {
    private collaborators: Horizon.Player[] = [];
    private editMode: boolean = false;
    
    override start() {
        this.setupCollaboration();
    }
    
    private setupCollaboration() {
        // Real-time collaboration features
        this.onCollaboratorJoin.add((player) => {
            this.addCollaborator(player);
        });
        
        this.onCollaboratorLeave.add((player) => {
            this.removeCollaborator(player);
        });
    }
    
    private addCollaborator(player: Horizon.Player) {
        this.collaborators.push(player);
        this.notifyCollaborationChange();
    }
    
    private removeCollaborator(player: Horizon.Player) {
        const index = this.collaborators.indexOf(player);
        if (index > -1) {
            this.collaborators.splice(index, 1);
        }
        this.notifyCollaborationChange();
    }
    
    private notifyCollaborationChange() {
        console.log(`Template has ${this.collaborators.length} active collaborators`);
    }
}
```

## Basic Template Creation

Create your first asset template with step-by-step guidance.

### Simple Door Template

**Basic Door Template Implementation:**
```typescript
class DoorTemplate extends Horizon.Component {
    // Shared properties (affect all instances)
    private isOpen: boolean = false;
    private openSpeed: number = 2.0;
    private openAngle: number = 90;
    private autoClose: boolean = true;
    private autoCloseDelay: number = 3000;
    
    // Instance properties (unique to each instance)
    private doorColor: Horizon.Color = new Horizon.Color(0.8, 0.6, 0.4);
    private doorMaterial: string = "wood";
    private doorSize: Horizon.Vector3 = new Horizon.Vector3(1, 2, 0.1);
    
    override start() {
        this.setupDoor();
    }
    
    private setupDoor() {
        // Set up door properties
        this.setDoorProperties();
        this.setupInteractions();
        this.setupVisuals();
    }
    
    private setDoorProperties() {
        // Apply template properties
        this.setOpenSpeed(this.openSpeed);
        this.setOpenAngle(this.openAngle);
        this.setAutoClose(this.autoClose);
        this.setAutoCloseDelay(this.autoCloseDelay);
    }
    
    private setupInteractions() {
        // Set up player interactions
        this.onPlayerTouch.add((player) => {
            this.toggleDoor();
        });
        
        // Set up proximity detection
        this.onPlayerProximity.add((player, distance) => {
            if (distance < 2.0 && !this.isOpen) {
                this.openDoor();
            }
        });
    }
    
    private setupVisuals() {
        // Apply visual properties
        this.setColor(this.doorColor);
        this.setMaterial(this.doorMaterial);
        this.setScale(this.doorSize);
    }
    
    private toggleDoor() {
        if (this.isOpen) {
            this.closeDoor();
        } else {
            this.openDoor();
        }
    }
    
    private openDoor() {
        if (this.isOpen) return;
        
        this.isOpen = true;
        this.animateDoor(true);
        
        if (this.autoClose) {
            setTimeout(() => {
                this.closeDoor();
            }, this.autoCloseDelay);
        }
    }
    
    private closeDoor() {
        if (!this.isOpen) return;
        
        this.isOpen = false;
        this.animateDoor(false);
    }
    
    private animateDoor(open: boolean) {
        const targetRotation = open ? this.openAngle : 0;
        const currentRotation = this.getRotation().y;
        
        // Smooth door animation
        this.animateRotation(
            new Horizon.Vector3(0, currentRotation, 0),
            new Horizon.Vector3(0, targetRotation, 0),
            this.openSpeed
        );
    }
    
    // Template update methods
    public updateTemplateProperties(properties: any) {
        if (properties.openSpeed !== undefined) {
            this.openSpeed = properties.openSpeed;
        }
        if (properties.autoClose !== undefined) {
            this.autoClose = properties.autoClose;
        }
        if (properties.autoCloseDelay !== undefined) {
            this.autoCloseDelay = properties.autoCloseDelay;
        }
        
        this.setDoorProperties();
    }
    
    public updateInstanceProperties(properties: any) {
        if (properties.doorColor !== undefined) {
            this.doorColor = properties.doorColor;
            this.setColor(this.doorColor);
        }
        if (properties.doorMaterial !== undefined) {
            this.doorMaterial = properties.doorMaterial;
            this.setMaterial(this.doorMaterial);
        }
        if (properties.doorSize !== undefined) {
            this.doorSize = properties.doorSize;
            this.setScale(this.doorSize);
        }
    }
}
```

### Light Switch Template

**Interactive Light Switch Template:**
```typescript
class LightSwitchTemplate extends Horizon.Component {
    // Shared properties
    private isOn: boolean = false;
    private toggleDelay: number = 500;
    private soundEnabled: boolean = true;
    private hapticEnabled: boolean = true;
    
    // Instance properties
    private switchColor: Horizon.Color = new Horizon.Color(0.2, 0.2, 0.2);
    private lightColor: Horizon.Color = new Horizon.Color(1, 1, 1);
    private lightIntensity: number = 1.0;
    private controlledLights: Horizon.Entity[] = [];
    
    override start() {
        this.setupLightSwitch();
    }
    
    private setupLightSwitch() {
        this.setupVisuals();
        this.setupInteractions();
        this.findControlledLights();
    }
    
    private setupVisuals() {
        // Set switch appearance
        this.setColor(this.switchColor);
        this.updateSwitchVisual();
    }
    
    private setupInteractions() {
        // Player interaction
        this.onPlayerTouch.add((player) => {
            this.toggleSwitch();
        });
        
        // Proximity interaction
        this.onPlayerProximity.add((player, distance) => {
            if (distance < 1.5) {
                this.showInteractionHint();
            } else {
                this.hideInteractionHint();
            }
        });
    }
    
    private findControlledLights() {
        // Find lights with matching tag
        this.controlledLights = this.getEntitiesWithTag("ControlledLight");
    }
    
    private toggleSwitch() {
        if (this.isOn) {
            this.turnOff();
        } else {
            this.turnOn();
        }
    }
    
    private turnOn() {
        this.isOn = true;
        this.updateSwitchVisual();
        this.controlLights(true);
        this.playToggleEffects();
    }
    
    private turnOff() {
        this.isOn = false;
        this.updateSwitchVisual();
        this.controlLights(false);
        this.playToggleEffects();
    }
    
    private updateSwitchVisual() {
        // Update switch appearance based on state
        const color = this.isOn ? 
            new Horizon.Color(0.8, 0.8, 0.2) : 
            this.switchColor;
        this.setColor(color);
    }
    
    private controlLights(on: boolean) {
        this.controlledLights.forEach(light => {
            if (on) {
                light.setColor(this.lightColor);
                light.setIntensity(this.lightIntensity);
            } else {
                light.setIntensity(0);
            }
        });
    }
    
    private playToggleEffects() {
        if (this.soundEnabled) {
            this.playToggleSound();
        }
        
        if (this.hapticEnabled) {
            this.triggerHapticFeedback();
        }
    }
    
    private showInteractionHint() {
        // Show interaction hint to player
        this.displayHint("Touch to toggle lights");
    }
    
    private hideInteractionHint() {
        // Hide interaction hint
        this.hideHint();
    }
    
    // Template update methods
    public updateTemplateProperties(properties: any) {
        if (properties.toggleDelay !== undefined) {
            this.toggleDelay = properties.toggleDelay;
        }
        if (properties.soundEnabled !== undefined) {
            this.soundEnabled = properties.soundEnabled;
        }
        if (properties.hapticEnabled !== undefined) {
            this.hapticEnabled = properties.hapticEnabled;
        }
    }
    
    public updateInstanceProperties(properties: any) {
        if (properties.switchColor !== undefined) {
            this.switchColor = properties.switchColor;
            this.updateSwitchVisual();
        }
        if (properties.lightColor !== undefined) {
            this.lightColor = properties.lightColor;
            if (this.isOn) {
                this.controlLights(true);
            }
        }
        if (properties.lightIntensity !== undefined) {
            this.lightIntensity = properties.lightIntensity;
            if (this.isOn) {
                this.controlLights(true);
            }
        }
    }
}
```

## Template Properties and Inheritance

Understanding how template properties work and how inheritance enables powerful customization.

### Property Types

**Shared Properties:**
- Affect all instances of a template
- Updated centrally and propagated automatically
- Used for behavior and functionality changes

**Instance Properties:**
- Unique to each template instance
- Allow customization without affecting other instances
- Used for visual and positional customization

### Template Inheritance

**Hierarchical Templates:**
```typescript
class BaseFurnitureTemplate extends Horizon.Component {
    // Base properties for all furniture
    protected durability: number = 100;
    protected weight: number = 10;
    protected material: string = "wood";
    protected canMove: boolean = true;
    
    override start() {
        this.setupBaseFurniture();
    }
    
    private setupBaseFurniture() {
        this.setupPhysics();
        this.setupInteractions();
    }
    
    private setupPhysics() {
        this.setMass(this.weight);
        this.setMaterial(this.material);
    }
    
    private setupInteractions() {
        if (this.canMove) {
            this.onPlayerGrab.add((player) => {
                this.handleGrab(player);
            });
        }
    }
    
    protected handleGrab(player: Horizon.Player) {
        // Base grab behavior
        console.log("Furniture grabbed");
    }
}

class ChairTemplate extends BaseFurnitureTemplate {
    // Chair-specific properties
    private seatHeight: number = 0.5;
    private hasBackrest: boolean = true;
    private canSit: boolean = true;
    
    override start() {
        super.start();
        this.setupChair();
    }
    
    private setupChair() {
        this.setupSeating();
    }
    
    private setupSeating() {
        if (this.canSit) {
            this.onPlayerTouch.add((player) => {
                this.handleSit(player);
            });
        }
    }
    
    private handleSit(player: Horizon.Player) {
        // Chair-specific sit behavior
        console.log("Player sitting on chair");
    }
}

class TableTemplate extends BaseFurnitureTemplate {
    // Table-specific properties
    private tableHeight: number = 0.8;
    private hasDrawers: boolean = false;
    private surfaceArea: number = 1.0;
    
    override start() {
        super.start();
        this.setupTable();
    }
    
    private setupTable() {
        this.setupSurface();
    }
    
    private setupSurface() {
        // Table-specific setup
        console.log("Table surface ready");
    }
}
```

### Dynamic Property Binding

**Property Binding System:**
```typescript
class DynamicTemplate extends Horizon.Component {
    private propertyBindings: Map<string, Function> = new Map();
    private boundProperties: Map<string, any> = new Map();
    
    override start() {
        this.setupPropertyBindings();
    }
    
    private setupPropertyBindings() {
        // Set up property bindings
        this.bindProperty("color", this.updateColor.bind(this));
        this.bindProperty("scale", this.updateScale.bind(this));
        this.bindProperty("rotation", this.updateRotation.bind(this));
    }
    
    private bindProperty(propertyName: string, updateFunction: Function) {
        this.propertyBindings.set(propertyName, updateFunction);
    }
    
    public updateProperty(propertyName: string, value: any) {
        this.boundProperties.set(propertyName, value);
        
        const updateFunction = this.propertyBindings.get(propertyName);
        if (updateFunction) {
            updateFunction(value);
        }
    }
    
    private updateColor(color: Horizon.Color) {
        this.setColor(color);
    }
    
    private updateScale(scale: Horizon.Vector3) {
        this.setScale(scale);
    }
    
    private updateRotation(rotation: Horizon.Vector3) {
        this.setRotation(rotation);
    }
    
    public getProperty(propertyName: string): any {
        return this.boundProperties.get(propertyName);
    }
    
    public getAllProperties(): Map<string, any> {
        return new Map(this.boundProperties);
    }
}
```

## Template Best Practices

Follow these best practices to create effective and maintainable templates.

### Design Principles

**1. Single Responsibility**
- Each template should have one clear purpose
- Avoid creating overly complex multi-purpose templates
- Break complex templates into smaller, focused components

**2. Consistent Naming**
- Use descriptive, consistent naming conventions
- Include version numbers in template names
- Use clear prefixes for related templates

**3. Property Organization**
- Group related properties together
- Use clear property names and descriptions
- Provide sensible default values

### Performance Considerations

**1. Resource Management**
- Minimize shared resources between instances
- Use efficient property updates
- Implement proper cleanup methods

**2. Optimization Techniques**
- Use LOD systems for complex templates
- Implement culling for distant instances
- Optimize property update frequency

**3. Memory Efficiency**
- Reuse objects when possible
- Implement proper disposal methods
- Monitor memory usage

### Collaboration Guidelines

**1. Template Documentation**
- Document template purpose and usage
- Provide clear property descriptions
- Include usage examples and best practices

**2. Version Control**
- Use semantic versioning for templates
- Maintain change logs
- Test templates before sharing

**3. Team Communication**
- Establish template naming conventions
- Create template usage guidelines
- Regular template reviews and updates

---

**Ready to Create Your First Template?**

Start with simple templates and gradually add complexity. Remember to test your templates thoroughly and document their usage for team collaboration.

**Practice Exercises:**
1. Create a basic door template with open/close functionality
2. Build a light switch template that controls multiple lights
3. Implement a furniture template with inheritance
4. Create a template with dynamic property binding

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
