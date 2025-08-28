# Advanced Enemy AI Behaviors: Complex Wave Progression and AI Systems

**Master advanced AI behaviors, wave progression mechanics, and complex enemy interactions.** This guide covers sophisticated AI systems, dynamic wave management, and advanced combat scenarios that create engaging and challenging gameplay experiences.

**Creator Skill Level**
Intermediate to Advanced

**Recommended Background Knowledge**
Completion of Enemy Wave Fundamentals and basic TypeScript scripting experience.

**Estimated Time to Complete**
60-90 minutes for advanced concepts, 45 minutes for implementation

## Table of Contents

1. [Advanced AI State Machines](#advanced-ai-state-machines)
2. [Group AI Behaviors](#group-ai-behaviors)
3. [Dynamic Wave Progression](#dynamic-wave-progression)
4. [Environmental AI](#environmental-ai)
5. [Performance Optimization](#performance-optimization)

## Advanced AI State Machines

Create sophisticated AI behaviors using advanced state machines and decision trees.

### Complex Enemy AI

**Advanced State Machine Implementation:**
```typescript
class AdvancedEnemyAI extends Horizon.Component {
    private state: AIState = AIState.Idle;
    private subState: SubState = SubState.None;
    private target: Horizon.Player | null = null;
    private lastStateChange: number = 0;
    private stateTimeout: number = 5000; // 5 seconds max in any state
    private health: number = 100;
    private maxHealth: number = 100;
    private stamina: number = 100;
    private maxStamina: number = 100;
    
    // AI personality traits
    private aggressiveness: number = 0.7; // 0-1 scale
    private caution: number = 0.3;
    private intelligence: number = 0.8;
    
    override start() {
        this.initializeAI();
    }
    
    private initializeAI() {
        this.startAIUpdate();
        this.startStaminaRegeneration();
    }
    
    private startAIUpdate() {
        setInterval(() => {
            this.updateAI();
        }, 100);
    }
    
    private updateAI() {
        // Check for state timeout
        this.checkStateTimeout();
        
        // Update based on current state
        switch (this.state) {
            case AIState.Idle:
                this.handleIdleState();
                break;
            case AIState.Investigating:
                this.handleInvestigatingState();
                break;
            case AIState.Hunting:
                this.handleHuntingState();
                break;
            case AIState.Combat:
                this.handleCombatState();
                break;
            case AIState.Retreating:
                this.handleRetreatingState();
                break;
            case AIState.Recovering:
                this.handleRecoveringState();
                break;
        }
    }
    
    private handleIdleState() {
        // Patrol or search for targets
        if (this.detectThreats()) {
            this.transitionTo(AIState.Investigating);
        } else {
            this.patrol();
        }
    }
    
    private handleInvestigatingState() {
        // Investigate suspicious activity
        if (this.confirmThreat()) {
            this.transitionTo(AIState.Hunting);
        } else if (this.investigationTimeout()) {
            this.transitionTo(AIState.Idle);
        } else {
            this.investigate();
        }
    }
    
    private handleHuntingState() {
        // Actively pursue target
        if (this.target && this.canEngage()) {
            this.transitionTo(AIState.Combat);
        } else if (this.lostTarget()) {
            this.transitionTo(AIState.Investigating);
        } else {
            this.hunt();
        }
    }
    
    private handleCombatState() {
        // Engage in combat
        if (this.shouldRetreat()) {
            this.transitionTo(AIState.Retreating);
        } else if (this.target && this.isInCombatRange()) {
            this.combat();
        } else {
            this.transitionTo(AIState.Hunting);
        }
    }
    
    private handleRetreatingState() {
        // Retreat from combat
        if (this.isSafe()) {
            this.transitionTo(AIState.Recovering);
        } else {
            this.retreat();
        }
    }
    
    private handleRecoveringState() {
        // Recover health and stamina
        if (this.isRecovered()) {
            this.transitionTo(AIState.Idle);
        } else {
            this.recover();
        }
    }
    
    private transitionTo(newState: AIState) {
        this.state = newState;
        this.lastStateChange = Date.now();
        this.onStateTransition(newState);
    }
    
    private onStateTransition(newState: AIState) {
        console.log(`AI transitioning to: ${AIState[newState]}`);
        
        switch (newState) {
            case AIState.Combat:
                this.enterCombatMode();
                break;
            case AIState.Retreating:
                this.enterRetreatMode();
                break;
            case AIState.Recovering:
                this.enterRecoveryMode();
                break;
        }
    }
    
    private detectThreats(): boolean {
        const players = Horizon.Player.getAllPlayers();
        for (const player of players) {
            const distance = this.getPosition().distance(player.getPosition());
            if (distance < 15) { // 15m detection range
                this.target = player;
                return true;
            }
        }
        return false;
    }
    
    private confirmThreat(): boolean {
        if (!this.target) return false;
        
        const distance = this.getPosition().distance(this.target.getPosition());
        return distance < 10; // 10m confirmation range
    }
    
    private canEngage(): boolean {
        if (!this.target) return false;
        
        const distance = this.getPosition().distance(this.target.getPosition());
        return distance < 5 && this.stamina > 20;
    }
    
    private shouldRetreat(): boolean {
        const healthPercentage = this.health / this.maxHealth;
        const staminaPercentage = this.stamina / this.maxStamina;
        
        return healthPercentage < 0.3 || staminaPercentage < 0.1;
    }
    
    private isSafe(): boolean {
        if (!this.target) return true;
        
        const distance = this.getPosition().distance(this.target.getPosition());
        return distance > 20;
    }
    
    private isRecovered(): boolean {
        return this.health > this.maxHealth * 0.8 && this.stamina > this.maxStamina * 0.9;
    }
    
    private startStaminaRegeneration() {
        setInterval(() => {
            if (this.stamina < this.maxStamina) {
                this.stamina = Math.min(this.maxStamina, this.stamina + 1);
            }
        }, 1000); // Regenerate 1 stamina per second
    }
}

enum AIState {
    Idle,
    Investigating,
    Hunting,
    Combat,
    Retreating,
    Recovering
}

enum SubState {
    None,
    Patrolling,
    Searching,
    Attacking,
    Defending,
    Fleeing
}
```

## Group AI Behaviors

Implement coordinated group behaviors for multiple enemies.

### Formation AI

**Formation-Based Movement:**
```typescript
class FormationAI extends Horizon.Component {
    private formationType: FormationType = FormationType.Line;
    private formationRadius: number = 3.0;
    private enemies: Horizon.Entity[] = [];
    private leader: Horizon.Entity | null = null;
    private target: Horizon.Player | null = null;
    
    override start() {
        this.setupFormation();
    }
    
    private setupFormation() {
        this.enemies = this.getNearbyEnemies();
        this.leader = this.selectLeader();
        this.startFormationAI();
    }
    
    private getNearbyEnemies(): Horizon.Entity[] {
        const allEnemies = this.getEntitiesWithTag("Enemy");
        const nearbyEnemies: Horizon.Entity[] = [];
        const myPos = this.getPosition();
        
        allEnemies.forEach(enemy => {
            const distance = myPos.distance(enemy.getPosition());
            if (distance < 10 && enemy !== this) {
                nearbyEnemies.push(enemy);
            }
        });
        
        return nearbyEnemies;
    }
    
    private selectLeader(): Horizon.Entity {
        // Select the strongest or most experienced enemy as leader
        return this.enemies[0] || this;
    }
    
    private startFormationAI() {
        setInterval(() => {
            this.updateFormation();
        }, 200);
    }
    
    private updateFormation() {
        if (!this.leader) return;
        
        const leaderPos = this.leader.getPosition();
        const targetPos = this.target?.getPosition() || leaderPos;
        
        // Calculate formation positions
        const formationPositions = this.calculateFormationPositions(leaderPos, targetPos);
        
        // Move enemies to formation positions
        this.enemies.forEach((enemy, index) => {
            if (formationPositions[index]) {
                this.moveToFormationPosition(enemy, formationPositions[index]);
            }
        });
    }
    
    private calculateFormationPositions(leaderPos: Horizon.Vector3, targetPos: Horizon.Vector3): Horizon.Vector3[] {
        const positions: Horizon.Vector3[] = [];
        const direction = targetPos.subtract(leaderPos).normalize();
        const perpendicular = new Horizon.Vector3(-direction.z, 0, direction.x);
        
        switch (this.formationType) {
            case FormationType.Line:
                this.calculateLineFormation(leaderPos, direction, positions);
                break;
            case FormationType.Circle:
                this.calculateCircleFormation(leaderPos, positions);
                break;
            case FormationType.V:
                this.calculateVFormation(leaderPos, direction, positions);
                break;
        }
        
        return positions;
    }
    
    private calculateLineFormation(leaderPos: Horizon.Vector3, direction: Horizon.Vector3, positions: Horizon.Vector3[]) {
        const perpendicular = new Horizon.Vector3(-direction.z, 0, direction.x);
        
        for (let i = 0; i < this.enemies.length; i++) {
            const offset = (i - this.enemies.length / 2) * this.formationRadius;
            const position = leaderPos.add(perpendicular.multiply(offset));
            positions.push(position);
        }
    }
    
    private calculateCircleFormation(leaderPos: Horizon.Vector3, positions: Horizon.Vector3[]) {
        for (let i = 0; i < this.enemies.length; i++) {
            const angle = (i / this.enemies.length) * 2 * Math.PI;
            const x = Math.cos(angle) * this.formationRadius;
            const z = Math.sin(angle) * this.formationRadius;
            const position = leaderPos.add(new Horizon.Vector3(x, 0, z));
            positions.push(position);
        }
    }
    
    private calculateVFormation(leaderPos: Horizon.Vector3, direction: Horizon.Vector3, positions: Horizon.Vector3[]) {
        const perpendicular = new Horizon.Vector3(-direction.z, 0, direction.x);
        
        for (let i = 0; i < this.enemies.length; i++) {
            const side = i % 2 === 0 ? 1 : -1;
            const row = Math.floor(i / 2) + 1;
            const xOffset = side * row * this.formationRadius;
            const zOffset = -row * this.formationRadius;
            
            const position = leaderPos.add(perpendicular.multiply(xOffset)).add(direction.multiply(zOffset));
            positions.push(position);
        }
    }
    
    private moveToFormationPosition(enemy: Horizon.Entity, targetPos: Horizon.Vector3) {
        const currentPos = enemy.getPosition();
        const direction = targetPos.subtract(currentPos).normalize();
        const distance = currentPos.distance(targetPos);
        
        if (distance > 0.5) {
            const newPos = currentPos.add(direction.multiply(2.0 * 0.1)); // 2 m/s speed
            enemy.setPosition(newPos);
            
            // Face the movement direction
            const rotation = Math.atan2(direction.x, direction.z) * 180 / Math.PI;
            enemy.setRotation(0, rotation, 0);
        }
    }
}

enum FormationType {
    Line,
    Circle,
    V
}
```

### Swarm AI

**Swarm Behavior Implementation:**
```typescript
class SwarmAI extends Horizon.Component {
    private swarmMembers: Horizon.Entity[] = [];
    private target: Horizon.Player | null = null;
    private swarmRadius: number = 5.0;
    private cohesionStrength: number = 0.5;
    private separationStrength: number = 0.8;
    private alignmentStrength: number = 0.3;
    
    override start() {
        this.setupSwarm();
    }
    
    private setupSwarm() {
        this.swarmMembers = this.getSwarmMembers();
        this.startSwarmAI();
    }
    
    private getSwarmMembers(): Horizon.Entity[] {
        const allEnemies = this.getEntitiesWithTag("SwarmEnemy");
        const myPos = this.getPosition();
        const members: Horizon.Entity[] = [];
        
        allEnemies.forEach(enemy => {
            const distance = myPos.distance(enemy.getPosition());
            if (distance < 15 && enemy !== this) {
                members.push(enemy);
            }
        });
        
        return members;
    }
    
    private startSwarmAI() {
        setInterval(() => {
            this.updateSwarm();
        }, 100);
    }
    
    private updateSwarm() {
        this.findTarget();
        
        this.swarmMembers.forEach(member => {
            const velocity = this.calculateSwarmVelocity(member);
            this.applyVelocity(member, velocity);
        });
    }
    
    private calculateSwarmVelocity(member: Horizon.Entity): Horizon.Vector3 {
        const cohesion = this.calculateCohesion(member);
        const separation = this.calculateSeparation(member);
        const alignment = this.calculateAlignment(member);
        const targetSeeking = this.calculateTargetSeeking(member);
        
        // Combine all forces
        const velocity = cohesion.multiply(this.cohesionStrength)
            .add(separation.multiply(this.separationStrength))
            .add(alignment.multiply(this.alignmentStrength))
            .add(targetSeeking.multiply(0.5));
        
        return velocity.normalize().multiply(3.0); // 3 m/s speed
    }
    
    private calculateCohesion(member: Horizon.Entity): Horizon.Vector3 {
        if (this.swarmMembers.length === 0) return new Horizon.Vector3(0, 0, 0);
        
        const center = this.calculateSwarmCenter();
        const memberPos = member.getPosition();
        return center.subtract(memberPos).normalize();
    }
    
    private calculateSeparation(member: Horizon.Entity): Horizon.Vector3 {
        const memberPos = member.getPosition();
        let separation = new Horizon.Vector3(0, 0, 0);
        
        this.swarmMembers.forEach(other => {
            if (other !== member) {
                const distance = memberPos.distance(other.getPosition());
                if (distance < this.swarmRadius) {
                    const direction = memberPos.subtract(other.getPosition()).normalize();
                    separation = separation.add(direction.multiply(1 / distance));
                }
            }
        });
        
        return separation.normalize();
    }
    
    private calculateAlignment(member: Horizon.Entity): Horizon.Vector3 {
        if (this.swarmMembers.length === 0) return new Horizon.Vector3(0, 0, 0);
        
        let averageVelocity = new Horizon.Vector3(0, 0, 0);
        
        this.swarmMembers.forEach(other => {
            if (other !== member) {
                // Get other member's velocity (simplified)
                const otherPos = other.getPosition();
                const memberPos = member.getPosition();
                const velocity = otherPos.subtract(memberPos).normalize();
                averageVelocity = averageVelocity.add(velocity);
            }
        });
        
        return averageVelocity.multiply(1 / this.swarmMembers.length).normalize();
    }
    
    private calculateTargetSeeking(member: Horizon.Entity): Horizon.Vector3 {
        if (!this.target) return new Horizon.Vector3(0, 0, 0);
        
        const memberPos = member.getPosition();
        const targetPos = this.target.getPosition();
        return targetPos.subtract(memberPos).normalize();
    }
    
    private calculateSwarmCenter(): Horizon.Vector3 {
        let center = new Horizon.Vector3(0, 0, 0);
        
        this.swarmMembers.forEach(member => {
            center = center.add(member.getPosition());
        });
        
        return center.multiply(1 / this.swarmMembers.length);
    }
    
    private applyVelocity(member: Horizon.Entity, velocity: Horizon.Vector3) {
        const currentPos = member.getPosition();
        const newPos = currentPos.add(velocity.multiply(0.1));
        member.setPosition(newPos);
        
        // Face movement direction
        const rotation = Math.atan2(velocity.x, velocity.z) * 180 / Math.PI;
        member.setRotation(0, rotation, 0);
    }
    
    private findTarget() {
        const players = Horizon.Player.getAllPlayers();
        let closestPlayer: Horizon.Player | null = null;
        let closestDistance = Infinity;
        
        players.forEach(player => {
            const distance = this.getPosition().distance(player.getPosition());
            if (distance < closestDistance) {
                closestDistance = distance;
                closestPlayer = player;
            }
        });
        
        this.target = closestPlayer;
    }
}
```

## Dynamic Wave Progression

Create adaptive wave systems that respond to player performance.

### Adaptive Wave Manager

**Dynamic Difficulty Adjustment:**
```typescript
class AdaptiveWaveManager extends Horizon.Component {
    private currentWave: number = 1;
    private playerPerformance: PlayerPerformance = new PlayerPerformance();
    private difficultyMultiplier: number = 1.0;
    private waveConfig: WaveConfiguration = new WaveConfiguration();
    private activeEnemies: Horizon.Entity[] = [];
    
    override start() {
        this.setupAdaptiveManager();
    }
    
    private setupAdaptiveManager() {
        this.startPerformanceMonitoring();
        this.startWaveScheduling();
    }
    
    private startPerformanceMonitoring() {
        setInterval(() => {
            this.updatePlayerPerformance();
            this.adjustDifficulty();
        }, 5000); // Check every 5 seconds
    }
    
    private updatePlayerPerformance() {
        const players = Horizon.Player.getAllPlayers();
        
        players.forEach(player => {
            this.playerPerformance.updateHealth(player.getHealth());
            this.playerPerformance.updateKillRate(this.calculateKillRate());
            this.playerPerformance.updateSurvivalTime(this.calculateSurvivalTime());
        });
    }
    
    private adjustDifficulty() {
        const performance = this.playerPerformance.getOverallPerformance();
        
        if (performance > 0.8) {
            // Player is doing well, increase difficulty
            this.difficultyMultiplier = Math.min(2.0, this.difficultyMultiplier + 0.1);
        } else if (performance < 0.3) {
            // Player is struggling, decrease difficulty
            this.difficultyMultiplier = Math.max(0.5, this.difficultyMultiplier - 0.1);
        }
        
        console.log(`Difficulty adjusted to: ${this.difficultyMultiplier}`);
    }
    
    private startWaveScheduling() {
        setTimeout(() => {
            this.startNextWave();
        }, 3000);
    }
    
    private startNextWave() {
        const waveData = this.generateWaveData();
        this.spawnWave(waveData);
        
        // Schedule next wave
        const nextWaveDelay = this.calculateNextWaveDelay();
        setTimeout(() => {
            this.currentWave++;
            this.startNextWave();
        }, nextWaveDelay);
    }
    
    private generateWaveData(): WaveData {
        const baseEnemyCount = 5 + (this.currentWave - 1) * 2;
        const adjustedEnemyCount = Math.floor(baseEnemyCount * this.difficultyMultiplier);
        
        const enemyTypes = this.selectEnemyTypes();
        
        return {
            enemyCount: adjustedEnemyCount,
            enemyTypes: enemyTypes,
            spawnInterval: this.calculateSpawnInterval(),
            waveDuration: this.calculateWaveDuration()
        };
    }
    
    private selectEnemyTypes(): EnemyTypeData[] {
        const types: EnemyTypeData[] = [];
        
        if (this.currentWave <= 3) {
            types.push({ type: "MeleeEnemy", weight: 1.0 });
        } else if (this.currentWave <= 6) {
            types.push({ type: "MeleeEnemy", weight: 0.7 });
            types.push({ type: "RangedEnemy", weight: 0.3 });
        } else {
            types.push({ type: "MeleeEnemy", weight: 0.5 });
            types.push({ type: "RangedEnemy", weight: 0.3 });
            types.push({ type: "FlyingEnemy", weight: 0.2 });
        }
        
        // Add boss every 5 waves
        if (this.currentWave % 5 === 0) {
            types.push({ type: "BossEnemy", weight: 1.0 });
        }
        
        return types;
    }
    
    private calculateSpawnInterval(): number {
        const baseInterval = 1000; // 1 second
        return Math.max(200, baseInterval / this.difficultyMultiplier);
    }
    
    private calculateWaveDuration(): number {
        return 30000; // 30 seconds base duration
    }
    
    private calculateNextWaveDelay(): number {
        const baseDelay = 5000; // 5 seconds
        return Math.max(2000, baseDelay / this.difficultyMultiplier);
    }
    
    private spawnWave(waveData: WaveData) {
        console.log(`Starting Wave ${this.currentWave} with ${waveData.enemyCount} enemies`);
        
        for (let i = 0; i < waveData.enemyCount; i++) {
            setTimeout(() => {
                this.spawnEnemy(waveData.enemyTypes);
            }, i * waveData.spawnInterval);
        }
    }
    
    private spawnEnemy(enemyTypes: EnemyTypeData[]) {
        const selectedType = this.selectRandomEnemyType(enemyTypes);
        const spawnPoint = this.getRandomSpawnPoint();
        
        if (spawnPoint && selectedType) {
            const enemy = this.createEnemy(selectedType.type, spawnPoint.getPosition());
            if (enemy) {
                this.activeEnemies.push(enemy);
                enemy.onDestroy.add(() => {
                    this.removeEnemy(enemy);
                });
            }
        }
    }
    
    private selectRandomEnemyType(enemyTypes: EnemyTypeData[]): EnemyTypeData | null {
        const totalWeight = enemyTypes.reduce((sum, type) => sum + type.weight, 0);
        let random = Math.random() * totalWeight;
        
        for (const type of enemyTypes) {
            random -= type.weight;
            if (random <= 0) {
                return type;
            }
        }
        
        return enemyTypes[0] || null;
    }
    
    private getRandomSpawnPoint(): Horizon.Entity | null {
        const spawnPoints = this.getEntitiesWithTag("SpawnPoint");
        if (spawnPoints.length === 0) return null;
        
        return spawnPoints[Math.floor(Math.random() * spawnPoints.length)];
    }
    
    private createEnemy(type: string, position: Horizon.Vector3): Horizon.Entity | null {
        const enemy = this.createEntity(type);
        if (enemy) {
            enemy.setPosition(position);
        }
        return enemy;
    }
    
    private removeEnemy(enemy: Horizon.Entity) {
        const index = this.activeEnemies.indexOf(enemy);
        if (index > -1) {
            this.activeEnemies.splice(index, 1);
        }
    }
    
    private calculateKillRate(): number {
        // Calculate kills per minute
        return this.playerPerformance.kills / (this.playerPerformance.survivalTime / 60000);
    }
    
    private calculateSurvivalTime(): number {
        return Date.now() - this.playerPerformance.startTime;
    }
}

class PlayerPerformance {
    public health: number = 100;
    public kills: number = 0;
    public survivalTime: number = 0;
    public startTime: number = Date.now();
    
    updateHealth(health: number) {
        this.health = health;
    }
    
    updateKillRate(killRate: number) {
        this.kills = killRate;
    }
    
    updateSurvivalTime(survivalTime: number) {
        this.survivalTime = survivalTime;
    }
    
    getOverallPerformance(): number {
        const healthScore = this.health / 100;
        const killScore = Math.min(1.0, this.kills / 10); // Normalize to 0-1
        const survivalScore = Math.min(1.0, this.survivalTime / 300000); // 5 minutes max
        
        return (healthScore + killScore + survivalScore) / 3;
    }
}

interface WaveData {
    enemyCount: number;
    enemyTypes: EnemyTypeData[];
    spawnInterval: number;
    waveDuration: number;
}

interface EnemyTypeData {
    type: string;
    weight: number;
}

class WaveConfiguration {
    public baseEnemyCount: number = 5;
    public enemyCountIncrement: number = 2;
    public baseSpawnInterval: number = 1000;
    public baseWaveDelay: number = 5000;
}
```

## Performance Optimization

Advanced optimization techniques for large-scale wave systems.

### Enemy Pooling System

**Object Pooling Implementation:**
```typescript
class EnemyPool extends Horizon.Component {
    private pools: Map<string, Horizon.Entity[]> = new Map();
    private activeEnemies: Horizon.Entity[] = [];
    private poolSizes: Map<string, number> = new Map();
    
    override start() {
        this.initializePools();
    }
    
    private initializePools() {
        this.poolSizes.set("MeleeEnemy", 20);
        this.poolSizes.set("RangedEnemy", 15);
        this.poolSizes.set("FlyingEnemy", 10);
        this.poolSizes.set("BossEnemy", 3);
        
        this.poolSizes.forEach((size, type) => {
            this.createPool(type, size);
        });
    }
    
    private createPool(enemyType: string, size: number) {
        const pool: Horizon.Entity[] = [];
        
        for (let i = 0; i < size; i++) {
            const enemy = this.createPooledEnemy(enemyType);
            if (enemy) {
                pool.push(enemy);
            }
        }
        
        this.pools.set(enemyType, pool);
    }
    
    private createPooledEnemy(type: string): Horizon.Entity | null {
        const enemy = this.createEntity(type);
        if (enemy) {
            enemy.setActive(false); // Start inactive
            enemy.onDestroy.add(() => {
                this.returnToPool(enemy, type);
            });
        }
        return enemy;
    }
    
    public spawnEnemy(type: string, position: Horizon.Vector3): Horizon.Entity | null {
        const pool = this.pools.get(type);
        if (!pool || pool.length === 0) {
            // Pool is empty, create new enemy
            const enemy = this.createPooledEnemy(type);
            if (enemy) {
                this.activateEnemy(enemy, position);
                return enemy;
            }
            return null;
        }
        
        const enemy = pool.pop()!;
        this.activateEnemy(enemy, position);
        return enemy;
    }
    
    private activateEnemy(enemy: Horizon.Entity, position: Horizon.Vector3) {
        enemy.setActive(true);
        enemy.setPosition(position);
        this.activeEnemies.push(enemy);
        
        // Reset enemy state
        this.resetEnemyState(enemy);
    }
    
    private resetEnemyState(enemy: Horizon.Entity) {
        // Reset health, position, and other properties
        enemy.setHealth(100);
        // Add other reset logic as needed
    }
    
    private returnToPool(enemy: Horizon.Entity, type: string) {
        const index = this.activeEnemies.indexOf(enemy);
        if (index > -1) {
            this.activeEnemies.splice(index, 1);
        }
        
        enemy.setActive(false);
        
        const pool = this.pools.get(type);
        if (pool) {
            pool.push(enemy);
        }
    }
    
    public getActiveEnemyCount(): number {
        return this.activeEnemies.length;
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

---

**Advanced AI Implementation Complete**

You now have a comprehensive understanding of advanced enemy AI behaviors, group coordination, and dynamic wave progression. These systems create engaging, adaptive combat experiences that respond to player performance.

**Next Steps:**
1. Implement formation AI for coordinated enemy groups
2. Create adaptive difficulty systems
3. Optimize performance with object pooling
4. Test and balance your wave systems

---

*This tutorial is part of the Horizon Worlds Creator Documentation. For more tutorials and resources, visit the [main documentation hub](https://github.com/MHCPCreators/worlds-documentation).*
