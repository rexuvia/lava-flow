# Lava Flow - Game Design Document

## Game Overview
**Lava Flow** transforms from a sandbox simulation into a strategic puzzle game where players must harness the chaotic power of flowing lava to achieve specific objectives. The core fun comes from understanding and manipulating the cellular automata physics—predicting how lava will flow, pool, and cool across varied terrain.

---

## 1. Level Structure

### Level Count: 12 Levels

We use a 3-act structure with 4 levels per act. Each act introduces new mechanics while increasing complexity.

### Difficulty Progression

| Act | Levels | Focus | Difficulty Curve |
|-----|--------|-------|------------------|
| Act I: Ignition | 1-4 | Learn core mechanics, simple goals | Linear, forgiving |
| Act II: Control | 5-8 | Constraints, efficiency, planning | Exponential, requires foresight |
| Act III: Mastery | 9-12 | Complex multi-objective puzzles | High precision, expert timing |

### What Makes Each Level Unique

Each level has a **signature element**:
- **Terrain Configuration**: Unique heightmap that creates interesting flow patterns
- **Objective Type**: One of 6 objective variants
- **Constraint Combination**: Lava budget + pour points + optional time limit
- **Interactive Elements**: Barriers, valves, bridges, collection basins

---

## 2. Goals & Objectives

### Six Objective Types

#### 1. **Fill Target** (Basic)
Fill designated target zone(s) with a minimum amount of lava.
- Visual: Glowing outline on terrain
- Success: Zone contains ≥ required lava amount
- Challenge: Efficient routing, avoiding waste

#### 2. **Reach Point** (Basic)
Get lava to touch specific point(s) on the map.
- Visual: Beacon pillars with pulsing light
- Success: Any lava reaches the point
- Challenge: Terrain navigation, building paths

#### 3. **Collection** (Intermediate)
Fill multiple collection basins to specific levels.
- Visual: Marked basins with fill meters
- Success: All basins filled to within tolerance (±10%)
- Challenge: Balanced distribution

#### 4. **Connect** (Intermediate)
Create a continuous lava path between two or more points.
- Visual: Connected beacons that light up when linked
- Success: Lava forms uninterrupted connection
- Challenge: Bridge gaps, control spread

#### 5. **Sequence** (Advanced)
Activate targets in specific order using lava timing.
- Visual: Numbered targets with countdown timers
- Success: Hit targets in order within time windows
- Challenge: Understanding flow rates, precise pouring

#### 6. **Contain** (Expert)
Fill an area WITHOUT spilling into forbidden zones.
- Visual: Red "danger zones" that trigger failure
- Success: Objective complete with no danger zone contamination
- Challenge: Maximum control, overflow management

### Objective Distribution

| Level | Primary Objective | Secondary Challenge |
|-------|-------------------|---------------------|
| 1 | Fill Target | Tutorial level, unlimited lava |
| 2 | Reach Point | Single pour point |
| 3 | Fill Target | Limited lava budget |
| 4 | Collection | Two basins, efficiency matters |
| 5 | Connect | First appearance of barriers |
| 6 | Collection | Time pressure introduced |
| 7 | Reach Point | Multiple targets, sequence implied |
| 8 | Connect | Valves introduced |
| 9 | Sequence | Precise timing required |
| 10 | Contain | No-fail zones introduced |
| 11 | Collection + Connect | Dual objective |
| 12 | Sequence + Contain | Ultimate challenge |

---

## 3. Mechanics

### Scoring System

**Base Score Components:**
- **Objective Completion**: 1000-5000 points (varies by level)
- **Lava Efficiency**: Bonus for using less than budget
  - Formula: `(1 - used/budget) × 2000` (if under budget)
- **Time Bonus**: (If applicable) `(timeLimit - timeUsed) × 10`
- **Precision Bonus**: For Collection objectives, closer to target = more points

**Star Rating Thresholds:**
- ⭐ (1 Star): Complete the objective (minimum viable)
- ⭐⭐ (2 Stars): Score ≥ 70% of maximum possible
- ⭐⭐⭐ (3 Stars): Score ≥ 90% of maximum possible

### Constraints

#### Lava Budget
- Each level has a **Lava Budget** (e.g., 50 units)
- Pouring consumes budget: 1 unit per pour action
- Continuous pour drains continuously
- Run out = level fails (unless objective already achieved)

#### Pour Points (Strategic)
- Some levels restrict **where** you can pour
- Visual: Glowing circles mark valid pour locations
- Forces players to work with terrain, not against it

#### Time Limits (Selective)
- Only on specific challenge levels (marked with ⏱️)
- Adds urgency to Sequence and Collection objectives
- Time bonuses reward speed on all levels

### Interactive Elements

#### 1. **Barriers** (Solid Walls)
- Impassable to lava, forces rerouting
- Can be permanent or toggleable
- Visual: Dark stone walls with cracks

#### 2. **Gates** (Controlled Barriers)
- Click to open/close
- Allows player to control flow timing
- Visual: Metal gates with pulsing red lights

#### 3. **Valves** (Flow Modifiers)
- Divert lava between multiple outputs
- Click to rotate/change direction
- Visual: Stone valve mechanisms with arrow indicators

#### 4. **Bridges** (Elevated Channels)
- Lava can flow across without filling below
- Creates elevated paths
- Visual: Arched stone bridges

#### 5. **Heat Zones** (Modifiers)
- Hot zones: Lava cools slower, flows faster
- Cold zones: Lava cools faster, may solidify
- Visual: Color-coded ground (red/blue tint)

#### 6. **Collectors** (Auto-siphon)
- Removes lava when filled to threshold
- Useful for Collection objectives
- Visual: Basin with drain pipe

---

## 4. Progression

### Level Unlocking

**Linear Progression**: Complete level N to unlock level N+1
- This ensures players learn mechanics in order
- Prevents frustration from attempting advanced puzzles too early

**Star Requirements for Late Levels:**
- Levels 9-10: Require 8 total stars to unlock
- Levels 11-12: Require 16 total stars to unlock
- This encourages replay for better scores

### Meta-Progression: Lava Mastery

No persistent upgrades (keeps game pure), but unlocks:

| Stars Earned | Unlock |
|--------------|--------|
| 6 | Sandbox Mode (free play with all terrain types) |
| 12 | Level Editor (create/share custom levels) |
| 18 | Challenge Mode (harder versions of all levels) |
| 24 (all) | Endless Mode (procedural infinite levels) |

### Replayability Features

1. **Leaderboards**: Best score per level (localStorage)
2. **Perfect Run Tracking**: Indicator for 3-star completions
3. **Par Challenges**: Beat level with <X% of budget used
4. **Speedrun Mode**: Separate timing-only challenge

---

## 5. UI Additions

### Main Menu Screen

```
┌─────────────────────────────────────┐
│           🔥 LAVA FLOW 🔥           │
│                                     │
│    [Continue]  [Level Select]       │
│                                     │
│    [Sandbox]   [Settings]           │
│                                     │
│         Stars: ⭐⭐⭐⭐⭐⭐            │
└─────────────────────────────────────┘
```

### Level Select Screen

**Grid Layout**: 3 columns × 4 rows = 12 levels
Each level card shows:
- Level number and preview thumbnail
- Best star rating earned (⭐⭐⭐ / ⭐⭐ / ⭐ / none)
- Best score
- Lock status (🔒 for locked levels)
- Objective type icon

**Act Separation**: Visual dividers between acts with thematic names
- Act I: "First Eruption" (levels 1-4)
- Act II: "Controlled Burn" (levels 5-8)
- Act III: "Volcanic Mastery" (levels 9-12)

### In-Game HUD

```
┌─────────────────────────────────────────────────────────────┐
│ Level 5: Bridge Builder                    ⏸️  🔄  🏠       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│                      [GAME AREA]                            │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│ 🎯 Connect the beacons                                      │
│ ⭐⭐⭐ (2500 / 3000)                                        │
│ 🌋 Budget: 23/50    ⏱️ Time: 45s    📊 Flow Rate: 12/s      │
└─────────────────────────────────────────────────────────────┘
```

**HUD Elements:**

| Element | Description |
|---------|-------------|
| Level Title | Name and number |
| Pause/Restart/Home | Control buttons |
| Objective Panel | Current goal with icon |
| Star Progress | Current score vs thresholds |
| Lava Budget | Current / Maximum |
| Timer | Only shows on time-limited levels |
| Flow Rate | Optional: Lava simulation speed indicator |
| Gate Controls | Appear when gates are present |

### Victory Screen

```
┌─────────────────────────────────────┐
│         🎉 LEVEL COMPLETE! 🎉       │
│                                     │
│           ⭐⭐⭐ ACHIEVED            │
│                                     │
│    Score: 2,847                     │
│    Best:  2,950                     │
│                                     │
│    Efficiency: 85%  🏆              │
│    Time: 1:24  ⏱️                   │
│                                     │
│    [Next Level]  [Retry]  [Menu]    │
└─────────────────────────────────────┘
```

### Failure Screen

```
┌─────────────────────────────────────┐
│         💀 LEVEL FAILED 💀          │
│                                     │
│    Reason: Lava budget exhausted    │
│                                     │
│    [Retry]  [Skip - Watch Ad]  [Menu]│
└─────────────────────────────────────┘
```

(Note: "Watch Ad" can be a fun fake-out or just "Get Hint")

---

## 6. Level Design Details

### Level 1: "First Pour" (Tutorial)
- **Objective**: Fill Target
- **Terrain**: Gentle slope, bowl-shaped target zone
- **Budget**: Unlimited
- **Teaching**: How to pour, how lava flows

### Level 4: "Twin Basins" (First Real Challenge)
- **Objective**: Collection
- **Terrain**: Two depressions at different heights
- **Budget**: 40 (tight)
- **Challenge**: Balance fill between basins, don't overfill

### Level 7: "The Waterfall" (Vertical Challenge)
- **Objective**: Reach Point
- **Terrain**: Stepped descent with a gap
- **Budget**: 30
- **Challenge**: Build up enough lava to overflow gap

### Level 10: "Forbidden Valley" (Precision)
- **Objective**: Contain
- **Terrain**: Narrow path with danger zones on sides
- **Budget**: 25
- **Challenge**: Slow, controlled pouring

### Level 12: "The Volcano's Heart" (Finale)
- **Objective**: Sequence + Contain
- **Terrain**: Complex multi-level with valves and gates
- **Budget**: 50
- **Challenge**: Activate 4 targets in order without spilling

---

## 7. Technical Implementation Notes

### State Management
```javascript
const gameState = {
  mode: 'MENU' | 'PLAYING' | 'PAUSED' | 'VICTORY' | 'DEFEAT',
  currentLevel: 0,
  levelData: { /* terrain, objectives, constraints */ },
  progress: {
    lavaUsed: 0,
    timeElapsed: 0,
    objectivesCompleted: [],
    score: 0
  },
  unlockedLevels: [1, 2, 3, ...],
  starsPerLevel: { 1: 3, 2: 2, ... }
};
```

### Level Data Format
```javascript
const levels = [
  {
    id: 1,
    name: "First Pour",
    terrain: 'valley',
    objective: {
      type: 'FILL_TARGET',
      zones: [{ x: 50, y: 60, radius: 10, required: 5 }]
    },
    constraints: {
      budget: Infinity,
      pourPoints: null,
      timeLimit: null
    },
    interactive: [],
    starThresholds: [1000, 1500, 2000]
  },
  // ... more levels
];
```

### Save Data (localStorage)
```javascript
{
  "version": 1,
  "unlockedLevels": [1, 2, 3, 4],
  "levelProgress": {
    "1": { "stars": 3, "bestScore": 2150 },
    "2": { "stars": 2, "bestScore": 1800 },
    // ...
  },
  "totalStars": 18,
  "sandboxUnlocked": true
}
```

---

## 8. Aesthetic Direction

### Visual Style
- **Terrain**: Grayscale heightmap with subtle lighting
- **Lava**: Bright orange-red with bloom/glow effect
- **UI**: Dark volcanic theme with orange accents
- **Objectives**: Glowing holographic overlays on terrain
- **Interactive Elements**: Mechanical, slightly sci-fi but grounded

### Audio Direction (Future)
- Ambient: Deep rumbling, distant volcano sounds
- Interaction: Satisfying pour sound, mechanical gate clicks
- Victory: Crescendo with cooling hiss
- Failure: Low rumble fade

---

## 9. Success Metrics

A successful implementation should make players feel:
1. **Smart** when they predict lava flow correctly
2. **Powerful** when they control chaos
3. **Satisfied** when they achieve efficiency
4. **Challenged** but not frustrated

The core loop: **Plan → Pour → Watch → Adjust → Achieve**

---

## 10. Future Expansion Ideas

- **Multiplayer**: Async challenges, shared leaderboards
- **Workshop**: Community level sharing
- **New Elements**: Ice (melts into water), obsidian (lava solidifies), steam physics
- **Daily Challenges**: Procedural level with leaderboard
- **Mobile**: Touch-optimized controls already supported

---

*Document Version: 1.0*
*Designed for: Single-file HTML implementation*
*Target: Transform sandbox into engaging puzzle experience*
