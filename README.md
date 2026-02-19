# 🌋 Lava Flow - Fluid Dynamics Simulation Game

A mesmerizing browser-based game where you pour glowing molten lava onto procedurally generated terrain and watch realistic fluid dynamics in action.

## 🎮 Gameplay

Click or tap anywhere on the terrain to pour lava from that point. Watch as the molten lava:
- Flows downhill following terrain contours
- Pools in valleys and low points
- Gradually cools from bright white-hot to dark solidified black
- Creates particle effects and glowing trails

## 🔧 Physics Approach

### Cellular Automata Fluid Simulation

The game uses a **cellular automata** approach for efficient, visually satisfying fluid simulation:

1. **Grid-Based System**: The terrain is divided into a 120×120 grid where each cell stores:
   - `terrain[y][x]` - Height of the ground at that position
   - `lava[y][x]` - Amount of lava present
   - `temperature[y][x]` - Heat level (1.0 = white-hot, 0.0 = cooled)

2. **Flow Mechanics**:
   - Each simulation step, lava flows from higher cells to lower neighbors
   - Flow rate is proportional to height difference: `flowRate = heightDiff × 0.3`
   - Diagonal flow is reduced by 70% to create realistic channeling
   - Lava is distributed to multiple downhill neighbors simultaneously

3. **Heat Dissipation**:
   - Temperature decreases by 0.5% per frame (`temp × 0.995`)
   - Hot lava (temp > 0.7) appears white/yellow/orange
   - Cooling lava (temp < 0.5) transitions through red to dark red/black
   - Solidified lava remains visible as dark terrain

4. **Particle System**:
   - Sparks spawn randomly from flowing hot lava
   - Each particle has velocity, gravity, and lifetime
   - Creates visual interest and emphasizes heat

### Performance Optimizations

- **Efficient neighbor scanning**: Only checks 8 immediate neighbors
- **Conditional updates**: Cells with no lava skip calculations
- **Canvas rendering**: Direct pixel manipulation for smooth 60fps
- **Grid size tuning**: 120×120 balances detail vs. performance

## 🎨 Visual Features

- **Temperature-based colors**:
  - White (>0.9) → Yellow (>0.7) → Orange (>0.5) → Red (>0.3) → Dark Red (>0.1) → Black
- **Glowing bloom effects** using canvas shadow blur
- **Particle sparks** in orange/yellow with physics
- **Terrain shading** based on height
- **Dark aesthetic** (#0a0a0f background, #1a1a2e terrain)

## 🏔️ Terrain Types

1. **Mountain Valley** - U-shaped valley perfect for lava rivers
2. **Crater** - Circular depression that pools lava in the center
3. **Steps** - Tiered platforms create cascading waterfalls
4. **Rolling Hills** - Sinusoidal terrain with multiple flow paths
5. **Funnel** - Converges lava toward the center

Plus a **Randomize** button for infinite procedural terrain!

## 🎯 Controls

- **Click/Tap** - Pour lava at that location
- **Hold** - Continuous pour while holding
- **Clear Lava** - Reset all lava but keep terrain
- **Next Terrain** - Cycle through 5 preset terrains
- **Randomize** - Generate a new random terrain

## 📱 Technical Details

- **Single HTML file** - No external dependencies
- **Canvas-based** - Hardware-accelerated rendering
- **Mobile-first** - Touch controls with `touch-action: none`
- **Responsive** - Adapts to any screen size
- **60fps target** - Smooth animation via requestAnimationFrame

## 🚀 How to Play

Simply open `index.html` in any modern web browser. No build process, no installation needed!

## 🧮 Algorithm Complexity

- **Time**: O(n²) per frame where n = GRID_SIZE (optimized with early exits)
- **Space**: O(n²) for three grids (terrain, lava, temperature)
- **Particle system**: O(p) where p = active particles (typically 50-200)

The simulation is intentionally simplified to prioritize visual satisfaction and performance over perfect physical accuracy. Real lava has complex properties (viscosity, solidification, gas expansion) that would be computationally expensive—this implementation captures the essence while staying buttery smooth!

---

## 📜 CHANGELOG

### Version 2.0 - Full Puzzle Game Release (2026-02-19)

**🎮 MAJOR TRANSFORMATION: Sandbox → Complete Puzzle Game**

The game has been completely redesigned from a simple physics sandbox into a full-featured puzzle game with progression, objectives, and challenge!

#### 🏆 12-Level Campaign Structure

**Act I: First Eruption (Levels 1-4)**
- **Level 1 - "First Pour"**: Learn the basics by filling a target zone
- **Level 2 - "The Beacon"**: Guide lava to reach a specific point with budget constraints
- **Level 3 - "Conservation"**: Efficient lava usage in a crater
- **Level 4 - "Twin Basins"**: Balance lava distribution between two basins

**Act II: Controlled Burn (Levels 5-8)**
- **Level 5 - "Bridge Builder"**: Connect beacons across a gap using barriers
- **Level 6 - "Time Trial"**: Race against the clock to fill multiple basins
- **Level 7 - "The Waterfall"**: Navigate vertical terrain with limited pour points
- **Level 8 - "The Valve"**: Master gate mechanics to connect three points

**Act III: Volcanic Mastery (Levels 9-12)**
- **Level 9 - "Chain Reaction"**: Activate targets in sequence with precise timing
- **Level 10 - "Forbidden Valley"**: Fill target while avoiding danger zones
- **Level 11 - "Double Duty"**: Complete multiple objectives simultaneously under time pressure
- **Level 12 - "The Volcano's Heart"**: Final challenge combining sequence, containment, and gates

#### 🎯 6 Objective Types

1. **FILL_TARGET**: Fill designated zones with a specific amount of lava
   - Visual: Cyan dashed circles indicating target zones
   - Example: Level 1, 3

2. **REACH_POINT**: Guide lava to reach specific beacon locations
   - Visual: Yellow/green beacon circles with pulsing effects
   - Example: Level 2, 7

3. **COLLECTION**: Fill multiple basins to precise targets (±15% tolerance)
   - Visual: Cyan circles with fill meters showing progress
   - Example: Level 4, 6

4. **CONNECT**: Create continuous lava paths between multiple points
   - Visual: Multiple beacons that must be linked
   - Uses BFS pathfinding to verify connectivity
   - Example: Level 5, 8

5. **SEQUENCE**: Activate targets in specific order within time windows
   - Visual: Numbered circles (1→2→3) that light up when activated
   - Example: Level 9, 12

6. **CONTAIN**: Fill a target without contaminating danger zones
   - Visual: Cyan target zone + red danger zones
   - Instant failure if danger zones are contaminated (>0.5 lava)
   - Example: Level 10, 12

**Plus hybrid objectives:**
- **MULTI**: Complete multiple objectives simultaneously (Level 11)
- **SEQUENCE_CONTAIN**: Sequence activation + danger zone avoidance (Level 12)

#### 🎛️ Interactive Elements

**Barriers**
- Static obstacles that block lava flow
- Used in Levels 5, 7, 11
- Rendered as dark gray blocks

**Gates**
- Toggleable barriers controlled by clicking circular buttons
- Click the gate button to open/close
- Open gates allow flow, closed gates block it
- Visual feedback: Red (closed) ↔ Green (open)
- Used in Levels 8, 12

#### 🎨 Complete UI System

**Main Menu**
- Continue button (resumes last unlocked level)
- Level Select button
- Sandbox Mode button (preserved original gameplay)
- Total stars counter display

**Level Select Screen**
- Grid layout with 12 level cards
- Act separators ("Act I: First Eruption", etc.)
- Per-level displays:
  - Level number and name
  - Star rating (⭐⭐⭐ or ☆☆☆)
  - Best score
  - Lock icon (🔒) for locked levels
- Hover effects on unlocked levels
- Progressive unlock system

**In-Game HUD**
- Level title and number
- Lava budget tracker (e.g., "50/60" or "∞")
- Timer (for time-limited levels)
- Current score (live updates)
- Star rating preview (⭐⭐⭐ / ☆☆☆)
- Objective description with emoji
- Pause, Restart, Menu buttons

**Victory Screen**
- Star rating display (1-3 stars)
- Final score
- Best score comparison
- Efficiency percentage
- Time taken (for timed levels)
- Next Level, Retry, Menu buttons

**Defeat Screen**
- Failure reason display:
  - "Lava budget exhausted"
  - "Time limit exceeded"
  - "Contaminated danger zone!"
- Retry and Menu buttons

#### 📊 Scoring & Star Rating System

**Score Calculation:**
- **Base completion**: 2000 points
- **Efficiency bonus**: Up to 2000 points based on lava conservation
  - Formula: `(1 - lavaUsed/budget) × 2000`
  - Encourages minimal lava usage
- **Time bonus**: 10 points per second remaining (for timed levels)
  - Formula: `(timeLimit - timeElapsed) × 10`

**Star Thresholds:**
Each level has custom thresholds that increase with difficulty:
- Early levels: 1000/1500/2000 (easier to 3-star)
- Mid levels: 2500/3300/4200 (moderate challenge)
- Late levels: 5000/6500/8000 (mastery required)

**Total possible**: 36 stars (12 levels × 3 stars each)

#### 💾 localStorage Persistence

**Saved Data:**
- Unlocked levels array
- Stars earned per level (highest only)
- Best score per level (highest only)
- Version number for future migrations

**Save Triggers:**
- After completing any level
- Automatic on victory screen

**Data Structure:**
```json
{
  "version": 1,
  "unlockedLevels": [1, 2, 3, ...],
  "starsPerLevel": { "1": 3, "2": 2, ... },
  "scorePerLevel": { "1": 2500, "2": 1800, ... }
}
```

#### 🗺️ Terrain Variety

12 unique terrain generators tailored to each level:
- `valley` - U-shaped valley (Level 1)
- `slope` - Gradual downhill (Level 2)
- `crater` - Circular depression (Level 3)
- `twinBasins` - Two separate basins (Level 4)
- `gap` - Chasm requiring bridging (Level 5)
- `steps` - Cascading platforms (Level 6)
- `waterfall` - Vertical drop (Level 7)
- `funnel` - Converging terrain (Level 8)
- `sequence` - Sinusoidal terrain (Level 9)
- `narrow` - Constrained channels (Level 10)
- `complex` - Multi-zone terrain (Level 11)
- `finale` - Sectored challenge (Level 12)

#### 🎮 Constraints System

**Budget Limits:**
- Infinite budget for tutorial/sandbox levels
- Constrained budget (30-60 units) for challenge levels
- Real-time tracking in HUD
- Defeat condition when exceeded

**Pour Point Restrictions:**
- Some levels restrict where you can pour
- Visual indicator: Orange dashed circles
- Only allow pouring within radius of designated points
- Example: Level 2 (waterfall source), Level 7 (cliff top)

**Time Limits:**
- Optional per-level time constraints (90-120 seconds)
- Real-time countdown in HUD
- Defeat condition when exceeded
- Adds urgency to strategic planning

#### 🔓 Progressive Unlock System

- All players start with only Level 1 unlocked
- Completing a level unlocks the next
- Any completion (even 1-star) unlocks progression
- Lock icons (🔒) on locked level cards
- Can replay any unlocked level for better scores/stars

#### ⚙️ Game State Management

**7 Game States:**
1. `MENU` - Main menu screen
2. `LEVEL_SELECT` - Level selection grid
3. `PLAYING` - Active gameplay
4. `PAUSED` - Paused gameplay (renders continue, HUD visible)
5. `VICTORY` - Level completed successfully
6. `DEFEAT` - Level failed
7. `SANDBOX` - Free-play mode (original sandbox preserved)

**State Transitions:**
- Clean transitions between all states
- Proper HUD show/hide based on state
- Overlay system with fade-in animations

#### 🎨 Enhanced Visuals

**New UI Polish:**
- Gradient backgrounds with glow effects
- Animated modal popups (slide-in/fade-in)
- Box shadows with lava-orange theme (#ff6600)
- Button hover effects (lift + glow intensification)
- Pulse animations on objective indicators
- Responsive grid layouts for level cards

**In-Game Enhancements:**
- Objective overlay rendering (targets, beacons, danger zones)
- Color-coded feedback (cyan=target, yellow=beacon, red=danger, green=complete)
- Fill meters for collection objectives
- Numbered sequence indicators
- Gate button indicators

#### 🧪 Technical Improvements

**Architecture:**
- Modular level definition system
- Reusable terrain generators
- Flexible objective checking system
- Interactive element framework
- Clean separation of concerns (simulation/rendering/UI/state)

**Performance:**
- All enhancements maintain 60fps target
- Efficient objective checking (only during PLAYING state)
- Optimized pathfinding for CONNECT objectives
- Minimal localStorage I/O

**Code Quality:**
- Comprehensive objective validation
- Robust state management
- Clean event handler organization
- Consistent naming conventions

---

**Built with ❤️ and 🌋 by OpenClaw**
