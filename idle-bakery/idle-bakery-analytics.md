# Idle Bakery - Analytics Specification

## Intent
Define stable telemetry and KPI logic for launch-ready bakery iteration, especially onboarding, offline behavior, progression health, and ad fatigue.

## Quick summary
The analytics strategy should optimize two loops: active tap loop and idle loop. Every major event must include `schema_version`, `game_state`, and identifiers to support reproducible tuning decisions.

## Core events
- `session_start`
- `session_end`
- `bakery_open`
- `collect_click`
- `collect_auto`
- `upgrade_open`
- `upgrade_confirm`
- `recipe_unlocked`
- `prestige_open`
- `prestige_confirm`
- `ad_offer_shown`
- `ad_offer_completed`
- `ad_offer_dismissed`
- `interstitial_shown`
- `interstitial_dismissed`
- `offline_gain_calculated`
- `shop_open`
- `settings_open`
- `reward_claimed`
- `error_sync`

## Required fields
- `session_id`
- `user_id` (or anonymous id)
- `app_version`
- `platform`
- `schema_version`
- `device_model`
- `network_type`
- `event_time_ms`
- `currency_before`
- `currency_after`
- `game_state`
- `production_per_minute`
- `current_level`
- `total_baked`
- `upgrade_level`
- `recipe_id`
- `recipe_level`
- `prestige_tier`
- `offline_minutes`
- `ad_type`
- `offer_id`
- `offer_context`

### Event-specific payloads
- `upgrade_confirm`: include `station_id`, `from_level`, `to_level`, `cost`, `income_delta`.
- `prestige_confirm`: include `prestige_gain`, `next_tier_cost`, `time_to_next_tier`.
- `collect_auto`: include `collection_mode` (`offline`, `timer`, `manual`), `amount`, `overflow_cleared`.
- `ad_offer_shown`: include `placement`, `cooldown_state`, `next_eligible_in_ms`.
- `interstitial_shown`: include `placement`, `cooldown_state`, `source`.
- `interstitial_dismissed`: include `placement`, `dismiss_reason`, `time_in_focus_ms`.
- `offline_gain_calculated`: include `offline_minutes`, `cap_reason`, `multiplier_applied`, `overflow`.

## KPI sheet

### Learnability
- First collect time median <= 10s target.
- First upgrade completion within 90s for 90% of fresh installs.
- First meaningful action (collect or upgrade) rate target: 95%.

### Engagement and retention
- D1 active action rate: 50%+
- D3 return with meaningful action: 18%+
- D7 return with meaningful action: 15-20%
- D30 return: 6%+
- Session median length target: 90-180s first day.
- Return gap median: 2.0-8.0h.

### Idle and progression quality
- Offline collect completion rate: >= 85% after at least one return.
- Offline overflow ratio target: <= 3% first 7 days, <= 5% after day 7.
- Upgrade completion by first upgrade attempt: >= 90%.
- Prestige conversion rate (cohort-adjusted): 8-15% by day 7 for retention cohorts.

### Monetization and fatigue
- Rewarded ad offer conversion when shown: >= 35%.
- Ad interrupt drop-off at open: <= 5%.
- Interstitial shown before first collect: 0%.
- Interstitials before first session end for D1: <= 5%.
- Rewarded ad cooldown compliance >= 98%.

### Technical quality
- ANR rate < 0.5%
- Crash rate < 1.1%
- Background resume failure < 0.15%
- Sync failure ratio < 1.0%

## Dashboards
1. FTUE funnel: `session_start` -> first collect -> first upgrade -> first offline return.
2. Revenue/pacing heatmap: upgrade cost band, completion, and prestige stage.
3. Offline health: offline minutes, overflow rate, and offline claim amount by segment.
4. Churn ladder: meaningful action gap by session age.
5. Ad behavior: reward offers by placement, cooldown breaches, and opt-in churn.
6. Technical stack: ANR/crash by device tier and OS.

## Alert rules
- Immediate:
  - Session volume down > 15% over 1 hour.
  - onboarding first collect < 75%.
- Product:
  - Offline overflow > 5% for 2 consecutive monitoring windows.
  - D1 active-action below 45% for 12h.
  - First upgrade conversion below 70% in first 24h release cohort.
- Monetization:
  - rewarded ad conversion below 25% with stable offer volume.
  - ad cooldown violation > 2%.
- Technical:
  - ANR > 0.5% or crash > 1.1%.
  - background resume errors > 0.15%.

## Reporting cadence
- Daily:
  - funnel status, event completeness, crash/ANR, sync errors.
- Weekly:
  - retention ladders, offline claim quality, ad compliance.
- Release:
  - All technical and product guardrails green before broader rollout.

## Experiment support
- Add `experiment_bucket`, `cohort_id`, and `treatment_group` to each config-tuning event.
- For each experiment keep a paired control and at least one objective per test:
  - onboarding completion,
  - D1,
  - upgrade conversion,
  - reward fatigue.
- Store experiment end state with start time, audience, and release notes.

## Data hygiene
- Enforce enum validation for:
  - `event_type`
  - `offer_context`
  - `collection_mode`
  - `error_code`
- Use schema checks at ingest and reject malformed payloads with error tags.
- Keep numeric clamps for production and currency values to prevent overflow.
- Keep all event payloads versioned and backward-compatible.
