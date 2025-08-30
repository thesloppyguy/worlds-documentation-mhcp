# Enemy Wave Fundamentals: Understanding Wave Systems in Horizon Worlds

**Master the foundational principles of enemy wave systems to create engaging, dynamic combat experiences.** This guide covers the essential concepts of wave mechanics, enemy types, and basic spawner implementation that form the foundation for all combat systems in Meta Horizon Worlds.

**Creator Skill Level**
Beginner to Intermediate

**Recommended Background Knowledge**
Basic understanding of the Horizon Worlds Desktop Editor and TypeScript scripting.

**Estimated Time to Complete**
45-60 minutes for fundamentals, 30 minutes for basic setup

## Table of Contents

1. [Understanding Wave Systems](#understanding-wave-systems)
2. [Enemy Types and Characteristics](#enemy-types-and-characteristics)
3. [Basic Wave Spawner](#basic-wave-spawner)
4. [Wave Progression Mechanics](#wave-progression-mechanics)
5. [Performance Considerations](#performance-considerations)

## Understanding Wave Systems

Enemy wave systems are the backbone of many action and survival games. They create dynamic, escalating challenges that keep players engaged and provide a sense of progression and achievement.

### What is a Wave System?

**Wave System Definition:**
- A sequence of enemy groups that spawn at predetermined intervals
- Each wave typically increases in difficulty and complexity
- Provides structured combat encounters with clear progression
- Can be endless or finite with specific win conditions

### Wave System Components

**Core Elements:**
- **Spawn Points**: Locations where enemies appear
- **Enemy Types**: Different enemy behaviors and characteristics
- **Wave Timing**: When and how frequently waves spawn
- **Difficulty Scaling**: How challenge increases over time
- **Wave Types**: Standard, boss, special event waves

### Wave System Benefits

**Player Engagement:**
- **Clear Progression**: Players see their advancement through waves
- **Escalating Challenge**: Keeps gameplay interesting and challenging
- **Achievement Goals**: Completing waves provides satisfaction
- **Replayability**: Different strategies for different wave types

**Game Design:**
- **Controlled Pacing**: Manage player experience and difficulty
- **Resource Management**: Players must balance resources across waves
- **Skill Testing**: Different waves test different player abilities
- **Story Integration**: Waves can advance narrative elements

## Enemy Types and Characteristics

Understanding different enemy types helps you create varied and interesting combat encounters.

### Melee Enemies

**Characteristics:**
- **Health**: 50-150 HP (moderate)
- **Speed**: 3-5 m/s (medium to fast)
- **Attack Range**: 1-2 meters (close combat)
- **Attack Damage**: 10-25 damage per hit
- **Behavior**: Aggressive, direct approach

**Best For**: Close-quarters combat, pressure tactics, resource management

**Basic Melee Enemy Implementation:**
```typescript
class MeleeEnemy extends Horizon.Component {
    private health: number = 100;
    private speed: number = 4.0;
    private attackRange: number = 1.5;
    private attackDamage: number = 15;
    private attackCooldown: number = 1000; // 1 second
    private lastAttackTime: number = 0;
    private target: Horizon.Player | null = null;
    private state: EnemyState = EnemyState.Idle;
    
    override start() {
        this.setupEnemy();
    }
    
    private setupEnemy() {
        // Set up enemy properties
        this.setHealth(this.health);
        this.setSpeed(this.speed);
        this.setAttackRange(this.attackRange);
        
        // Start AI behavior
        this.startAI();
    }
    
    private startAI() {
        setInterval(() => {
            this.updateAI();
        }, 100); // Update every 100ms
    }
    
    private updateAI() {
        switch (this.state) {
            case EnemyState.Idle:
                this.findTarget();
                break;
            case EnemyState.Chasing:
                this.chaseTarget();
                break;
            case EnemyState.Attacking:
                this.attackTarget();
                break;
            case EnemyState.Retreating:
                this.retreat();
                break;
        }
    }
    
    private findTarget() {
        const players = Horizon.Player.getAllPlayers();
        let closestPlayer: Horizon.Player | null = null;
        let closestDistance = Infinity;
        
        players.forEach(player => {
            const distance = this.getPosition().distance(player.getPosition());
            if (distance < closestDistance && distance < 20) { // 20m detection range
                closestDistance = distance;
                closestPlayer = player;
            }
        });
        
        if (closestPlayer) {
            this.target = closestPlayer;
            this.state = EnemyState.Chasing;
        }
    }
    
    private chaseTarget() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const targetPos = this.target.getPosition();
        const myPos = this.getPosition();
        const distance = myPos.distance(targetPos);
        
        if (distance <= this.attackRange) {
            this.state = EnemyState.Attacking;
        } else {
            // Move towards target
            const direction = targetPos.subtract(myPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            this.setPosition(newPos);
            
            // Face the target
            const rotation = Math.atan2(direction.x, direction.z) * 180 / Math.PI;
            this.setRotation(0, rotation, 0);
        }
    }
    
    private attackTarget() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const currentTime = Date.now();
        if (currentTime - this.lastAttackTime >= this.attackCooldown) {
            // Perform attack
            this.performAttack();
            this.lastAttackTime = currentTime;
        }
        
        // Check if target is still in range
        const distance = this.getPosition().distance(this.target.getPosition());
        if (distance > this.attackRange) {
            this.state = EnemyState.Chasing;
        }
    }
    
    private performAttack() {
        // Deal damage to target
        if (this.target) {
            this.target.takeDamage(this.attackDamage);
            
            // Play attack animation/sound
            this.playAttackEffect();
        }
    }
    
    private playAttackEffect() {
        // Add visual/audio effects for attack
        console.log("Melee enemy attacks!");
    }
    
    private retreat() {
        // Move away from target when health is low
        if (this.target) {
            const targetPos = this.target.getPosition();
            const myPos = this.getPosition();
            const direction = myPos.subtract(targetPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            this.setPosition(newPos);
        }
    }
    
    public takeDamage(damage: number) {
        this.health -= damage;
        
        if (this.health <= 0) {
            this.die();
        } else if (this.health < 30) {
            this.state = EnemyState.Retreating;
        }
    }
    
    private die() {
        // Play death effect
        console.log("Melee enemy dies!");
        
        // Remove from scene
        this.destroy();
    }
}

enum EnemyState {
    Idle,
    Chasing,
    Attacking,
    Retreating
}
```

### Ranged Enemies

**Characteristics:**
- **Health**: 30-80 HP (lower than melee)
- **Speed**: 2-4 m/s (slower than melee)
- **Attack Range**: 5-15 meters (long range)
- **Attack Damage**: 8-20 damage per shot
- **Behavior**: Keep distance, use cover, coordinated attacks

**Best For**: Tactical combat, area denial, player positioning challenges

**Basic Ranged Enemy Implementation:**
```typescript
class RangedEnemy extends Horizon.Component {
    private health: number = 60;
    private speed: number = 3.0;
    private attackRange: number = 10.0;
    private minAttackRange: number = 5.0; // Don't get too close
    private attackDamage: number = 12;
    private attackCooldown: number = 1500; // 1.5 seconds
    private lastAttackTime: number = 0;
    private target: Horizon.Player | null = null;
    private state: EnemyState = EnemyState.Idle;
    
    override start() {
        this.setupRangedEnemy();
    }
    
    private setupRangedEnemy() {
        this.setHealth(this.health);
        this.setSpeed(this.speed);
        this.setAttackRange(this.attackRange);
        this.startAI();
    }
    
    private updateAI() {
        switch (this.state) {
            case EnemyState.Idle:
                this.findTarget();
                break;
            case EnemyState.Chasing:
                this.maintainDistance();
                break;
            case EnemyState.Attacking:
                this.rangedAttack();
                break;
            case EnemyState.Retreating:
                this.retreat();
                break;
        }
    }
    
    private maintainDistance() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const targetPos = this.target.getPosition();
        const myPos = this.getPosition();
        const distance = myPos.distance(targetPos);
        
        if (distance <= this.minAttackRange) {
            // Too close, back away
            const direction = myPos.subtract(targetPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            this.setPosition(newPos);
        } else if (distance <= this.attackRange) {
            this.state = EnemyState.Attacking;
        } else {
            // Move closer to target
            const direction = targetPos.subtract(myPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            this.setPosition(newPos);
        }
        
        // Face the target
        const direction = targetPos.subtract(myPos).normalize();
        const rotation = Math.atan2(direction.x, direction.z) * 180 / Math.PI;
        this.setRotation(0, rotation, 0);
    }
    
    private rangedAttack() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const currentTime = Date.now();
        if (currentTime - this.lastAttackTime >= this.attackCooldown) {
            this.performRangedAttack();
            this.lastAttackTime = currentTime;
        }
        
        // Check if target is still in range
        const distance = this.getPosition().distance(this.target.getPosition());
        if (distance > this.attackRange || distance < this.minAttackRange) {
            this.state = EnemyState.Chasing;
        }
    }
    
    private performRangedAttack() {
        if (this.target) {
            // Create projectile
            this.createProjectile();
            
            // Play attack effect
            this.playRangedAttackEffect();
        }
    }
    
    private createProjectile() {
        // Create and fire projectile towards target
        const projectile = this.spawnProjectile();
        if (projectile && this.target) {
            const targetPos = this.target.getPosition();
            const myPos = this.getPosition();
            const direction = targetPos.subtract(myPos).normalize();
            
            projectile.setPosition(myPos);
            projectile.setVelocity(direction.multiply(15)); // 15 m/s projectile speed
        }
    }
    
    private spawnProjectile(): Horizon.Entity | null {
        // Spawn projectile entity
        return this.createEntity("Projectile");
    }
    
    private playRangedAttackEffect() {
        console.log("Ranged enemy fires!");
    }
}
```

### Flying Enemies

**Characteristics:**
- **Health**: 40-100 HP (moderate)
- **Speed**: 4-7 m/s (fast, 3D movement)
- **Attack Range**: 2-8 meters (variable)
- **Attack Damage**: 8-18 damage per attack
- **Behavior**: Aerial attacks, hit-and-run tactics, group coordination

**Best For**: Vertical combat, area control, dynamic movement challenges

**Basic Flying Enemy Implementation:**
```typescript
class FlyingEnemy extends Horizon.Component {
    private health: number = 70;
    private speed: number = 5.0;
    private attackRange: number = 3.0;
    private attackDamage: number = 10;
    private attackCooldown: number = 1200; // 1.2 seconds
    private lastAttackTime: number = 0;
    private target: Horizon.Player | null = null;
    private state: EnemyState = EnemyState.Idle;
    private flightHeight: number = 5.0; // Fly at 5m height
    
    override start() {
        this.setupFlyingEnemy();
    }
    
    private setupFlyingEnemy() {
        this.setHealth(this.health);
        this.setSpeed(this.speed);
        this.setAttackRange(this.attackRange);
        
        // Set initial flight height
        const pos = this.getPosition();
        this.setPosition(pos.x, this.flightHeight, pos.z);
        
        this.startAI();
    }
    
    private updateAI() {
        switch (this.state) {
            case EnemyState.Idle:
                this.findTarget();
                break;
            case EnemyState.Chasing:
                this.flyToTarget();
                break;
            case EnemyState.Attacking:
                this.aerialAttack();
                break;
            case EnemyState.Retreating:
                this.aerialRetreat();
                break;
        }
    }
    
    private flyToTarget() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const targetPos = this.target.getPosition();
        const myPos = this.getPosition();
        const distance = myPos.distance(targetPos);
        
        if (distance <= this.attackRange) {
            this.state = EnemyState.Attacking;
        } else {
            // Fly towards target
            const direction = targetPos.subtract(myPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            
            // Maintain flight height
            newPos.y = this.flightHeight;
            this.setPosition(newPos);
            
            // Face the target
            const rotation = Math.atan2(direction.x, direction.z) * 180 / Math.PI;
            this.setRotation(0, rotation, 0);
        }
    }
    
    private aerialAttack() {
        if (!this.target) {
            this.state = EnemyState.Idle;
            return;
        }
        
        const currentTime = Date.now();
        if (currentTime - this.lastAttackTime >= this.attackCooldown) {
            this.performAerialAttack();
            this.lastAttackTime = currentTime;
        }
        
        // Check if target is still in range
        const distance = this.getPosition().distance(this.target.getPosition());
        if (distance > this.attackRange) {
            this.state = EnemyState.Chasing;
        }
    }
    
    private performAerialAttack() {
        if (this.target) {
            // Dive attack or ranged attack from air
            this.diveAttack();
        }
    }
    
    private diveAttack() {
        // Perform a dive attack towards the target
        console.log("Flying enemy performs dive attack!");
        
        // Deal damage
        if (this.target) {
            this.target.takeDamage(this.attackDamage);
        }
    }
    
    private aerialRetreat() {
        // Fly away from target
        if (this.target) {
            const targetPos = this.target.getPosition();
            const myPos = this.getPosition();
            const direction = myPos.subtract(targetPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            
            // Maintain flight height
            newPos.y = this.flightHeight;
            this.setPosition(newPos);
        }
    }
}
```

### Boss Enemies

**Characteristics:**
- **Health**: 200-1000+ HP (very high)
- **Speed**: 2-6 m/s (variable, often slower)
- **Attack Range**: 2-15 meters (multiple attack types)
- **Attack Damage**: 25-100+ damage per attack
- **Behavior**: Complex AI, multiple phases, special abilities

**Best For**: Climactic encounters, story progression, major challenges

**Basic Boss Enemy Implementation:**
```typescript
class BossEnemy extends Horizon.Component {
    private health: number = 500;
    private maxHealth: number = 500;
    private speed: number = 3.0;
    private attackDamage: number = 40;
    private target: Horizon.Player | null = null;
    private state: BossState = BossState.Idle;
    private phase: number = 1;
    private lastSpecialAbility: number = 0;
    private specialAbilityCooldown: number = 10000; // 10 seconds
    
    override start() {
        this.setupBoss();
    }
    
    private setupBoss() {
        this.setHealth(this.health);
        this.setSpeed(this.speed);
        this.startBossAI();
    }
    
    private startBossAI() {
        setInterval(() => {
            this.updateBossAI();
        }, 100);
    }
    
    private updateBossAI() {
        // Check for phase transitions
        this.checkPhaseTransition();
        
        switch (this.state) {
            case BossState.Idle:
                this.findTarget();
                break;
            case BossState.Chasing:
                this.bossChase();
                break;
            case BossState.Attacking:
                this.bossAttack();
                break;
            case BossState.SpecialAbility:
                this.useSpecialAbility();
                break;
            case BossState.Retreating:
                this.bossRetreat();
                break;
        }
    }
    
    private checkPhaseTransition() {
        const healthPercentage = this.health / this.maxHealth;
        
        if (healthPercentage <= 0.25 && this.phase === 2) {
            this.enterPhase3();
        } else if (healthPercentage <= 0.5 && this.phase === 1) {
            this.enterPhase2();
        }
    }
    
    private enterPhase2() {
        this.phase = 2;
        this.speed *= 1.5;
        this.attackDamage *= 1.3;
        console.log("Boss enters Phase 2 - Increased speed and damage!");
    }
    
    private enterPhase3() {
        this.phase = 3;
        this.speed *= 1.2;
        this.attackDamage *= 1.5;
        console.log("Boss enters Phase 3 - Final form!");
    }
    
    private bossChase() {
        if (!this.target) {
            this.state = BossState.Idle;
            return;
        }
        
        const targetPos = this.target.getPosition();
        const myPos = this.getPosition();
        const distance = myPos.distance(targetPos);
        
        if (distance <= 3.0) {
            this.state = BossState.Attacking;
        } else {
            // Move towards target
            const direction = targetPos.subtract(myPos).normalize();
            const newPos = myPos.add(direction.multiply(this.speed * 0.1));
            this.setPosition(newPos);
            
            // Face the target
            const rotation = Math.atan2(direction.x, direction.z) * 180 / Math.PI;
            this.setRotation(0, rotation, 0);
        }
        
        // Check for special ability
        this.checkSpecialAbility();
    }
    
    private checkSpecialAbility() {
        const currentTime = Date.now();
        if (currentTime - this.lastSpecialAbility >= this.specialAbilityCooldown) {
            this.state = BossState.SpecialAbility;
        }
    }
    
    private bossAttack() {
        if (!this.target) {
            this.state = BossState.Idle;
            return;
        }
        
        // Perform powerful attack
        this.performBossAttack();
        
        // Return to chasing
        this.state = BossState.Chasing;
    }
    
    private performBossAttack() {
        if (this.target) {
            this.target.takeDamage(this.attackDamage);
            console.log(`Boss deals ${this.attackDamage} damage!`);
        }
    }
    
    private useSpecialAbility() {
        const currentTime = Date.now();
        this.lastSpecialAbility = currentTime;
        
        switch (this.phase) {
            case 1:
                this.phase1SpecialAbility();
                break;
            case 2:
                this.phase2SpecialAbility();
                break;
            case 3:
                this.phase3SpecialAbility();
                break;
        }
        
        this.state = BossState.Chasing;
    }
    
    private phase1SpecialAbility() {
        console.log("Boss uses Phase 1 special ability!");
        // Implement phase 1 special ability
    }
    
    private phase2SpecialAbility() {
        console.log("Boss uses Phase 2 special ability!");
        // Implement phase 2 special ability
    }
    
    private phase3SpecialAbility() {
        console.log("Boss uses Phase 3 special ability!");
        // Implement phase 3 special ability
    }
    
    public takeDamage(damage: number) {
        this.health -= damage;
        
        if (this.health <= 0) {
            this.bossDeath();
        }
    }
    
    private bossDeath() {
        console.log("Boss defeated!");
        // Trigger victory conditions, rewards, etc.
        this.destroy();
    }
}

enum BossState {
    Idle,
    Chasing,
    Attacking,
    SpecialAbility,
    Retreating
}
```

## Basic Wave Spawner

Create a simple wave spawner to get started with enemy wave systems.

### Simple Wave Spawner

**Basic Implementation:**
```typescript
class BasicWaveSpawner extends Horizon.Component {
    private currentWave: number = 1;
    private enemiesPerWave: number = 5;
    private waveDelay: number = 3000; // 3 seconds between waves
    private spawnPoints: Horizon.Entity[] = [];
    private activeEnemies: Horizon.Entity[] = [];
    private isSpawning: boolean = false;
    
    override start() {
        this.setupSpawner();
    }
    
    private setupSpawner() {
        // Get spawn points
        this.spawnPoints = this.getSpawnPoints();
        
        // Start first wave
        setTimeout(() => {
            this.startWave();
        }, 2000); // 2 second delay before first wave
    }
    
    private getSpawnPoints(): Horizon.Entity[] {
        // Get all spawn point entities in the scene
        return this.getEntitiesWithTag("SpawnPoint");
    }
    
    private startWave() {
        if (this.isSpawning) return;
        
        this.isSpawning = true;
        console.log(`Starting Wave ${this.currentWave}`);
        
        // Spawn enemies for this wave
        this.spawnWaveEnemies();
        
        // Schedule next wave
        setTimeout(() => {
            this.currentWave++;
            this.isSpawning = false;
            this.startWave();
        }, this.waveDelay);
    }
    
    private spawnWaveEnemies() {
        const enemiesToSpawn = this.calculateEnemiesForWave();
        
        for (let i = 0; i < enemiesToSpawn; i++) {
            setTimeout(() => {
                this.spawnEnemy();
            }, i * 500); // Spawn enemies 0.5 seconds apart
        }
    }
    
    private calculateEnemiesForWave(): number {
        // Increase enemies per wave
        return this.enemiesPerWave + (this.currentWave - 1) * 2;
    }
    
    private spawnEnemy() {
        if (this.spawnPoints.length === 0) return;
        
        // Select random spawn point
        const spawnPoint = this.spawnPoints[Math.floor(Math.random() * this.spawnPoints.length)];
        const spawnPos = spawnPoint.getPosition();
        
        // Determine enemy type based on wave
        const enemyType = this.selectEnemyType();
        
        // Spawn enemy
        const enemy = this.spawnEnemyOfType(enemyType, spawnPos);
        
        if (enemy) {
            this.activeEnemies.push(enemy);
            
            // Listen for enemy death
            enemy.onDestroy.add(() => {
                this.removeEnemyFromList(enemy);
            });
        }
    }
    
    private selectEnemyType(): string {
        if (this.currentWave <= 3) {
            return "MeleeEnemy";
        } else if (this.currentWave <= 6) {
            return Math.random() < 0.7 ? "MeleeEnemy" : "RangedEnemy";
        } else if (this.currentWave <= 10) {
            const rand = Math.random();
            if (rand < 0.5) return "MeleeEnemy";
            else if (rand < 0.8) return "RangedEnemy";
            else return "FlyingEnemy";
        } else {
            const rand = Math.random();
            if (rand < 0.4) return "MeleeEnemy";
            else if (rand < 0.7) return "RangedEnemy";
            else if (rand < 0.9) return "FlyingEnemy";
            else return "BossEnemy";
        }
    }
    
    private spawnEnemyOfType(enemyType: string, position: Horizon.Vector3): Horizon.Entity | null {
        switch (enemyType) {
            case "MeleeEnemy":
                return this.createMeleeEnemy(position);
            case "RangedEnemy":
                return this.createRangedEnemy(position);
            case "FlyingEnemy":
                return this.createFlyingEnemy(position);
            case "BossEnemy":
                return this.createBossEnemy(position);
            default:
                return this.createMeleeEnemy(position);
        }
    }
    
    private createMeleeEnemy(position: Horizon.Vector3): Horizon.Entity {
        const enemy = this.createEntity("MeleeEnemy");
        enemy.setPosition(position);
        return enemy;
    }
    
    private createRangedEnemy(position: Horizon.Vector3): Horizon.Entity {
        const enemy = this.createEntity("RangedEnemy");
        enemy.setPosition(position);
        return enemy;
    }
    
    private createFlyingEnemy(position: Horizon.Vector3): Horizon.Entity {
        const enemy = this.createEntity("FlyingEnemy");
        enemy.setPosition(position.x, position.y + 5, position.z); // Spawn in air
        return enemy;
    }
    
    private createBossEnemy(position: Horizon.Vector3): Horizon.Entity {
        const enemy = this.createEntity("BossEnemy");
        enemy.setPosition(position);
        return enemy;
    }
    
    private removeEnemyFromList(enemy: Horizon.Entity) {
        const index = this.activeEnemies.indexOf(enemy);
        if (index > -1) {
            this.activeEnemies.splice(index, 1);
        }
    }
    
    public getCurrentWave(): number {
        return this.currentWave;
    }
    
    public getActiveEnemyCount(): number {
        return this.activeEnemies.length;
    }
}
```

## Wave Progression Mechanics

Understanding how waves progress and scale in difficulty.

### Wave Difficulty Scaling

**Progressive Scaling Factors:**
- **Enemy Count**: More enemies per wave
- **Enemy Types**: Introduction of new enemy types
- **Enemy Health**: Increased health pools
- **Enemy Damage**: Higher damage output
- **Spawn Rate**: Faster enemy spawning
- **Special Events**: Boss waves, special challenges

### Wave Types

**Standard Waves:**
- Regular enemy spawns
- Gradual difficulty increase
- Resource management focus

**Boss Waves:**
- Special boss enemy spawns
- Higher rewards
- Story progression moments

**Event Waves:**
- Special conditions or modifiers
- Unique challenges
- Limited-time events

### Wave Timing

**Spawn Timing:**
- **Immediate**: All enemies spawn at once
- **Staggered**: Enemies spawn over time
- **Conditional**: Enemies spawn based on conditions

**Wave Intervals:**
- **Fixed**: Consistent time between waves
- **Dynamic**: Time varies based on performance
- **Conditional**: Next wave starts when current wave is cleared

## Performance Considerations

Basic performance optimization for wave systems.

### Enemy Limits

**Recommended Limits:**
- **Active Enemies**: 20-50 enemies maximum
- **Spawn Rate**: 1-2 enemies per second
- **Wave Size**: 5-15 enemies per wave

### Optimization Techniques

**Basic Optimizations:**
- Limit active enemy count
- Use object pooling for enemies
- Implement enemy culling for distant enemies
- Optimize AI update frequency

**Memory Management:**
- Clean up destroyed enemies
- Reuse enemy objects when possible
- Limit particle effects and animations

### Performance Monitoring

**Key Metrics:**
- Active enemy count
- Frame rate during waves
- Memory usage
- AI update frequency

---

**Ready to Build Your First Wave System?**

Start with the basic wave spawner and gradually add complexity. Remember to test your waves thoroughly and balance difficulty for your target audience.

**Practice Exercises:**
1. Create a simple 5-wave system with melee enemies
2. Add ranged enemies to wave 3+
3. Implement a boss wave every 5 waves
4. Add difficulty scaling based on player performance

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
