# Building Dynamic Enemy Waves: From Simple Spawners to Complex AI Behaviors

**Create engaging combat encounters with intelligent enemy AI, dynamic wave progression, and scalable difficulty systems.** This comprehensive guide teaches you how to build sophisticated enemy wave systems in Meta Horizon Worlds, from basic spawn mechanics to advanced AI behaviors that adapt to player skill levels.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Basic TypeScript knowledge and understanding of Horizon Worlds scripting APIs.

**Estimated Time to Complete**
3-4 hours for full tutorial, 1-2 hours for basic wave system

## Table of Contents

1. [Enemy Wave System Fundamentals](#enemy-wave-system-fundamentals)
2. [Enemy Types and Behaviors](#enemy-types-and-behaviors)
3. [Wave Progression Mechanics](#wave-progression-mechanics)
4. [AI Behavior Implementation](#ai-behavior-implementation)
5. [Performance Optimization](#performance-optimization)
6. [Advanced Wave Scenarios](#advanced-wave-scenarios)
7. [Troubleshooting and Debugging](#troubleshooting-and-debugging)

## Enemy Wave System Fundamentals

A well-designed enemy wave system creates engaging, scalable combat encounters that keep players challenged and entertained. The key is balancing difficulty progression with player skill development.

### Core Components of Wave Systems

**1. Spawn Manager**
- Controls when and where enemies appear
- Manages wave timing and progression
- Handles difficulty scaling

**2. Enemy AI Controller**
- Defines enemy behavior patterns
- Manages movement and combat logic
- Handles player interaction

**3. Wave Progression System**
- Tracks current wave and difficulty
- Manages enemy type selection
- Controls wave completion conditions

**4. Player Feedback System**
- Provides clear wave status information
- Rewards successful completion
- Offers difficulty adjustments

![Wave System Architecture - Diagram showing the relationship between spawn manager, AI controllers, and progression systems]

### Wave System Design Principles

**Scalability**: System should handle 1-50+ enemies simultaneously
**Performance**: Optimized for smooth gameplay on all devices
**Balance**: Difficulty increases gradually with player skill
**Variety**: Different enemy types and behaviors keep encounters fresh
**Feedback**: Clear indicators of progress and success

## Enemy Types and Behaviors

Designing diverse enemy types creates more engaging combat encounters. Each enemy type should have distinct behaviors, strengths, and weaknesses.

### Melee Enemies

**Characteristics:**
- Close-range combat specialists
- High health and damage
- Slow movement speed
- Simple AI patterns

**Best For:** Tank enemies, crowd control, close-quarters combat

![Melee Enemy Setup - Screenshot showing melee enemy configuration with health, damage, and movement settings]

**Implementation Example:**
```typescript
class MeleeEnemy extends Horizon.Component {
    private health: number = 100;
    private damage: number = 25;
    private moveSpeed: number = 2.0;
    private attackRange: number = 2.0;
    
    override start() {
        this.setupBehavior();
    }
    
    private setupBehavior() {
        // Simple chase and attack behavior
        setInterval(() => {
            this.chasePlayer();
            this.checkAttackRange();
        }, 100);
    }
    
    private chasePlayer() {
        const player = Horizon.Player.getLocalPlayer();
        if (!player) return;
        
        const direction = player.getPosition().subtract(this.entity.getPosition());
        this.entity.setPosition(this.entity.getPosition().add(direction.normalize().multiply(this.moveSpeed * 0.1)));
    }
}
```

### Ranged Enemies

**Characteristics:**
- Long-range attack capabilities
- Lower health than melee enemies
- Medium movement speed
- More complex AI patterns

**Best For:** Support enemies, area denial, tactical combat

![Ranged Enemy Configuration - Screenshot showing ranged enemy with projectile settings and attack patterns]

**Implementation Example:**
```typescript
class RangedEnemy extends Horizon.Component {
    private health: number = 60;
    private damage: number = 15;
    private attackRange: number = 8.0;
    private lastAttackTime: number = 0;
    private attackCooldown: number = 2000; // 2 seconds
    
    override start() {
        this.setupRangedBehavior();
    }
    
    private setupRangedBehavior() {
        setInterval(() => {
            this.maintainDistance();
            this.attackIfInRange();
        }, 100);
    }
    
    private maintainDistance() {
        const player = Horizon.Player.getLocalPlayer();
        if (!player) return;
        
        const distance = player.getPosition().distance(this.entity.getPosition());
        if (distance < this.attackRange * 0.7) {
            // Move away from player
            const direction = this.entity.getPosition().subtract(player.getPosition());
            this.entity.setPosition(this.entity.getPosition().add(direction.normalize().multiply(1.5 * 0.1)));
        }
    }
    
    private attackIfInRange() {
        const now = Date.now();
        if (now - this.lastAttackTime < this.attackCooldown) return;
        
        const player = Horizon.Player.getLocalPlayer();
        if (!player) return;
        
        const distance = player.getPosition().distance(this.entity.getPosition());
        if (distance <= this.attackRange) {
            this.fireProjectile(player.getPosition());
            this.lastAttackTime = now;
        }
    }
}
```

### Flying Enemies

**Characteristics:**
- 3D movement capabilities
- Variable attack patterns
- Medium health and damage
- Complex AI for aerial combat

**Best For:** Vertical combat, area control, dynamic encounters

![Flying Enemy Setup - Screenshot showing flying enemy with 3D movement and aerial attack patterns]

### Boss Enemies

**Characteristics:**
- High health and damage
- Multiple attack phases
- Special abilities and mechanics
- Complex AI patterns

**Best For:** Wave climax, special events, major challenges

![Boss Enemy Configuration - Screenshot showing boss enemy with multiple health phases and special abilities]

## Wave Progression Mechanics

Creating engaging wave progression keeps players motivated and challenged throughout the experience.

### Basic Wave Structure

**Wave Components:**
1. **Spawn Phase**: Enemies appear over time
2. **Combat Phase**: Players fight enemies
3. **Completion Phase**: Wave ends when all enemies defeated
4. **Intermission**: Brief rest period between waves

![Wave Structure Diagram - Visual representation of wave phases and timing]

### Difficulty Scaling

**Progressive Difficulty Factors:**
- **Enemy Count**: More enemies per wave
- **Enemy Types**: Introduction of new enemy types
- **Spawn Rate**: Faster enemy spawning
- **Enemy Health/Damage**: Increased stats
- **Special Abilities**: New enemy capabilities

```typescript
class WaveManager extends Horizon.Component {
    private currentWave: number = 1;
    private difficultyMultiplier: number = 1.0;
    
    private calculateWaveDifficulty(): WaveConfig {
        return {
            enemyCount: Math.floor(5 + (this.currentWave * 2)),
            spawnRate: Math.max(1000, 3000 - (this.currentWave * 100)),
            enemyHealth: 100 + (this.currentWave * 10),
            enemyDamage: 25 + (this.currentWave * 5),
            specialEnemyChance: Math.min(0.3, this.currentWave * 0.05)
        };
    }
}
```

### Wave Types and Variations

**Standard Waves:**
- Fixed enemy count and types
- Predictable progression
- Good for learning mechanics

**Endless Waves:**
- Continuously increasing difficulty
- No maximum wave limit
- Tests player endurance

**Boss Waves:**
- Special waves with boss enemies
- Unique mechanics and rewards
- Milestone achievements

**Event Waves:**
- Special conditions or modifiers
- Limited-time challenges
- Unique rewards

![Wave Type Examples - Screenshot showing different wave configurations and their properties]

## AI Behavior Implementation

Creating intelligent enemy AI requires careful planning and implementation of behavior patterns.

### State Machine AI

**AI States:**
1. **Idle**: Waiting for player detection
2. **Chase**: Moving toward player
3. **Attack**: Performing attack actions
4. **Retreat**: Moving away from player
5. **Stunned**: Temporarily disabled

```typescript
enum EnemyState {
    IDLE = "idle",
    CHASE = "chase", 
    ATTACK = "attack",
    RETREAT = "retreat",
    STUNNED = "stunned"
}

class EnemyAI extends Horizon.Component {
    private currentState: EnemyState = EnemyState.IDLE;
    private stateTimer: number = 0;
    
    override start() {
        this.startAI();
    }
    
    private startAI() {
        setInterval(() => {
            this.updateAI();
        }, 100);
    }
    
    private updateAI() {
        switch (this.currentState) {
            case EnemyState.IDLE:
                this.handleIdleState();
                break;
            case EnemyState.CHASE:
                this.handleChaseState();
                break;
            case EnemyState.ATTACK:
                this.handleAttackState();
                break;
            case EnemyState.RETREAT:
                this.handleRetreatState();
                break;
        }
    }
    
    private handleIdleState() {
        const player = this.detectPlayer();
        if (player) {
            this.changeState(EnemyState.CHASE);
        }
    }
    
    private handleChaseState() {
        const player = Horizon.Player.getLocalPlayer();
        if (!player) {
            this.changeState(EnemyState.IDLE);
            return;
        }
        
        const distance = player.getPosition().distance(this.entity.getPosition());
        if (distance <= this.attackRange) {
            this.changeState(EnemyState.ATTACK);
        } else {
            this.moveTowardPlayer();
        }
    }
}
```

### Pathfinding and Navigation

**Basic Pathfinding:**
- Direct line movement toward target
- Obstacle avoidance
- Group movement coordination

**Advanced Pathfinding:**
- NavMesh integration
- Dynamic obstacle avoidance
- Formation movement

![Pathfinding Visualization - Screenshot showing enemy pathfinding with NavMesh and obstacle avoidance]

### Group AI Behaviors

**Formation Movement:**
- Enemies move in coordinated groups
- Maintain relative positions
- Create tactical advantages

**Swarm Behavior:**
- Multiple enemies coordinate attacks
- Surround and overwhelm players
- Dynamic group dynamics

**Support Behaviors:**
- Healers support other enemies
- Ranged enemies provide cover fire
- Tanks draw player attention

![Group AI Examples - Screenshot showing different group behaviors and formations]

## Performance Optimization

Enemy wave systems can be performance-intensive. Proper optimization ensures smooth gameplay.

### Enemy Pooling

**Object Pooling Benefits:**
- Reduces garbage collection
- Faster enemy spawning
- Better memory management

```typescript
class EnemyPool {
    private pool: Horizon.Entity[] = [];
    private maxPoolSize: number = 50;
    
    public getEnemy(): Horizon.Entity | null {
        if (this.pool.length > 0) {
            return this.pool.pop()!;
        }
        return this.createNewEnemy();
    }
    
    public returnEnemy(enemy: Horizon.Entity) {
        if (this.pool.length < this.maxPoolSize) {
            this.resetEnemy(enemy);
            this.pool.push(enemy);
        }
    }
}
```

### LOD for Enemies

**Level of Detail Implementation:**
- **High Detail**: Close enemies with full AI
- **Medium Detail**: Mid-range enemies with simplified AI
- **Low Detail**: Distant enemies with minimal processing

### Culling and Visibility

**Optimization Techniques:**
- Disable AI for off-screen enemies
- Reduce update frequency for distant enemies
- Use spatial partitioning for large areas

![Performance Optimization Panel - Screenshot showing performance metrics and optimization settings]

## Advanced Wave Scenarios

Create unique and memorable combat encounters with advanced wave mechanics.

### Environmental Hazards

**Hazard Types:**
- **Lava Pits**: Damage enemies and players
- **Moving Platforms**: Dynamic combat arenas
- **Weather Effects**: Visibility and movement modifiers
- **Trap Systems**: Automated damage sources

![Environmental Hazards - Screenshot showing lava pits, moving platforms, and weather effects in combat arena]

### Dynamic Spawn Points

**Spawn Point Types:**
- **Fixed Points**: Predictable enemy locations
- **Random Points**: Unpredictable spawning
- **Moving Points**: Spawn locations that change
- **Player-Based**: Spawn relative to player position

### Wave Modifiers

**Modifier Types:**
- **Speed Boost**: Enemies move faster
- **Health Boost**: Enemies have more health
- **Damage Boost**: Enemies deal more damage
- **Elemental Effects**: Fire, ice, lightning modifiers

![Wave Modifiers Panel - Screenshot showing different modifier options and their effects]

## Troubleshooting and Debugging

Common issues and solutions for enemy wave systems.

### Enemy Not Spawning

**Possible Causes:**
- Spawn manager not initialized
- Invalid spawn point locations
- Performance limits exceeded

**Solutions:**
1. Check spawn manager initialization
2. Verify spawn point validity
3. Monitor performance metrics

### AI Not Working

**Possible Causes:**
- AI script not attached
- State machine errors
- Performance issues

**Solutions:**
1. Verify script attachment
2. Debug state transitions
3. Optimize AI update frequency

### Performance Issues

**Possible Causes:**
- Too many active enemies
- Inefficient AI loops
- Memory leaks

**Solutions:**
1. Implement enemy pooling
2. Optimize AI update cycles
3. Monitor memory usage

![Debugging Tools - Screenshot showing debugging panel with performance metrics and error logs]

## Best Practices Summary

### Do's ✅
- Start with simple enemy types and behaviors
- Implement proper difficulty scaling
- Use object pooling for performance
- Test with multiple players
- Provide clear player feedback
- Balance challenge with accessibility

### Don'ts ❌
- Overcomplicate AI behaviors initially
- Ignore performance optimization
- Create unfair difficulty spikes
- Forget mobile device limitations
- Neglect player feedback systems
- Use too many enemies simultaneously

## Resources and References

### Official Documentation
- **[Horizon Worlds Scripting API](https://developers.meta.com/horizon-worlds/learn/documentation/typescript)**
- **[Performance Best Practices](https://developers.meta.com/horizon-worlds/learn/documentation/performance-best-practices-and-tooling)**
- **[NavMesh Documentation](https://developers.meta.com/horizon-worlds/learn/documentation/desktop-editor/navmesh)**

### Additional Learning
- **Game AI Programming**: Study AI behavior patterns
- **Combat Design**: Learn from successful games
- **Performance Optimization**: Understand real-time systems

### Community Resources
- **Horizon Worlds Creator Community**: Share AI techniques
- **Discord Servers**: Join AI programming discussions
- **GitHub Repositories**: Find open-source AI examples

---

**Ready to Build Epic Combat Encounters?**

Start with simple enemy types and gradually add complexity. Remember, good AI doesn't need to be complex - it needs to be fun and fair. Test your wave systems thoroughly, gather player feedback, and iterate on your designs.

**Next Steps:**
1. Create a basic enemy spawn system
2. Implement simple AI behaviors
3. Add wave progression mechanics
4. Optimize for performance
5. Test with real players!

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
