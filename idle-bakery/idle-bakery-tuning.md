# Idle Bakery - Tuning Guide

## Intent
Provide concrete math, thresholds, and release procedures to keep production growth measurable and predictable.

## Quick summary
Tune Idle Bakery with four stable knobs only: output growth, storage, recipe quality, and offline cap. Every release should adjust one knob on one cohort only, evaluate for 5k sessions, then move on.

## Baseline tuning envelope
- Initial ovens: 1 oven, base production `1.0` Bun/min.
- Initial recipe multiplier: 1.0x.
- Base storage cap: 150 Bun.
- Base offline cap: 60 minutes.
- Offline reward multiplier: 1.0x base, 1.15x with premium multiplier bonus.
- First upgrade cost: 40 Bun.
- Base upgrade increase:
  - Oven level +25% production per level.
  - Recipe level +30% output multiplier per level.
  - Storage upgrade +60% cap per relevant upgrade.
- Prestige conversion gain: 3% - 12% permanent multiplier, locked by prestige tier.

## Core progression formulas

### Oven production
- `oven_output = base_output * (1 + 0.25 * (level - 1)) * recipe_multiplier * global_multiplier`
- `global_multiplier = 1 + global_boost + ad_boost + event_boost`

### Recipe effect
- `recipe_gain = 1.0 * (1 + 0.30 * (recipe_level - 1))`
- Hard cap on any recipe multiplier at 8.0x to avoid runaway loops.

### Storage and waste
- `effective_storage = base_storage * (1 + storage_bonus)`
- If generated output exceeds storage, clip to storage with `overflow_count` increments.
- Overflow ratio should remain below 3% of total generated income in first 3 sessions.

### Prestige formula
- `prestige_currency = floor(7 + 3 * log2(1 + total_produced / 4000))`
- `global_boost_from_prestige = 1 + prestige_currency * 0.04`
- Prestige soft lock: max +25% at low tiers; unlock more range as tiers progress.

## Phase table

### Phase 0: Launch day 0-2
- Target time-to-first-upgrade: 60-90 seconds.
- Session depth target: one upgrade + one manual collect by minute 2.
- Offline window: 30-60 minutes.
- Unlocks: oven upgrades, recipe 2, storage 1.

### Phase 1: Week 1 growth
- Enable milestone rewards every 12 hours.
- Increase storage and event modifiers first, production second.
- Prestige open at cumulative progress milestone only if players can complete two upgrades and one prestige-prep in same week.
- Reduce friction by one-click collect animation for 90-second offline windows.

### Phase 2: Week 2-4 retention
- Add advanced oven branch and one prestige-lens card.
- Add optional risk-reward event modifiers (timed +10% to +35%).
- Offline cap extends to 90-120 minutes for active returning cohorts only.
- Add one monthly event recipe with limited-time coefficient.

## Dynamic tuning playbook
- DDA inputs:
  - first-hour upgrade conversion
  - upgrade abandon rate by station
  - offline collect frequency
  - average session gap
- DDA rules:
  - If upgrade conversion < 18% in first 24h, lower cost of first 2 oven tiers by 8-12%.
  - If offline claim latency > 2 minutes before interaction, add clarity labels and one-tap claim prompts.
  - If offline cap overflow > 6%, increase storage +15% on next 2-day wave for affected cohorts.
- Safety caps:
  - Never increase any multiplier by more than 10% in one release.
  - Never adjust prestige formula and output formula in the same patch.
  - Keep cost progression slope non-decreasing by at most 15% per version.

## Difficulty and retention balance
- Gentle progression band:
  - Early: low slope, high short-term payoff.
  - Mid: slope increase with optional prestige target.
  - Late: higher cost, higher payoff, stronger long-run goals.
- Friction checks:
  - If `median session gap` > 18h for day 1 cohorts, introduce one soft return reward.
  - If collect frequency under 1.3 per day, review offline presentation.

## Tuning workflow
1. Pull 7-day cohort metrics by version.
2. Select one lever and one audience segment.
3. Run one controlled experiment with min 5k sessions.
4. Approve only if:
   - retention and crash remain in green,
   - onboarding KPIs stable,
   - and one targeted KPI improves by >= 3%.
5. Tag release with impacted `station_ids` and a revert key.

## Release playbook
- Weekly release windows for minor balancing: one new config patch per week.
- Pre-release check:
  - Formula integrity tests
  - Offline cap clamp tests
  - Prestige recovery path checks
- Rollout:
  - 10% staged -> 50% -> 100% over 24h if guardrails hold.
- Auto-rollback when:
  - onboarding or collect-funnel drops more than 8 points in 6h,
  - ANR > 0.5% or crash > 1.1%.

## Data schema for tuning
- `bakery_levels.json` fields:
  - `station_id`
  - `level`
  - `base_output`
  - `upgrade_cost`
  - `storage_bonus`
  - `recipe_id`
  - `prestige_tier`
  - `unlock_min_level`
  - `cost_scale`
  - `max_output_cap`

## Target thresholds and alerts
- First-upgrade conversion below 20% in first day: lower first-2 upgrade costs by 10%.
- Offline overflow > 5%: add storage bump for affected cohorts.
- Prestige drop-off > 15% week-on-week: reduce prerequisite requirement by one milestone.
- Upgrade skip loop > 4 consecutive failures by same player: surface assisted upgrade pack in menu.

## Implementation notes
- Keep tuning constants in external JSON and feature-flagged per release.
- Use deterministic offline reward simulation to support replay and audits.
- Store all formulas with clear comment blocks and version tags.
- Include migration notes for any schema-breaking tuning changes.
