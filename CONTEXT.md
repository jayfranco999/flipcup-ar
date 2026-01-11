# Flip Cup AR - Session Context

## How to Resume This Project

When starting a new Claude session for this project, paste this at the start:

```
I'm continuing work on Flip Cup AR, a motion-tracking party drinking game.

Key files to read:
- /Users/jayfrancis/claude-test/flipcup-ar/GAME_DESIGN.md (full roadmap)
- /Users/jayfrancis/claude-test/flipcup-ar/index.html (current implementation)

Current state: v1.1 Chaos Edition is live with 4 game modes.

We're brainstorming party game chaos mechanics. The vibe is:
- Vaporwave aesthetics
- Maximum chaos for party settings
- Drinking rules that create memorable moments
- Hand tracking via MediaPipe

[Then describe what you want to work on next]
```

---

## Project Summary

**What is this?**: Browser-based AR Flip Cup game using webcam hand tracking. Players raise hands when ready, then flick up to flip cups with a timing-based shot meter.

**Tech Stack**: Single HTML file, MediaPipe Hands, CSS animations, vanilla JS

**Live URL**: https://jayfranco999.github.io/flipcup-ar/
**Repo**: https://github.com/jayfranco999/flipcup-ar

---

## Current Features (v1.1)

### Game Modes
1. **CLASSIC** - Relay race, 5 flips per player
2. **SUDDEN DEATH** - Miss once = eliminated
3. **SPEED DEMON** - 60 seconds, most flips wins
4. **CHAOS** - All modifiers enabled (the full send mode)

### CHAOS Mode Features
- **Drunk Meter**: Wobbles with increasing amplitude per round
- **Challenge Cards**: Random modifier per player (BLINDFOLD, PRESSURE, SPEED, REVERSE, DRUNK, PRECISION)
- **Shrinking Zones**: Green zone gets smaller each player
- **Drinking Prompts**: On-screen prompts when rules trigger

### Visual Effects
- Screen shake on misses
- Big screen shake + glitch on PATHETIC
- Confetti burst on success
- Feedback text (PERFECT!, CHOKE!, CLUTCH!, etc.)

### Drinking Rules (CHAOS mode)
| Rule | Trigger |
|------|---------|
| CHOKE TAX | Miss in green zone |
| MERCY SHAME | Need all attempts |
| COMBO BREAKER | Break 3+ streak |
| LUCKY BASTARD | Red zone success (other team drinks) |
| PATHETIC TAX | Get PATHETIC feedback |

---

## Design Philosophy

1. **Weighted RNG over pure skill** - Hand tracking isn't precise enough for pure skill, so timing affects success probability (green zone = 70%, red = 10%)

2. **Escalating chaos** - Meter speeds up, zones shrink, wobble increases as game progresses

3. **Drinking rules as spectacle** - Big on-screen prompts make rules visible and fun

4. **Near-miss tension** - Every flip could be PERFECT or PATHETIC - that's the hook

---

## Brainstormed Ideas (Not Yet Implemented)

### High Priority
- **Ghost Needles**: Fake needles appear to confuse
- **Callout System UI**: Let players actually click to call their shot (currently auto-assigns)
- **Sound Effects**: Gunshot, wobble, celebration sounds

### Medium Priority
- **Power-ups/Sabotage**: Freeze opponent's meter, blackout, speed boost
- **Shame Cam**: Zoom webcam on loser after miss
- **Highlight Reel**: Auto-capture best/worst moments

### Future Modes
- **Tournament Mode**: Bracket system for large groups
- **Moving Webcam**: Preview moves around screen (like Duck Hunt challenge mode)

---

## Key Code Patterns

### Hand Detection
```javascript
const isRaised = wrist.y < 0.35;
const flickVelocity = (hand.lastY || wrist.y) - indexTip.y;
const isFlicking = flickVelocity > 0.06;
```

### Meter Wobble
```javascript
wobbleAmount = config.wobbleBase + (roundNumber * config.wobbleIncrease);
// Applied via CSS custom properties
meter.style.setProperty('--wobble-amount', wobbleAmount + 'px');
```

### Zone Success Rates
```javascript
sweetSpotChance: 0.95,  // 47.5-52.5%
greenChance: 0.70,      // 35-65%
yellowChance: 0.35,     // 20-80%
redChance: 0.10         // 0-20%, 80-100%
```

---

## Related Projects

- **Duck Hunt AR**: `/Users/jayfrancis/claude-test/duckhunt-ar/` - Finger gun shooting game with similar hand tracking setup

---

## Session Notes

### What We've Discussed
- Party game design philosophy (low effort, max chaos)
- Drinking rule integration
- Challenge card system for variety
- Vaporwave aesthetic direction
- Future features: callout system, sabotage powers, tournament mode

### Design Decisions Made
- 60 second timer for timed modes
- 3 flips per player in CHAOS (faster rounds)
- Shrink factor of 12% per player
- Wobble increases by 5px per round
- Challenge cards dealt randomly, not chosen

---

*Last updated after v1.1 Chaos Edition release*
