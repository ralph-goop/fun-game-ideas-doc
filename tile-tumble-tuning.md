# Tile Tumble - Tuning Guide

## Intent
Provide concrete knobs and thresholds to ship, monitor, and rebalance match-3 progression without destabilizing retention.

## Quick summary
Start with conservative, additive tuning: board complexity, objective density, booster frequency, and retry policy. Small changes per release reduce churn from sudden difficulty shocks.

## Baseline tuning envelope
- Board size: 8x8 at launch.
- Tile count: 6 base tile variants.
- Move limits:
  - Intro boards: 26-30 moves available.
  - Mid boards: 22-24 moves available.
  - Late boards: 18-21 moves available.
- Blocker baseline: 0 to 1 blocker on first 5 boards, then max 2 by board 10.
- Score multiplier ladder:
  - 3-match: 1.0x
  - 4-match: 1.5x
  - 5-match: 2.0x
  - 4+ chains: x1.1 each additional chain
- Retry allowance by board difficulty band:
  - Easy: 1 free retry
  - Medium: 1 free + 1 rewarded-ad retry
  - Hard: 1 free + 2 optional ad retries

## Quality bands by board number

### Early phase
- Boards 1-10
- Move pressure: low
- Blocker pressure: low
- Objective complexity: single objective
- Target fail rate: 15-22%

### Mid phase
- Boards 11-24
- Move pressure: medium
- Blocker pressure: medium
- Objective complexity: dual objective
- Target fail rate: 22-28%

### Late phase
- Boards 25+
- Move pressure: medium-high
- Blocker pressure: varied by family
- Objective complexity: route + clearance or chain condition
- Target fail rate: 28-35%

## Difficulty index (D)
- D = 0.4 * move_pressure + 0.35 * blocker_pressure + 0.25 * objective_entropy
- move_pressure: 1.0 for <18 moves, 0.5 for 22-24 moves, 0.0 for 26+ moves
- blocker_pressure: 0.0 for 0 blockers, 0.5 for 1 blocker, 1.0 for 2+ blockers
- objective_entropy: 0 for single objective, 0.5 for dual objective, 1.0 for dual + route
- Limit D by band: keep D changes under 0.06 per 5 boards.

## Dynamic difficulty adjustment (DDA)
- Input signal: fail rate, swaps_per_goal, restart frequency, and rage-abort share in last 20 sessions.
- DDA actions:
  - If fail rate > 32% and rage-abort > 12%, reduce blocker_pressure by 1 level for next board.
  - If fail rate < 18% and no rage-aborts, increase move_pressure only (never +1 band at once).
- Safety caps:
  - Objective_entropy cannot increase by more than 1 level per 8 boards.
  - Move pressure changes max +1 tile reduction or +4 board moves per intervention.
  - Blocker changes max 1 blocker per 5 board window.

## Live ops and release rhythm
- Weekly cadence of tuning changes, one major lever at a time.
- Suggested release order:
  1) Retry policy
  2) move limits
  3) objective entropy
  4) seasonal tile effects
- No release should shift more than two tuning knobs at once.

## Monetization tuning
- Rewarded ad frequency starts at conservative pace and scales with D1 retention.
- Default: ad offer in hard fail states only.
- Increase ad conversion targets after 7-day stability review.
- Interstitial policy:
  - no ads before board 4
  - no repeated ads in under 35 seconds
  - no ads during goal explanation flow

## Tuning workflows
1. Pull 7-day window for key KPIs.
2. Choose one control with explicit expected direction.
3. Test against 5k session minimum.
4. Accept only if rage-abort and D1 impact are within guardrails.
5. Add release note with board IDs affected and revert plan.

## Trigger thresholds
- If first board completion falls below 85%, increase move headroom by 10-15% and reduce first 3 blocker spawns.
- If board 10 completion exceeds 95%, introduce one medium blocker at board 12 and reduce move headroom by 2.
- If same-board retries exceed 2.0 per session, add one recovery assist option for that board family.
- If no player can reach mid-phase by day 5, slow DDA and extend early bands by 2 boards.

## Data schema for tuning
- `boards.json` fields:
  - `board_id`
  - `tier`
  - `move_limit`
  - `blocker_count`
  - `objective_type`
  - `objective_parameters`
  - `chain_bonus_enabled`
  - `seasonal_mode`
  - `retry_profile`
  - `dda_group`

## Implementation notes
- Keep all tuning constants external and hot-reloadable in config.
- Validate board generators with automated solver checks and deadlock detection.
- Add a board-level feature flag for seasonal systems and rollback in production.
- Add debug overlay for last N board IDs, active D value, and retry profile.
