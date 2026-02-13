# Neural Network Maze Navigator - Specification

## 1. Project Overview

- **Project Name**: Neural Maze Runner
- **Type**: Interactive Web Application (Single HTML File)
- **Core Functionality**: A creature equipped with a neural network learns to navigate through a maze using a genetic algorithm. Users can observe training in real-time and export/import neural network weights.
- **Target Users**: Developers, students, and enthusiasts interested in neural networks and genetic algorithms

---

## 2. UI/UX Specification

### Layout Structure

```
+--------------------------------------------------+
|  HEADER: Title + Controls                       |
+--------------------------------------------------+
|                                                  |
|  MAIN CANVAS: Maze + Creature Visualization     |
|  (800x600px, centered)                          |
|                                                  |
+--------------------------------------------------+
|  INFO PANEL: Stats + Neural Network View        |
+--------------------------------------------------+
```

- **Header**: 60px height, contains title and control buttons
- **Main Canvas**: 800x600px, centered in viewport
- **Info Panel**: 200px height, displays stats and network visualization

### Visual Design

**Color Palette**:
- Background: `#0a0a0f` (deep space black)
- Canvas Background: `#12121a` (dark navy)
- Maze Walls: `#2d3748` (slate gray)
- Maze Path: `#1a1a2e` (dark purple-gray)
- Creature (best): `#00ff88` (neon green)
- Creature (current): `#ff6b6b` (coral red)
- Creature (dead): `#4a5568` (gray)
- UI Accent: `#7c3aed` (electric purple)
- Text Primary: `#e2e8f0` (light gray)
- Text Secondary: `#94a3b8` (muted gray)
- Success: `#10b981` (emerald)
- Warning: `#f59e0b` (amber)

**Typography**:
- Font Family: `'JetBrains Mono', 'Fira Code', monospace`
- Title: 28px, bold, letter-spacing: 2px
- Body Text: 14px
- Stats: 12px, tabular-nums

**Spacing**:
- Container padding: 24px
- Element gaps: 16px
- Button padding: 12px 24px

**Visual Effects**:
- Canvas border: 2px solid `#7c3aed` with glow effect
- Buttons: Subtle gradient with hover glow
- Neural network nodes: Pulsing animation
- Creature trail: Fading trail effect

### Components

**Control Buttons**:
- Start/Pause Training - Toggle button
- Reset - Resets simulation
- Speed Control - Slider (1x to 10x)
- Export Network - Downloads JSON file
- Import Network - File input

**Stats Display**:
- Generation number
- Best fitness score
- Current fitness score
- Time survived
- Steps taken
- Alive creatures count

**Neural Network Visualization**:
- Input layer: 4 nodes (raycast distances: left, front-left, front, front-right)
- Hidden layer: 6 nodes
- Output layer: 2 nodes (turn left, turn right)
- Color-coded connections (green=positive, red=negative)
- Node activation display

---

## 3. Functionality Specification

### Maze Generation
- Predefined maze layout (16x11 grid)
- Start position marked with green tint
- Goal position marked with gold/amber tint
- Walls rendered as filled rectangles
- Path as darker areas

### Creature
- Circular shape, 8px radius
- Ray sensors: 4 rays projecting forward (angles: -45°, -15°, 15°, 45°)
- Sensor length: 60px
- Movement: Constant forward speed (2px/frame), rotation controlled by neural network
- Collision detection with walls

### Neural Network
- Architecture: 4 input nodes → 6 hidden nodes → 2 output nodes
- Activation function: Hyperbolic tangent (tanh)
- Weights initialized randomly between -1 and 1
- Outputs: [turn_left_strength, turn_right_strength]
- Final rotation = turn_left - turn_right

### Genetic Algorithm
- Population size: 50 creatures per generation
- Selection: Top 20% survive to next generation
- Reproduction: Mutated copies of survivors fill population
- Mutation rate: 5% chance per weight
- Mutation strength: 0.1 (weight ± random(-0.1, 0.1))
- Fitness calculation:
  - Base: Distance to goal when died or reached goal
  - Bonus: Faster completion = higher fitness
  - Penalty: Wall collisions reduce fitness
  - Goal reached: Fitness = 1000 + (2000 - steps_taken)

### Training Loop
1. Initialize population with random weights
2. For each creature:
   - Read sensors
   - Feed through neural network
   - Update position/rotation
   - Check collisions
   - Update fitness
3. When all creatures dead or goal reached:
   - Sort by fitness
   - Select top performers
   - Create next generation with mutation
4. Repeat

### Import/Export
- Export: Save current best neural network as JSON
  ```json
  {
    "generation": 42,
    "fitness": 850.5,
    "weights": {
      "hidden": [[...], ...],
      "output": [[...], ...]
    },
    "biases": {
      "hidden": [...],
      "output": [...]
    }
  }
  ```
- Import: Load JSON file and apply to creature
- Visual feedback on successful import/export

### Edge Cases
- Creature goes out of bounds: Kill immediately
- No path to goal: Algorithm continues trying
- Import invalid file: Show error message
- Import file without required fields: Show error message

---

## 4. Acceptance Criteria

### Visual Checkpoints
- [x] Maze renders clearly with distinct walls and paths
- [x] Creatures visible as colored circles
- [x] Ray sensors visible as lines from creature
- [x] Neural network visualization updates in real-time
- [x] Stats panel shows all metrics
- [x] Buttons have hover effects

### Functional Checkpoints
- [x] Creatures move and rotate based on neural network output
- [x] Collision detection works (creatures die on wall contact)
- [x] Genetic algorithm creates new generations
- [x] Fitness improves over generations (eventually)
- [x] Export downloads valid JSON file
- [x] Import loads and applies neural network
- [x] Training can be paused and resumed
- [x] Speed control affects simulation speed

### Performance
- Smooth 60fps animation
- No memory leaks over extended training
- Responsive UI during training
