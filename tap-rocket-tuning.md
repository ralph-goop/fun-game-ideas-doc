# Tap Rocket - Tuning Guide

## Intent
Create a concrete tuning reference so difficulty, fairness, and progression can be shipped and rebalanced with a single source of truth.

## Quick summary
Tune with small, visible levers first: run speed, obstacle spacing, gravity timing, and reward cadence. Keep changes within safe bands and monitor one leading metric per test.

## Baseline tuning envelope
- Forward speed: 5.0 -> 6.8 units/sec over first 8 segments
- Jump impulse: 8.0 base units
- Gravity strength: 12.0 baseline (can be inverted to -1.2x to +1.4x around segment checkpoints)
- Vertical dead zone tolerance: 0.18 to 0.30 of obstacle gap, by tier
- Tap debounce: 90ms minimum gap
- Recovery lane frequency: one guaranteed recovery lane every 6 runs at beginner tiers, then 1 every 9 runs
- First 10 seconds: no fail condition, one successful tap required before failure lock

## Early flow and pattern pacing
### Recommended time bands
- 0-10s: speed 1.0x, no moving hazards, wide static gates only.
- 10-30s: 1.1x speed, low moving hazard count, two safe recovery patterns.
- 30-60s: 1.25x speed, medium moving hazards, first hazard rhythm complexity introduced.
- 60s+: 1.4x+, high-mix patterns, recovery lane spacing increases.

### Pattern bank setup
- Author chunks of 3-8s of play for each difficulty band.
- Tag each chunk with:
  - difficulty tier
  - movement profile
  - safe windows
  - recovery guarantee
- Spawn rules:
  - early segments: fixed-safe chunk order
  - later segments: weighted randomness with replayability bonus
- Every chunk must include a readable reaction cue for first obstacle contact.

## Phase table
### Phase 0: FTUE (runs 1-3)
- Time window: 0-12 minutes average cumulative play
- Target first-obstacle success: 85%+
- Gate spacing: wide, 1.6x standard gap
- Max one moving obstacle per 10 seconds
- No gravity flips in first 2 runs
- On-screen help text active on first death only

### Phase 1: Skill build (runs 4-10)
- Forward speed baseline: 5.2 units/sec
- Obstacle family mix: 55% static, 30% moving, 15% gravity events
- Vertical window tightening from 0.28 -> 0.23
- Introduce hold boost only for users who complete 3 successful runs
- Retry assist trigger: after 3 consecutive late hits

### Phase 2: Persistence (runs 11-24)
- Forward speed target: 5.8 units/sec
- Obstacle family mix: 40% static, 40% moving, 20% gravity events
- Hazard rhythm complexity: introduce paired timing hazards every 3 segments
- One optional risk lane every 2 segments
- Reward density increase only when median run duration stays stable ±8%

### Phase 3: Endurance and loop (runs 25+)
- Forward speed target: 6.5->6.8 by segment 40
- Obstacle family mix: 30% static, 45% moving, 25% gravity events
- Introduce seasonal modifiers and micro-events for route variety
- Add 1 high-risk mini-challenge every 6 segments
- Enable optional co-op ghost benchmark challenge

## Dynamic Difficulty Adjustment (DDA)
- Input for DDA: first three run outcomes (distance, deaths per minute, hazard type failure concentration).
- Adjustment bands:
  - Up-level by 5% if median run distance is >15% above target and survival >85%.
  - Down-level by 5% if death rate is >20% above target over 20 runs.
- DDA limits:
  - Speed change cap: 0.12 units/sec per segment
  - Hazard-density change cap: 0.4 hazards/10s per segment
  - Must retain at least one recovery lane in every 3 tuned chunks.

## Difficulty math
- Difficulty index D = 0.4*speed_norm + 0.35*hazard_density + 0.25*reaction_density
  - speed_norm = (current_speed - 5.0) / 1.8
  - hazard_density = obstacles_per_10s / 8
  - reaction_density = 1 - average_timing_window_fraction
- Keep D changes under 0.08 per segment to avoid spikes.
- No tier boundary jump over +0.12 in one step.

## Spawn bands and guardrails
- Hazard density by segment band:
  - 1-5: 3.4 hazards/10s
  - 6-12: 4.0 hazards/10s
  - 13-25: 4.6 hazards/10s
  - 26+: 4.9 hazards/10s
- Vertical complexity bands:
  - easy: window 0.28, obstacle_height variance low
  - medium: window 0.24, variance medium
  - hard: window 0.20, variance medium-high

## Hitbox and feedback targets
- Player hitbox: 80-85% of sprite bounds.
- Pickup hitbox: 120-140% of sprite bounds.
- Near-miss ratio: track percentage of grazes vs full collisions; increase near-miss density if players do not retry quickly.

## Tuning workflow (weekly)
1. Gather 7-day control metrics: first-obstacle success, median run duration, death reasons, restart conversion.
2. Pick one knob at a time (speed, obstacle spacing, reward multiplier, DDA cap).
3. Run one experiment variant for 5k sessions.
4. Accept if D1+ is flat (within -2 percentage points) and run median improves or stabilizes.
5. Merge only when kill count and failure entropy shift is controlled.

## Onboarding tuning checks
- If first-obstacle success < 85%, increase gap spacing by 8% and reduce gravity intensity by 10%.
- If first obstacle success > 95% after 1,000 sessions, add small risk lane in phase 2 only.
- If quit-at-run-2 > 20%, add one recovery lane in phase 1 and reduce hazard swap frequency.
- If D1 < 35% after 1k DAU, remove one forced speed bump for a full segment release.

## Example config schema
- `segments.json`
  - `segment_index`
  - `forward_speed`
  - `hazard_mix`
  - `gravity_profile`
  - `reward_multiplier`
  - `risk_lanes`
  - `recovery_gate_frequency`
  - `dda_target_success`
  - `dda_state`

## Production implementation notes
- Keep spawn and physics constants in `tunings.json` and `segments.json`.
- Use deterministic seeds per segment for debug replay.
- Add a debug overlay with live values for run speed, upcoming obstacle type, and timing window.
- Add a pattern validation script that rejects any generated sequence with zero safe path.
- Log tuning changes and keep release notes with IDs for each playtestable change.