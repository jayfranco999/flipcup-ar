# Flip Cup AR - Game Design Document

## Current Version: v1.0 (Vaporwave Edition)

---

## Core Concept
Browser-based Flip Cup party game with AR webcam integration. Players use hand gestures (raise hand to signal ready, flick up to flip) tracked via MediaPipe. Teams race to flip cups with a timing-based shot meter that adds skill + chaos.

**Target Vibe**: Late-night bar chaos, vaporwave aesthetics, maximum party energy

---

## Implemented Features (v1.0)

### Controls
- Hand raise (wrist Y < 0.35) = "I'm ready" signal
- Upward flick gesture = flip attempt
- Position on screen (left/right) determines which team you control

### Game Modes

| Mode | Description | Win Condition |
|------|-------------|---------------|
| **Classic** | Relay race, 5 flips per player | First team to complete all flips |
| **Sudden Death** | Miss once = eliminated | Last team with players standing |
| **Speed Demon** | 60 second timer, unlimited flips | Most total flips wins |

### Shot Meter System
- Oscillating needle, left-to-right
- **Zones**: Red (0-20%) → Yellow (20-35%) → Green (35-47.5%) → Sweet Spot (47.5-52.5%) → mirrors back
- **Success Rates**: Sweet 95%, Green 70%, Yellow 35%, Red 10%
- **Dynamic Speed**: Meter speeds up per completed player (pressure builds)
- **Mercy Rule**: 5 failed attempts = automatic flip

### Feedback System
| Feedback | Trigger | Style |
|----------|---------|-------|
| PERFECT! | Sweet spot success | Green glow, pop animation |
| CLEAN! | Green zone success | Cyan glow |
| SLOPPY! | Yellow zone success | Orange glow |
| LUCKY! | Red zone success | Magenta glow |
| CHOKE! | Green zone miss | Red, emphasis |
| PATHETIC! | 3+ misses | Red, shake animation |
| CLUTCH! | Success after 4+ attempts | Gold glow, dramatic |

### Visuals
- Vaporwave sunset gradient background
- Animated perspective grid (80s aesthetic)
- Retro sun with horizontal line cutouts
- Neon glow effects throughout
- Press Start 2P retro font
- Cyan/magenta color scheme
- Hand position indicators on webcam
- Gesture labels (RAISED, FLICK!)

---

## Drinking Rules (IRL)

### Current (Simple)
- Drink your cup before raising hand each turn
- Losing team drinks at end of game

### Planned Rule Expansions

| Rule | Trigger | Penalty |
|------|---------|---------|
| **Choke Tax** | Miss in green zone | Take a sip |
| **Mercy Shame** | Need all 5 attempts | Finish your drink |
| **Combo Breaker** | Break a 3+ combo | Take a sip |
| **Lucky Bastard** | Red zone success | OTHER team drinks |
| **Pathetic Tax** | Get "PATHETIC" feedback | 2 sips |
| **Perfect Pressure** | Team gets all perfects | Losers chug |
| **Elimination Chug** | Eliminated in sudden death | Finish it |
| **Speed Shame** | Fewest flips in Speed Demon | Waterfall |

---

## Future Iterations

### v1.1 - Chaos Modifiers

#### Meter Madness
| Modifier | Description | Implementation |
|----------|-------------|----------------|
| **Drunk Meter** | Meter wobbles/sways sinusoidally | Add sin wave to needle position |
| **Earthquake** | Whole meter shakes violently | CSS shake animation + random offset |
| **Shrinking Zone** | Green zone smaller each player | Dynamic flex values |
| **Ghost Needles** | Fake needles appear | Multiple needle elements, one real |
| **Blackout Zones** | Parts of meter go dark | Random opacity transitions |
| **Speed Surge** | Random acceleration mid-swing | Variable speed multiplier |
| **Reverse!** | Direction randomly flips | Random meterDir changes |
| **Strobe** | Meter flickers on/off | Opacity animation |

#### Visual Chaos
- **Glitch Effect**: RGB split + scan lines on fails
- **Screen Shake**: Camera rumble on big moments
- **Confetti Explosion**: Particle burst on success
- **Drunk Vision**: Screen blur/sway under pressure
- **Flash Bang**: Screen flash on clutch moments
- **Zoom Punch**: Dramatic zoom in/out on results

### v1.2 - Challenge Rounds

Rotating modifiers that apply to specific players/rounds:

1. **Blindfold Round** - Meter completely hidden, pure rhythm
2. **Precision Round** - Only sweet spot counts as success
3. **Speed Round** - 2x meter speed
4. **Drunk Vision** - Blur + chromatic aberration effect
5. **Mirror Round** - Visual feedback feels inverted
6. **Pressure Round** - One attempt only, no mercy
7. **Chaos Round** - Multiple modifiers stack

### v1.3 - Power-ups & Sabotage

Earned through combos or special achievements:

| Power-up | Effect | Earn Condition |
|----------|--------|----------------|
| **Freeze** | Freeze opponent's meter 2 sec | 3x combo |
| **Blackout** | Cover opponent's meter briefly | Perfect hit |
| **Speed Boost** | Opponent's meter 2x faster | Lucky (red zone success) |
| **Chaos Card** | Random effect triggers | 5x combo |
| **Shield** | One free miss protection | Clutch flip |
| **Steal** | Success steals opponent progress | Perfect in Sudden Death |

### v1.4 - Advanced Party Features

#### Callout System
- Before flip, player can call their shot: "PERFECT!", "CLEAN!", etc.
- Correct callout = bonus points
- Wrong callout = penalty/drink
- Adds psychological pressure

#### Shame Cam
- On miss, webcam zooms in on player's reaction
- Held for 2-3 seconds
- Maximum embarrassment

#### Crowd Prompts
- Text appears for spectators: "CHUG CHUG CHUG", "PRESSURE!", "ONE MORE!"
- Gamifies the spectator experience

#### Highlight Reel
- Auto-captures key moments (perfect flips, chokes, comebacks)
- Generates shareable clip at end

### v1.5 - Tournament Mode

- Bracket system for larger groups
- Best of 3/5 matches
- Loser bracket for redemption
- Final championship round with all modifiers

---

## Technical Architecture

### Current Stack
- Single HTML file (portable, easy hosting)
- MediaPipe Hands for gesture tracking
- CSS animations for visual effects
- Vanilla JavaScript state management
- GitHub Pages hosting

### Performance Considerations
- ModelComplexity: 0 (fastest hand tracking)
- RequestAnimationFrame for smooth meter
- CSS transforms for GPU-accelerated animations
- Minimal DOM manipulation during gameplay

### Gesture Detection
```javascript
// Hand raise detection
const isRaised = wrist.y < 0.35;

// Flick detection (velocity-based)
const flickVelocity = (hand.lastY || wrist.y) - indexTip.y;
const isFlicking = flickVelocity > 0.06;
```

---

## Configuration (Tuning)

```javascript
const config = {
    flipsPerPlayer: 5,        // 1 for production, 5 for testing
    baseMeterSpeed: 1.2,      // Starting needle speed
    meterSpeedIncrease: 0.15, // Speed increase per player
    sweetSpotChance: 0.95,    // 95% in sweet spot
    greenChance: 0.70,        // 70% in green
    yellowChance: 0.35,       // 35% in yellow
    redChance: 0.10,          // 10% in red
    maxAttempts: 5            // Mercy rule threshold
};
```

---

## Competitive Balance Notes

### Why Weighted RNG?
- Hand tracking isn't precise enough for pure skill
- RNG adds party chaos and comeback potential
- Timing still matters (green vs red = 7x difference)
- Sweet spot rewards precision without being impossible

### Comeback Mechanics
- Mercy rule prevents infinite stuck players
- Speed Demon mode allows rapid catching up
- Clutch feedback rewards perseverance

### Team Balance
- Same rules for both teams
- Position-based team assignment (fair split)
- No inherent advantage to either side

---

## Viral/Share Moments

### The Roast Receipt (Future)
```
╔══════════════════════════════╗
║     FLIP CUP RECEIPT         ║
╠══════════════════════════════╣
║ Flips attempted:     12      ║
║ Flips landed:         8      ║
║ Accuracy:           66%      ║
║ Chokes:              2       ║
║ Times "PATHETIC":    1       ║
║                              ║
║ Rating: CERTIFIED MESS       ║
╚══════════════════════════════╝
```

### Streak Titles (Future)
**Success Streaks:**
- 3x: "Warming Up"
- 5x: "On Fire"
- 7x: "Flip God"
- 10x: "Untouchable"

**Fail Streaks:**
- 3x: "Having a Moment"
- 5x: "Liability"
- 7x: "Team Anchor (Derogatory)"
- 10x: "Designated Driver"

---

## Sound Design (Future)

| Event | Sound |
|-------|-------|
| Hand raised | Confirmation beep |
| Countdown | Arcade countdown |
| Meter active | Tension loop |
| Success | Satisfying thunk + cheer |
| Miss | Wobble + crowd groan |
| Perfect | Extra sparkle SFX |
| Pathetic | Sad trombone |
| Win | Victory fanfare |

---

## Immediate Roadmap

### v1.1 Priority Features
1. Drunk Meter modifier (wobble effect)
2. Screen shake on events
3. Confetti particles on success
4. On-screen drinking rule prompts
5. Callout system (call your shot)

### v1.2 Priority Features
1. Challenge round rotation
2. Blindfold mode
3. Glitch visual effects
4. Power-up system foundation

---

## Long-term Vision

The ultimate party game that:
- Requires zero setup (just open browser)
- Works with any group size
- Has built-in drinking rules
- Creates shareable moments
- Escalates chaos naturally
- Makes everyone look ridiculous

**Core retention hook**: The near-miss drama. Every flip could be PERFECT or PATHETIC - that tension is the game.

---

*Last updated: v1.0 - Vaporwave Edition*
