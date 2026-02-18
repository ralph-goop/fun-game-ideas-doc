# Tap Rocket - Analytics Plan

## Intent
Track in-game behavior with enough detail to tune difficulty, onboarding, monetization, and retention without guesswork.

## Quick summary
Use event-driven telemetry from session start to run end. Prioritize 3 signal buckets: learnability, skill retention, and frustration. Use weekly reviews to tune only one high-confidence lever at a time.

## Core event model
- session_start
- tutorial_step_complete
- run_start
- tap_input
- obstacle_spawn
- obstacle_hit
- gravity_flip
- reward_pickup
- run_end
- retry_prompt_shown
- retry_action_taken
- rewarded_ad_started
- rewarded_ad_completed
- purchase_started
- purchase_completed
- game_pause

## Required event fields
- `run_id`
- `user_id` (or anonymous hash)
- `run_distance_m`
- `run_seed`
- `platform`
- `app_version`
- `segment_index`
- `input_latency_ms`
- `obstacle_type`
- `collision_position`
- `retry_reason`
- `session_time_seconds`

## Primary KPIs
### Onboarding
- FTUE completion rate (goal 78-85%)
- first_obstacle_clear_time (goal < 30s)
- first_obstacle_success_rate (goal >= 85%)

### Retention
- D1 session completion to 2 runs (goal >= 45%)
- 3rd run completion rate (goal >= 60%)
- median time to first failure (goal 55-90s)

### Skill and loop health
- run_median_duration per segment (no drop > 12% week over week)
- retry_rate_within_60s (target >= 70%)
- run_loss_reason_entropy (avoid single-point death patterns)

### Monetization
- rewarded_ad_opt_in_after_fail (target >= 35%)
- rewarded_ad_to_retry_conversion (target >= 20% of shown offers)
- ad interruption_drop (target < 4% after end-of-run ad)

## Segment dashboards
- Tier-level funnel: start -> first run success -> second-run return -> third-run return.
- Obstacle profile heatmap: deaths by obstacle and segment.
- Friction map: pause, quit, and repeated tap clusters by segment.

## Experiment protocol
1. Start with a control week baseline.
2. Test one knob at a time (speed, obstacle spacing, reward multiplier, gravity window).
3. Use fixed holdouts by geography and install cohort.
4. Run for minimum 5k sessions before declaring pass/fail.
5. Stop tests that reduce D1 by more than 5 percentage points unless retention lift is already above 10 percentage points.

## Alerts and anomalies
- Alert if first_obstacle_success_rate drops by 10 points in 6 hours.
- Alert if retry_rate_within_60s falls below 60% for two consecutive days.
- Alert if crash or timeout spikes after run_start by > 8%.

## Reporting cadence
- Daily: playability and crash checks.
- Weekly: onboarding, monetization, and retention review.
- Milestone reviews: every profile or content patch.

## Data quality guardrails
- Validate missing values in core fields before merge.
- Cap event volume by session and sample for debug builds.
- Exclude known QA sessions from benchmark windows.
