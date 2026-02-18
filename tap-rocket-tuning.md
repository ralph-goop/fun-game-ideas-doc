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

## Difficulty math
- Difficulty index D = 0.4*speed_norm + 0.35*hazard_density + 0.25*reaction_density
  - speed_norm = (current_speed - 5.0) / 1.8
  - hazard_density = obstacles_per_10s / 8
  - reaction_density = 1 - average_timing_window_fraction
- Keep D changes under 0.08 per segment to avoid spikes.

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

## Tuning workflow (weekly)
1. Gather 7-day control metrics: first-obstacle success, median run duration, death reasons, restart conversion.
2. Pick one lane parameter: speed OR hazard density OR reward multiplier.
3. Run one experiment variant for 5k sessions.
4. Accept if D1+ is flat (within -2 percentage points) and run median improves or stabilizes.
5. Merge only when kill count and failure entropy shift is controlled.

## Onboarding tuning checks
- If first-obstacle success < 85%, increase gap spacing by 8% and reduce gravity intensity by 10%.
- If first obstacle success > 95% after 1,000 sessions, add small risk lane in phase 2 only.
- If quit-at-run-2 > 20%, add one recovery lane in phase 1 and reduce hazard swap frequency.

## Example config schema
- `segments.json`
  - `segment_index`
  - `forward_speed`
  - `hazard_mix`
  - `gravity_profile`
  - `reward_multiplier`
  - `risk_lanes`
  - `recovery_gate_frequency`

## Production implementation notes
- Keep spawn and physics constants in `tunings.json` and `segments.json`.
- Use deterministic seeds per segment for debug replay.
- Add a debug overlay with live values for run speed, upcoming obstacle type, and timing window.
