# Tile Tumble - Analytics Specification

## Intent
Define telemetry needed to tune onboarding, match-3 frustration, retention, and ad behavior quickly and safely.

## Quick summary
Tile Tumble analytics should center on board-level difficulty health and retry behavior. Small, stable event schemas beat high-volume noise.

## Core events
- `session_start`
- `ftue_step`
- `board_open`
- `swap_input`
- `match_resolve`
- `board_cascade`
- `booster_used`
- `retry_offered`
- `retry_used`
- `board_completed`
- `board_failed`
- `board_aborted`
- `objective_progress`
- `rewarded_ad_started`
- `rewarded_ad_completed`
- `interstitial_shown`
- `pause`
- `session_end`

## Required fields
- `session_id`
- `user_id` or anonymous id
- `board_id`
- `board_index`
- `attempt_id`
- `swap_count`
- `move_limit`
- `blocker_count`
- `objective_type`
- `outcome`
- `time_to_first_swap_ms`
- `retry_count`
- `cascade_count`
- `chain_max`
- `dda_adjusted`
- `difficulty_index`
- `platform`
- `app_version`
- `schema_version`

### Additional required fields
- `first_3_board_state`
- `reshuffle_count`
- `device_model`
- `network_state`
- `rewarded_ad_type`

## Event meaning
- `ftue_step`: 1 = board intro, 2 = first legal move prompt shown, 3 = first legal move made, 4 = first objective complete, 5 = first fail.
- `swap_input`: include `from_cell`, `to_cell`, `is_legal`, `input_latency_ms`.
- `match_resolve`: include `match_len`, `is_chain`, `match_cells`, `score_delta`.
- `board_cascade`: include `chain_index`, `added_score`, `tiles_added`.
- `board_failed`: include `reason`, `last_action_swap_count`, `remaining_moves`.
- `retry_used`: include `method` (`free`, `rewarded_ad`, `currency`).
- `interstitial_shown`: include `placement`, `cooldown_state`, `time_since_session_start_ms`.

## KPI sheet

### Learnability
- First legal swap latency: target <= 8s.
- First board first-attempt completion: target >= 85%.
- First 3 board completion share: should remain within planned band.
- FTUE first-tap target: under 8s.

### Engagement and retention
- D1 session_completion_2boards: target >= 40%.
- D3 return_rate: target 25%+.
- Median boards per session by cohort.
- Median swaps per completed board.
- D1 retention target: 40%.
- D7 retention target: 15%.
- D30 retention target: 5%.

### Frustration and flow
- Rage-abort rate: board-level and global.
- Retry rate by board and board family.
- Retry depth: retries per fail and second retry conversion.
- Move exhaustion events: track at early and late tiers.

### Technical stability
- ANR rate target: < 0.47%.
- Crash rate target: < 1.09%.
- Device-tier anomaly: any device model above 8% issue rate requires segment-level action.

### Monetization health
- rewarded_ad_offer_count and completion rate.
- rewarded_ad_revenue_per_1k_sessions.
- interstitial_compliance: respect cooldown and board index constraints.
- ad_to_retry conversion by board tier.
- Rewarded-ad opt-in target: >= 40% where offered.
- Interstitial policy watch: 1-3 per session max, 3-5 minute spacing.

### Gameplay quality
- Match success by board objective.
- Chain count distribution by level.
- Board fail reason distribution and entropy.
- Difficulty index trend over board index and session age.

## Alerts
- Immediate:
  - session volume drop > 15%
  - onboarding completion < 75%
  - rage-abort spike > 12%
- Immediate technical: ANR > 0.47% or crash > 1.09%.
- Retention:
  - D1 to D3 dip of 5 points in 24h
  - board 1 completion below 85% for 2 consecutive monitoring windows
- Monetization:
  - rewarded ad completion below 30% when offered at constant frequency
  - interstitial frequency above threshold before board 4
  - interstitial spacing under 3 minutes or more than 3 per session

## Dashboards
1. Onboarding funnel: session_start -> ftue_step3 -> first board complete.
2. Board heatmap: board_id, fail reason, and retry path.
3. Friction ladder: difficulty_index vs completion trend and rage-abort trend.
4. Monetization view: rewarded and interstitial placement conversion.
5. Live tuning watchlist: board-level KPIs with confidence intervals.
6. Technical dashboard: ANR/crash by device model and OS.

## Reporting cadence
- Daily: data completeness checks + fail reason distribution.
- Weekly: onboarding metrics, D1/D3, rage-abort, and monetization lift.
- Release: all guardrail KPIs must be green before full rollout.

## Data hygiene and rollout
- Use strict schema versions for all analytics events.
- Keep enums stable for event names, fail reasons, and board types.
- Validate schema on server before storing high-volume analytics.
- Flag malformed payloads for immediate triage.
- Keep all board IDs and game version ids in every session-scoped event.
- Track `reshuffle_reason` and `rewarded_ad_type` as controlled enums.
