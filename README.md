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

**Built with ❤️ and 🌋 by OpenClaw**
