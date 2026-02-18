# Tap Rocket - Analytics Specification

## Intent
Track all run-relevant telemetry needed to tune onboarding, difficulty, retention, and monetization quickly.

## Quick summary
Event quality and consistency matter more than volume. Capture stable identifiers and timing so each release can be evaluated with one hypothesis.

## Core events
- `session_start`
- `ftue_step`
- `run_start`
- `tap_input`
- `obstacle_spawn`
- `obstacle_pass`
- `obstacle_hit`
- `gravity_flip`
- `dda_adjustment`
- `recovery_lane_used`
- `retry_prompt_shown`
- `retry_action_taken`
- `mission_started`
- `mission_completed`
- `rewarded_ad_started`
- `rewarded_ad_completed`
- `interstitial_shown`
- `run_end`
- `game_pause`
- `session_end`

## Required fields
- `run_id`
- `session_id`
- `attempt_id`
- `user_id` (or anonymous ID)
- `segment_index`
- `run_distance_m`
- `run_duration_ms`
- `obstacle_type`
- `failure_reason`
- `tap_count`
- `time_since_last_tap_ms`
- `input_latency_ms`
- `run_state`
- `dda_state`
- `app_version`
- `platform`
- `schema_version`
- `currency_delta`
- `mission_id`

## Event taxonomy and meaning
- `ftue_step`: 1 = intro, 2 = first obstacle appears, 3 = first pass, 4 = first fail, 5 = first successful recovery
- `tap_input`: include x,y normalized screen position and response latency
- `obstacle_pass`: include lane id and clearance margin
- `dda_adjustment`: include `from` and `to` values for difficulty index, plus trigger reason
- `run_end`: include score, best_run_delta, run_state, and retry_path
- `retry_action_taken`: values include `immediate`, `revive_ad`, `session_restart`
- `interstitial_shown`: include `placement`, `source`, `cooldown_bucket`
- `mission_completed`: include mission id, mission progress, and reward type

## KPI sheet
### Learnability
- First-clarity check: tap understanding by 7 seconds (`ftue_time_to_first_tap` <= 7)
- FTUE completion rate (target 78-85%)
- First 10-second safe completion rate (target >=90%)
- first_obstacle_success_rate (>= 85%)
- first_obstacle_time_to_clear (<= 30s)

### Engagement and retention
- restart_rate_within_60s
- D1 session completion to 2 runs (>= 45%)
- D1 completion target (>= 35-40%)
- median_run_count_24h
- D7 return and session_count_before_revenue_intro

### Fun and frustration
- median_run_duration by segment
- death_reason_entropy (too concentrated reasons can hurt retention)
- difficulty_shock (run drop at segment transitions)
- median near-miss count (higher near misses can indicate healthy frustration)

### Monetization
- ad_offer_impression
- rewarded_ad_completion_rate (target strong completion where offered at fail point)
- interstitial_cooldown_compliance
- ad_offer_interruption_drop
- reward_value_attribution to next run retention

## Alerts
- Immediate: any event volume drop > 15%
- Onboarding: first_obstacle_success < 85% for 6 hours
- Retention: run_median_24h down 10% week over week
- Monetization: rewarded_ad_completion < 30% while impressions stable
- Ads: interstitial shown before 3 runs in session 1

## Dashboards
1. Onboarding funnel: session_start -> first_tap -> first_pass -> first_run_complete.
2. Obstacle heatmap: failures per obstacle class, lane, and segment.
3. Retention ladder: runs completed, restart timing, ad conversions.
4. Economy panel: currency spend by context and segment.
5. DDA monitor: difficulty index, adjustment direction, and recovery behavior.

## Reporting cadence
- Daily: ingestion checks and top failure reasons.
- Weekly: D1, D7, monetization lift, and major tuning recommendations.
- Biweekly: tuning recommendations by segment.
- Every release: pre-launch signoff check against all guardrail KPIs.

## Data contracts for developers
- Use stable enums for all event names and failure reasons.
- Keep numeric ranges clamped (avoid negative times, impossible values).
- Version all analytics payload schema with `schema_version` for backward compatibility.
- Track ad placement cooldown and suppress duplicate ad requests until cooldown window closes.
