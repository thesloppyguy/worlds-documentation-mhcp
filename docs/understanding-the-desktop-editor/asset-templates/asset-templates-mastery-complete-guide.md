# Asset Templates Mastery: From Unity to Horizon Worlds Workflow

**Master the power of Asset Templates to streamline your team's workflow, eliminate merge conflicts, and build collaboratively with unprecedented efficiency.** This comprehensive guide covers everything from basic template creation to advanced team collaboration techniques, showing you how Asset Templates differ from Unity and how to leverage them for maximum productivity in Horizon Worlds.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Basic understanding of Horizon Worlds Desktop Editor and fundamental asset management concepts.

**Estimated Time to Complete**
2-3 hours for full tutorial, 45-60 minutes for basic template setup

## Table of Contents

1. [Understanding Asset Templates](#understanding-asset-templates)
2. [Asset Templates vs Unity Workflow](#asset-templates-vs-unity-workflow)
3. [Creating Your First Template](#creating-your-first-template)
4. [Advanced Template Techniques](#advanced-template-techniques)
5. [Team Collaboration Workflows](#team-collaboration-workflows)
6. [Script Integration with Templates](#script-integration-with-templates)
7. [Template Versioning and Updates](#template-versioning-and-updates)
8. [Troubleshooting Common Issues](#troubleshooting-common-issues)

## Understanding Asset Templates

Asset Templates in Horizon Worlds are powerful tools that allow you to create reusable, shareable components that can be instantiated across your world. Unlike traditional asset duplication, templates maintain connections and can be updated globally.

### What Are Asset Templates?

**Asset Templates are:**
- Reusable asset configurations with embedded scripts
- Shareable across team members
- Updateable globally (change once, updates everywhere)
- Performance-optimized for multiple instances

**Key Benefits:**
- **Consistency**: Ensure uniform assets across your world
- **Efficiency**: Create once, use everywhere
- **Collaboration**: Team members can work simultaneously
- **Maintenance**: Update all instances from a single source

![Asset Template Overview - Diagram showing how templates connect to instances and enable global updates]

### How Templates Differ from Unity

**Unity Prefabs vs Horizon Templates:**

| Unity Prefabs | Horizon Asset Templates |
|---------------|-------------------------|
| File-based system | Database-driven system |
| Version control conflicts | Built-in conflict resolution |
| Manual prefab updates | Automatic template synchronization |
| Limited collaboration | Real-time team collaboration |
| Complex branching | Linear template evolution |

![Unity vs Horizon Comparison - Side-by-side comparison of Unity prefab workflow vs Horizon template workflow]

### Template Architecture

**Core Components:**
1. **Template Definition**: The master template with all properties
2. **Template Instances**: Individual copies placed in your world
3. **Template Updates**: Synchronized changes across all instances
4. **Template Sharing**: Team collaboration features

![Template Architecture Diagram - Visual representation of template components and relationships]

## Asset Templates vs Unity Workflow

Understanding the differences helps you leverage Horizon's unique advantages.

### Unity Prefab Workflow Limitations

**Common Unity Issues:**
- **Merge Conflicts**: Multiple developers editing same prefab
- **Version Control**: Complex branching and merging
- **Update Propagation**: Manual prefab replacement
- **Collaboration Bottlenecks**: Sequential editing requirements

**Unity Workflow Example:**
1. Developer A creates prefab
2. Developer B needs to modify prefab
3. Developer A must finish first
4. Merge conflicts occur
5. Manual resolution required
6. Prefabs become out of sync

![Unity Workflow Problems - Screenshot showing merge conflicts and version control issues]

### Horizon Template Workflow Advantages

**Horizon Benefits:**
- **Parallel Development**: Multiple developers work simultaneously
- **Automatic Sync**: Changes propagate instantly
- **Conflict Resolution**: Built-in merge strategies
- **Real-time Collaboration**: Live template updates

**Horizon Workflow Example:**
1. Developer A creates template
2. Developer B modifies template properties
3. Changes sync automatically
4. All instances update globally
5. No conflicts or manual resolution

![Horizon Workflow Benefits - Screenshot showing seamless template collaboration]

### Template Property System

**Property Types:**
- **Shared Properties**: Updated globally across all instances
- **Instance Properties**: Unique to each template instance
- **Script Properties**: Managed by attached TypeScript scripts
- **Material Properties**: Visual and physical characteristics

![Template Property Panel - Screenshot showing template properties with shared vs instance settings]

## Creating Your First Template

Let's start with a simple template to understand the fundamentals.

### Step 1: Design Your Template

**Template Planning:**
1. **Identify Reusable Elements**: What assets will you use multiple times?
2. **Define Properties**: What should be customizable per instance?
3. **Plan Scripts**: What behaviors should the template have?
4. **Consider Variations**: How will instances differ from each other?

**Example Template: Interactive Door**
- **Shared**: Door model, basic animations, script logic
- **Instance**: Door position, rotation, unlock requirements
- **Scripts**: Door opening/closing, lock system, sound effects

![Template Planning Worksheet - Screenshot showing template design planning interface]

### Step 2: Create the Base Asset

1. **Build Your Asset**
   - Create the visual model in Blender or use existing assets
   - Import into Horizon Worlds Desktop Editor
   - Position and scale appropriately

2. **Add Basic Properties**
   - Set up materials and textures
   - Configure physics properties
   - Add collision detection

![Base Asset Creation - Screenshot showing door asset with materials and physics setup]

### Step 3: Convert to Template

1. **Select Your Asset**
   - Right-click on the asset in the hierarchy
   - Choose **"Convert to Template"**

2. **Configure Template Settings**
   - **Template Name**: Choose a descriptive name
   - **Description**: Add helpful details for team members
   - **Category**: Organize templates by function
   - **Tags**: Add searchable keywords

![Template Conversion Dialog - Screenshot showing template creation dialog with settings]

### Step 4: Define Template Properties

1. **Shared Properties** (Updated globally)
   - **Model**: The 3D mesh and materials
   - **Scripts**: Core behavior logic
   - **Animations**: Door opening/closing animations

2. **Instance Properties** (Unique per instance)
   - **Position**: Where the door is placed
   - **Rotation**: Door orientation
   - **Scale**: Door size variations
   - **Custom Data**: Instance-specific information

![Template Properties Panel - Screenshot showing shared vs instance property configuration]

### Step 5: Add Scripts to Template

```typescript
import { Horizon } from "@horizon/horizon";

class DoorTemplate extends Horizon.Component {
    private isOpen: boolean = false;
    private isLocked: boolean = false;
    private unlockRequirement: string = "";
    
    // Template properties
    propsDefinition = {
        unlockRequirement: { type: Horizon.PropTypes.String, default: "" },
        doorSpeed: { type: Horizon.PropTypes.Number, default: 2.0 },
        soundEnabled: { type: Horizon.PropTypes.Boolean, default: true }
    };
    
    override start() {
        this.setupDoor();
    }
    
    private setupDoor() {
        // Set up door behavior based on template properties
        this.unlockRequirement = this.props.unlockRequirement;
        
        // Add interaction trigger
        this.entity.onTriggerEnter.add((player) => {
            this.handlePlayerInteraction(player);
        });
    }
    
    private handlePlayerInteraction(player: Horizon.Player) {
        if (this.isLocked) {
            this.checkUnlockRequirement(player);
        } else {
            this.toggleDoor();
        }
    }
    
    private toggleDoor() {
        this.isOpen = !this.isOpen;
        this.animateDoor();
        
        if (this.props.soundEnabled) {
            this.playDoorSound();
        }
    }
}

Horizon.Component.register(DoorTemplate);
```

![Template Script Integration - Screenshot showing TypeScript script attached to template with property bindings]

## Advanced Template Techniques

Once you've mastered basic templates, these advanced techniques will help you create more sophisticated and flexible systems.

### Template Inheritance

**Creating Template Hierarchies:**
- **Base Templates**: Fundamental building blocks
- **Specialized Templates**: Extended versions with additional features
- **Template Composition**: Combining multiple templates

**Example: Door Template Hierarchy**
```
Base Door Template
├── Standard Door (basic open/close)
├── Locked Door (requires key/trigger)
├── Automatic Door (motion sensor)
└── Security Door (multiple locks, alarms)
```

![Template Inheritance Diagram - Visual representation of template hierarchy and relationships]

### Dynamic Template Properties

**Runtime Property Updates:**
```typescript
class DynamicTemplate extends Horizon.Component {
    private templateManager: Horizon.TemplateManager;
    
    override start() {
        this.templateManager = Horizon.TemplateManager.getInstance();
        this.setupDynamicUpdates();
    }
    
    private setupDynamicUpdates() {
        // Update template properties based on game state
        setInterval(() => {
            this.updateTemplateProperties();
        }, 1000);
    }
    
    private updateTemplateProperties() {
        const gameState = this.getGameState();
        
        // Update all instances of this template
        this.templateManager.updateTemplateInstances(
            this.entity.getTemplateId(),
            {
                difficulty: gameState.currentDifficulty,
                theme: gameState.currentTheme,
                timeOfDay: gameState.timeOfDay
            }
        );
    }
}
```

![Dynamic Properties Panel - Screenshot showing runtime template property updates]

### Template Variants

**Creating Template Variations:**
1. **Duplicate Template**: Create a copy with modifications
2. **Modify Properties**: Change shared properties for the variant
3. **Add Scripts**: Include variant-specific behaviors
4. **Test Instances**: Verify variant works correctly

**Example: Door Template Variants**
- **Wooden Door**: Natural materials, creaking sounds
- **Metal Door**: Industrial materials, hydraulic sounds
- **Glass Door**: Transparent materials, sliding animation
- **Secret Door**: Hidden appearance, special unlock logic

![Template Variants - Screenshot showing multiple door template variants with different properties]

### Template Optimization

**Performance Best Practices:**
- **LOD Integration**: Use Level of Detail for distant instances
- **Culling**: Disable off-screen template instances
- **Pooling**: Reuse template instances when possible
- **Batch Updates**: Update multiple instances simultaneously

```typescript
class OptimizedTemplate extends Horizon.Component {
    private static instancePool: Horizon.Entity[] = [];
    private static maxPoolSize: number = 20;
    
    static getInstance(): Horizon.Entity {
        if (this.instancePool.length > 0) {
            return this.instancePool.pop()!;
        }
        return this.createNewInstance();
    }
    
    static returnInstance(instance: Horizon.Entity) {
        if (this.instancePool.length < this.maxPoolSize) {
            this.resetInstance(instance);
            this.instancePool.push(instance);
        }
    }
}
```

![Template Optimization Panel - Screenshot showing performance metrics and optimization settings]

## Team Collaboration Workflows

Asset Templates shine in team environments. Learn how to maximize collaboration efficiency.

### Template Sharing Setup

**Sharing Configuration:**
1. **Template Permissions**: Set who can edit templates
2. **Version Control**: Track template changes
3. **Conflict Resolution**: Handle simultaneous edits
4. **Change Notifications**: Alert team to updates

![Template Sharing Settings - Screenshot showing template permission and sharing configuration]

### Parallel Development Workflow

**Team Collaboration Process:**
1. **Template Creation**: Lead developer creates base template
2. **Property Definition**: Define shared vs instance properties
3. **Team Distribution**: Share template with team members
4. **Parallel Development**: Multiple developers work simultaneously
5. **Automatic Sync**: Changes propagate to all team members
6. **Review Process**: Template changes reviewed before finalization

![Parallel Development Diagram - Visual representation of team collaboration workflow]

### Template Review System

**Change Management:**
- **Proposed Changes**: Team members suggest modifications
- **Review Process**: Lead developer reviews changes
- **Approval Workflow**: Changes approved before implementation
- **Rollback Capability**: Revert to previous template versions

![Template Review Interface - Screenshot showing template change review and approval system]

### Template Documentation

**Documentation Best Practices:**
- **Template Purpose**: Clear description of what the template does
- **Property Guide**: Explanation of all properties and their effects
- **Usage Examples**: Sample implementations and use cases
- **Troubleshooting**: Common issues and solutions

```markdown
# Door Template Documentation

## Purpose
Interactive door system with customizable unlock requirements and animations.

## Properties
- `unlockRequirement`: String - Item or condition needed to unlock
- `doorSpeed`: Number - Animation speed (1.0-5.0)
- `soundEnabled`: Boolean - Enable/disable sound effects

## Usage Examples
1. Standard door: Set unlockRequirement to ""
2. Key door: Set unlockRequirement to "golden_key"
3. Switch door: Set unlockRequirement to "lever_activated"

## Troubleshooting
- Door not opening: Check unlockRequirement and player inventory
- No sound: Verify soundEnabled is true and audio files are loaded
```

![Template Documentation - Screenshot showing comprehensive template documentation interface]

## Script Integration with Templates

Templates and scripts work together to create powerful, reusable systems.

### Template-Aware Scripts

**Scripts that Understand Templates:**
```typescript
class TemplateAwareScript extends Horizon.Component {
    private templateId: string;
    private templateProperties: any;
    
    override start() {
        this.templateId = this.entity.getTemplateId();
        this.loadTemplateProperties();
        this.setupTemplateBehavior();
    }
    
    private loadTemplateProperties() {
        const templateManager = Horizon.TemplateManager.getInstance();
        this.templateProperties = templateManager.getTemplateProperties(this.templateId);
    }
    
    private setupTemplateBehavior() {
        // Configure behavior based on template properties
        if (this.templateProperties.type === "interactive") {
            this.setupInteraction();
        } else if (this.templateProperties.type === "decorative") {
            this.setupDecoration();
        }
    }
}
```

![Template-Aware Script - Screenshot showing script that reads and responds to template properties]

### Template Event System

**Template-Specific Events:**
```typescript
class TemplateEventManager extends Horizon.Component {
    override start() {
        this.setupTemplateEvents();
    }
    
    private setupTemplateEvents() {
        // Listen for template property changes
        Horizon.TemplateManager.getInstance().onTemplateUpdated.add((templateId, properties) => {
            if (templateId === this.entity.getTemplateId()) {
                this.handleTemplateUpdate(properties);
            }
        });
        
        // Listen for template instance creation
        Horizon.TemplateManager.getInstance().onInstanceCreated.add((templateId, instance) => {
            if (templateId === this.entity.getTemplateId()) {
                this.handleInstanceCreated(instance);
            }
        });
    }
    
    private handleTemplateUpdate(properties: any) {
        // Update behavior based on new template properties
        this.updateBehavior(properties);
    }
}
```

![Template Event System - Screenshot showing template event handling and property updates]

### Script Property Binding

**Binding Script Properties to Templates:**
```typescript
class PropertyBoundScript extends Horizon.Component {
    // Properties that sync with template
    propsDefinition = {
        health: { type: Horizon.PropTypes.Number, default: 100 },
        damage: { type: Horizon.PropTypes.Number, default: 25 },
        speed: { type: Horizon.PropTypes.Number, default: 2.0 }
    };
    
    override start() {
        this.syncWithTemplate();
    }
    
    private syncWithTemplate() {
        // Update script properties when template changes
        Horizon.TemplateManager.getInstance().onTemplateUpdated.add((templateId, properties) => {
            if (templateId === this.entity.getTemplateId()) {
                this.updateScriptProperties(properties);
            }
        });
    }
    
    private updateScriptProperties(templateProperties: any) {
        if (templateProperties.health !== undefined) {
            this.props.health = templateProperties.health;
        }
        if (templateProperties.damage !== undefined) {
            this.props.damage = templateProperties.damage;
        }
        if (templateProperties.speed !== undefined) {
            this.props.speed = templateProperties.speed;
        }
    }
}
```

![Property Binding - Screenshot showing script properties bound to template properties]

## Template Versioning and Updates

Managing template evolution and updates across your project.

### Template Version Control

**Version Management:**
- **Version Numbers**: Semantic versioning (major.minor.patch)
- **Change Logs**: Document all template modifications
- **Backward Compatibility**: Ensure updates don't break existing instances
- **Migration Scripts**: Automate updates for existing instances

![Template Version Control - Screenshot showing template version history and change tracking]

### Template Update Strategies

**Update Approaches:**
1. **Incremental Updates**: Small, frequent changes
2. **Major Updates**: Significant template overhauls
3. **Optional Updates**: Allow instances to opt-in to updates
4. **Forced Updates**: Critical updates applied to all instances

**Update Process:**
```typescript
class TemplateUpdateManager extends Horizon.Component {
    private updateTemplate(templateId: string, newProperties: any) {
        const templateManager = Horizon.TemplateManager.getInstance();
        
        // Create backup of current template
        const backup = templateManager.getTemplateProperties(templateId);
        
        // Apply updates
        templateManager.updateTemplate(templateId, newProperties);
        
        // Notify all instances of the update
        templateManager.notifyTemplateUpdate(templateId);
        
        // Log update for version control
        this.logTemplateUpdate(templateId, backup, newProperties);
    }
}
```

![Template Update Process - Screenshot showing template update workflow and version tracking]

### Template Migration

**Handling Breaking Changes:**
```typescript
class TemplateMigration extends Horizon.Component {
    private migrateTemplateInstances(templateId: string, migrationScript: Function) {
        const templateManager = Horizon.TemplateManager.getInstance();
        const instances = templateManager.getTemplateInstances(templateId);
        
        instances.forEach(instance => {
            // Apply migration script to each instance
            migrationScript(instance);
            
            // Update instance properties
            instance.updateProperties();
        });
    }
    
    // Example migration script
    private migrateDoorTemplate(instance: Horizon.Entity) {
        const oldProperties = instance.getProperties();
        
        // Convert old property format to new format
        if (oldProperties.requiresKey) {
            instance.setProperty("unlockRequirement", oldProperties.keyType);
            instance.removeProperty("requiresKey");
            instance.removeProperty("keyType");
        }
    }
}
```

![Template Migration - Screenshot showing template migration process and property conversion]

## Troubleshooting Common Issues

Common template problems and their solutions.

### Template Not Updating

**Possible Causes:**
- Template not properly converted
- Instance properties overriding shared properties
- Script conflicts preventing updates
- Network synchronization issues

**Solutions:**
1. Verify template conversion was successful
2. Check instance vs shared property settings
3. Review script logic for conflicts
4. Test network connectivity

![Template Update Debug - Screenshot showing template update troubleshooting panel]

### Instance Properties Not Working

**Possible Causes:**
- Properties not marked as instance-specific
- Script not reading instance properties correctly
- Template property conflicts
- Property type mismatches

**Solutions:**
1. Verify property configuration in template
2. Check script property reading logic
3. Ensure property types match expectations
4. Test property inheritance

### Performance Issues

**Possible Causes:**
- Too many template instances
- Inefficient template scripts
- Large template property sets
- Poor template optimization

**Solutions:**
1. Implement template instance pooling
2. Optimize template scripts
3. Reduce template property complexity
4. Use LOD for distant instances

![Performance Optimization - Screenshot showing template performance metrics and optimization tools]

### Collaboration Conflicts

**Possible Causes:**
- Simultaneous template edits
- Conflicting property changes
- Version control issues
- Permission problems

**Solutions:**
1. Use template review system
2. Implement change notifications
3. Set up proper permissions
4. Use version control features

![Collaboration Conflict Resolution - Screenshot showing conflict resolution interface]

## Best Practices Summary

### Do's ✅
- Plan templates carefully before creation
- Use descriptive template names and documentation
- Test templates thoroughly before sharing
- Implement proper version control
- Optimize templates for performance
- Use template events for dynamic behavior

### Don'ts ❌
- Create overly complex templates initially
- Ignore template performance implications
- Forget to document template usage
- Make breaking changes without migration
- Overuse shared properties
- Neglect template testing

## Resources and References

### Official Documentation
- **[Asset Templates Guide](https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/assets/asset-templates)**
- **[Template API Reference](https://developers.meta.com/horizon-worlds/learn/documentation/typescript/template-api)**
- **[Collaboration Tools](https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/collaboration)**

### Additional Learning
- **Template Design Patterns**: Study reusable component design
- **Team Collaboration**: Learn effective team workflows
- **Version Control**: Understand template evolution management

### Community Resources
- **Horizon Worlds Creator Community**: Share template designs
- **Discord Servers**: Join template discussion groups
- **Template Libraries**: Find community-created templates

---

**Ready to Master Asset Templates?**

Start with simple templates and gradually build complexity. Remember, good templates are reusable, well-documented, and performance-optimized. Focus on creating templates that solve real problems for your team and enhance your development workflow.

**Next Steps:**
1. Create your first basic template
2. Experiment with shared vs instance properties
3. Add scripts to your templates
4. Share templates with your team
5. Build a template library for your projects!

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
