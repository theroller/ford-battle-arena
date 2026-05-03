# Ford vs Finn: Battle Arena — Game Design Reference

Single `index.html`, no build step, no dependencies. Deployable to GitHub Pages.
Safari on iPad → Add to Home Screen.

---

## Core Loop

1. Both players pick a weapon and allocate stat points
2. 3-2-1-FIGHT! countdown blocks input until it completes
3. Each player taps their half of the screen to attack the other
4. First player to 0 HP loses the round
5. Best-of-5 rounds wins the match

---

## Combat Mechanics

### Hit Types
| Tap | Result |
|---|---|
| Anywhere on your zone (off-spot) | Weak chip hit: 1–3 dmg, no crits |
| Power spot (gold pulse) | Full hit: weapon damage + crits |
| Hold until bar fills | Special: high damage / extra effects |

### Power Spot
- Always visible on each player's side — a pulsing gold circle
- Drifts to a new position every 5–9 seconds automatically
- Also relocates after each hit
- Stays away from the center divider to avoid confusion

### Hold-to-Charge (Special)
- Touch and hold your zone; a charge bar fills at the bottom
- Release when full → special attack fires
- Charge speed controlled by the Charge stat

---

## Weapons (v1.9 — balanced)

| Weapon | Tap dmg | Spec dmg | Special ability | Notes |
|---|---|---|---|---|
| ⚔️ Sword | 3–7 | 12–20 | — | +5% crit bonus; steady reliable damage |
| 🏹 Poison Arrow | 1–3 | 2–4 + DoT | Poison DoT | Tap is weak; special poisons for 7 ticks × 3 dmg |
| 🪓 Axe | 2–8 | 14–24 | Brutal crits | −8% crit chance but crit mult +1.0 bonus; boom or bust |
| 🪄 Magic Wand | 2–7 | 2–8 × 3 bolts | 3-bolt special | Normal tap = 1 bolt; hold = 3 staggered bolts (180ms apart) |

**Normal tap** always fires 1 bolt (even wand).  
**Special** fires `bolts` count (wand: 3, others: 1).  
Arrow special directly applies `specPoisonTicks:7, specPoisonDmg:3`.

---

## Character Stats

5 points total per player; default allocation is 1/1/1 (leaves 2 free).  
Persists between rounds within a match; resets on new match.

| Stat | Level 0 | Level 1 | Level 2 | Level 3 | Level 4 |
|---|---|---|---|---|---|
| ⚡ Crit % | 4% | 10% | 16% | 22% | 30% |
| 💥 Power (crit mult) | ×1.2 | ×1.4 | ×1.7 | ×2.0 | ×2.4 |
| 🔋 Charge speed | 1700ms | 1450ms | 1200ms | 1000ms | 800ms |

Build titles shown on weapon screen:
- THE LUCKY ONE — crit ≥ 3
- THE CRUSHER — power ≥ 3
- THE SPEEDSTER — charge ≥ 3
- THE WARRIOR — balanced

---

## Crit Math

```
chance = clamp(0.02, 0.60, playerCritChance + weapon.critBonus)
mult   = playerPowerMult + weapon.critMultBonus  (only on crit)
damage = baseDmg × (isCrit ? mult : 1)
```

---

## Constants

```js
MAX_HP        = 150
ROUNDS_TO_WIN = 3
TOTAL_PTS     = 5
MAX_LEVEL     = 4   // per stat
SPOT_DRIFT    = 5000–9000ms
BOLT_DELAY    = 180ms between wand bolts
```

---

## Expected Damage Per Spot Tap (default 1/1/1 stats)

| Weapon | E[tap dmg] | E[special] |
|---|---|---|
| Sword | ~5.2 | ~16 |
| Axe | ~5.2 | ~19 |
| Wand | ~4.6 | ~15 (3 bolts) |
| Arrow | ~2.1 | ~3 direct + 21 poison |

---

## Planned Features (Roadmap)

| Week | Feature | Concept it sneaks in |
|---|---|---|
| ✅ 1 | Basic tap battle, health bars, win screen | Variables, conditionals, game loop |
| ✅ 2 | Weapon picker (sword, arrow, axe) | Parameters, trade-offs |
| ✅ 3 | Special move — hold to charge | Timing, risk/reward |
| ✅ 4 | Character stat allocation | Trade-offs, build optimization |
| ✅ 5 | Magic wand + power spots | New mechanics, skill-based targeting |
| 6 | Arena picker — Desert, Ice, Space, Lava | Theming, random events |
| 7 | New characters (Mom, Dad) with different stats | Data objects |
| 8 | Battle log — "Ford hit Finn for 7!" scrolls | String formatting, event history |
| 9 | Ford draws the characters — photos → sprites | Personalization |
| 10 | Score tracker across sessions | localStorage persistence |
