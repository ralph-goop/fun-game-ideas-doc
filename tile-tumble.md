# Tile Tumble - Detailed Research & Build Notes

## Intent
Single document for Game 1 from the original ideas list: a simple, implementation-friendly match-3 tile game with a stronger design backbone and development plan.

## Quick summary
Tile Tumble keeps the classic easy-to-learn swap-and-match loop while adding modern match-3 hooks that improve retention: low-friction onboarding, meaningful rewards, predictable patterns, and optional progression systems with clear pacing.

## Source-grounded observations
Research and best-practice reviews around tile puzzle/casual game design suggest these principles:
- Manual level design beats fully procedural generation for match-3 because it better controls player difficulty, pacing, and emotion.
- Casual titles succeed with very small core verbs (tap, drag, swap) and one-handed mobile accessibility.
- Early onboarding and a clear, low-risk entry loop are critical, then mechanics should be added gradually.
- Match-3 systems should feel rewarding even on imperfect play; low penalty makes sessions feel less stressful.
- Reward pulses, cascades, and visual clarity are core engagement drivers.
- Live-ops content cadence and tuning by actual attempts/time data improve retention more than static level plans.

Sources used:
- https://www.gamedeveloper.com/design/smart-casual-the-state-of-tile-puzzle-games-level-design-part-1
- https://medium.com/design-bootcamp/design-analysis-of-match-3-games-fb63879ecd8f
- https://gamedesignskills.com/game-design/casual/

## Refined game profile
- Name: Tile Tumble
- Genre: Casual puzzle (match-3)
- Core action: swap adjacent tiles to make matches of three+ and trigger cascades.
- Primary appeal: instant recognition-based play with short, replayable rounds.
- Device/UX: portrait phone/tablet, one-hand friendly.

## Core loop (v2)
1. Player sees a board with a clear objective (score, target score, or clear blockers).
2. Player makes one legal swap.
3. Match resolves, tiles disappear, gravity settles the board, and cascades happen.
4. Player receives immediate micro-reward feedback (score, particle burst, sound, small text pop).
5. Board may add a small obstacle, timer pressure, or move limit depending on level type.
6. When win/lose condition hits, next board is offered with a short summary and clear incentive to continue.

## Design targets for build quality
- Session target: 90-120 seconds for an easy board, 180 seconds for first challenge board.
- Failure friction: avoid hard stops; use move boosts, short retries, and one mild penalty mechanic.
- Learnability: reveal the full move vocabulary by level 4 at latest.
- Visual contrast: tile shapes and colors must be instantly distinguishable.
- Retention rhythm: 3-day, 7-day, 14-day micro-goals visible via account map.

## Board rules and mechanics
### Suggested base board
- 8x8 grid at launch
- 6 tile variants
- Standard match: 3, 4, 5 rows/columns
- Match score multipliers by combo or chain length

### Win/lose condition variants
- Score target within move limit (easy)
- Clear fixed blockers/objectives (medium)
- Reach a “path tile” in under N moves (advanced)

### Difficulty shaping
- Easy start: no blockers, generous board, no move pressure first 3 boards.
- Middle start: limited boosters, one blocker per board, moderate target.
- Late start: multi-objective boards (score + clear + route), still avoid no-move deaths too often.
- Track attempts per board to tune; avoid binary pass-rate obsession.

## Fun mechanics (research-grounded)
- Pattern recognition satisfaction: clear color/shape grammar and predictable matching rules.
- Completion drive: players like “fixing” board disorder through short goals.
- Momentum rewards: cascades and board-clearing chain events increase perceived control and fun.
- Loss aversion control: hearts/move replenishment and retries are safer than repeated hard blocks.

## Additional idea pack for Tile Tumble
### 1) Seasonal Tiles (key twist)
Introduce board-wide seasonal tile sets every few levels:
- Frost season: tiles freeze neighbors after each match unless player uses a thaw booster.
- Lava season: blocked areas become fragile, collapsing after two adjacent clears.
- Crystal season: 2x2 area match creates one clear burst instead of one line clear.

Why it stays memorable: same core system, new puzzle language every few levels.

### 2) Clutter Quota mode
Every board has a “clutter meter” shown on HUD. Extra points are gained for removing clutter tiles quickly with chains. This adds urgency without forcing strict timers.

### 3) Neighborhood goals
Chain goals across 3 boards (ex: light 3 lantern tiles total) to encourage planning over one-off board solving.

### 4) Soft daily event board
Daily board with one rotating rule modifier and a guaranteed 4-match in first 10 moves.

### 5) Board journal
Keep a simple “found combos” log (e.g., first 4-match, first 5-match, 10-chain board). Works as low-stakes social bragging.

## Economy and retention ideas
- Soft currency: Spark (common) for retries and small boosts.
- Premium currency: Ember (premium) for one-time board hints, daily reset, cosmetic themes.
- Gentle ads: optional rewarded ad for one free retry or one board hint.
- Progression gate: star constellations unlock board packs, never locked behind too many long waits.

## Content and level plan
- Phase 1 MVP (5-7 days): board engine, base matching rules, score+move loops, 60 boards, one seasonal twist, basic UI.
- Phase 2 (following week): 40-60 more boards, daily event, board journal, retry economy.
- Phase 3: weekly live-ops cycle with 2 puzzle variants and 1 new seasonal tile rule.

## Testing checkpoints (before release)
- FTUE test: can a new player explain objective and make first legal move in <10 seconds?
- First board completion rate: 85% target first-time users for first 3 boards.
- Retry abuse check: no board should force more than 12% aborts by frustrated users.
- Retention proxy: level 10 completion and median moves/board after 24h should stay near target.

## Production notes from research
- Build level data-driven from day one (JSON board states, constraints, goals).
- Keep game over as a soft state: reduce penalties before introducing strict failure.
- Add telemetry early: board id, attempted swaps, cascades, boosts used, abandon points.
- Validate any new tile mechanic with two A/B variants before broad roll-out.

## Open questions for next phase
1) Which core fantasy should we choose: clean modern or fantasy-forest style?
2) Do we want a competitive ladder or just a campaign map first?
3) Should ads be optional early (default off for premium-lite flow) or always present for free users?

## Suggested next doc updates
- Add a separate `tile-tumble-board-spec.md` for exact tile data format.
- Add `tile-tumble-analytics.md` with KPI definitions and dashboards.
- Add `tile-tumble-monetization.md` with economy guardrails and economy balancing notes.