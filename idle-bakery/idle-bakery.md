# Idle Bakery - Detailed Research & Build Notes

## Intent
Production notes for Game 3 from the idea register: an idle bakery where players automate baking, expand capacity, and unlock prestige layers without blocking short-session play.

## Quick summary
Idle Bakery should feel immediate at launch (no setup tax) while rewarding deeper systems over days. Keep every progression layer additive: faster production, stronger recipes, and reliable offline gains, with no mandatory spending gates in the first two sessions.

## Source-grounded observations
From game-loop references and ad-supported idle-market benchmarks:
- Idle retention improves with clear long-tail loops plus short-term agency: manual claims, one meaningful upgrade, and a recurring event reward.
- Early retention is strongest when first value is delivered within 10-15 seconds and can be acted on in the first minute.
- Offline progress should be capped to avoid power-currency inflation and protect long-term balancing.
- Rewarded ads work best when tied to optional speed/efficiency boosts, not mandatory progress skips.
- Prestige layers should be transparent with visible long-term gain and short startup reset pain.
- Ad fatigue in idle loops appears when ads are shown too often during passive periods.
- Instrumented iteration windows should focus on `first_claim`, `upgrade_path`, and `days_since_install` rather than raw session length only.

Sources used:
- https://www.gamedeveloper.com/design/what-you-can-do-with-an-indie-game-loop
- https://developers.google.com/games/services/analytics
- https://www.reddit.com/r/gamedev/wiki/cash-cow
- https://www.gameanalytics.com/blog/hyper-casual-game-common-mistakes

## Refined game profile
- Name: Idle Bakery
- Genre: Idle / Incremental
- Core action: start ovens, collect pastries/coins, upgrade recipes and throughput.
- Primary appeal: visible growth and automation feel with low-friction taps.
- Device/UX: portrait-first mobile, light interactions and status-first HUD.
- Economy style: soft currency for upgrades, premium currency for convenience.

## Core loop (v3)
1. Player opens bakery and sees production output over time.
2. Player taps `Collect` to convert generated goods into currency immediately.
3. Player spends currency to place or upgrade ovens and unlock recipes.
4. Production rate increases, raising both instant income and offline yield.
5. Prestige path occasionally opens when bakery reaches a threshold.
6. Player optionally watches rewarded ads for temporary boosts.
7. Session ends in a few planned actions, with clear next objective on return.

## First 3 onboarding milestones

### Stage 0: Start (first 10 seconds)
- Start with 1 Oven, 1 recipe, and one-tap `Collect`.
- Auto-highlight collect button and primary upgrade card on launch.
- No forced ad or popup before first collect.
- Target outcome: collect event in first 10 seconds for 98% of new users.

### Stage 1: First upgrade (first 60 seconds)
- Introduce one cheap oven upgrade path and one recipe slot.
- Show progress line: "2x income from next upgrade" to make impact obvious.
- Offer one free manual collect every 15 seconds during first minute.
- Target outcome: 95% of users complete one meaningful upgrade within 90 seconds.

### Stage 2: First expansion (minute 2-4)
- Introduce second production lane and offline gain label.
- Unlock one optional convenience action: `Turbo Collect` with long-press help tooltip.
- Target outcome: first repeat visit within 24h by session-end action prompt.

## Build and gameplay targets
- FTUE: first collect action within 10 seconds.
- First meaningful upgrade: 60-90 seconds for 90%+ new players in seeded testing.
- Session target: 90-180 seconds for first session.
- Mid-session return: 55-65% of first-day players return by hour 12.
- Upgrade conversion: at least 20% of users do one upgrade in first session.
- D1 target: 50%+ players return with meaningful action, including offline collect.
- D7 target: 15-20%.
- D30 target: 6%.
- Offline trust: no more than 3h of uncollected reward at any one-time claim moment.

## Economy design
- Primary currency: `Bun` for all base upgrades and claims.
- Secondary currency: `Butter` for premium cosmetic and skip/comfort options.
- Core economy levers:
  - Production speed of ovens
  - Recipe multiplier
  - Storage capacity
  - Offline multiplier
- No hard gate on core gameplay progression in first week: all base content available.

## Retention and game feel systems
- Daily cadence: one rotating bakery theme with one recipe bonus and one temporary boost.
- Prestige hook: convert accumulated bakery reputation into a reset reward (`Star Loaf` tokens).
- Prestige should grant 5-15% permanent multiplier and keep a visible next threshold.
- Reward pacing should be visible in two layers:
  - Near term: next minute expected income
  - Long term: time to next major expansion
- Keep loops short: at least one actionable task each 30-45 seconds for active users.

## Monetization guardrails (at launch)
- Rewarded ads: only for optional boosts (double collect window, +20% speed for 8 minutes, storage expansion once/day).
- Interstitials: no interstitial before first manual collect and no more than 1 in the first 25 minutes of total session time.
- Rewarded offer cooldown: minimum 3 minutes between identical offers.
- Ad frequency cap: max 2 rewarded + 1 interstitial per hour segment for users under D7.
- Never force monetization on first visit or during first two sessions.

## Technical quality gates
- ANR rate target < 0.5%
- Crash rate target < 1.1%
- Background resume failure under 0.15% on iOS/Android
- Any device segment >8% issue rate should pause rollout.
- Offline logic integrity checks every release (stale timestamps, negative offline gains, integer overflow).

## Testing checkpoints before release
- FTUE test: first collect latency median <= 10 seconds.
- First upgrade success: >= 90% within 90 seconds.
- Upgrade and ad funnel: no forced path before upgrade #1.
- Economy sanity: max daily spend/reward loops should never trigger negative balances.
- Data sanity: all key events include schema version and gameplay IDs.

## Production notes from research
- Keep station and recipe configs in server-driven tables for live balancing.
- Simulate 30-day progression from day-one test cohorts before touching prestige multipliers.
- Use event-gated rollouts for any multiplier increase >10%.
- Build a 48-hour rollback plan for production config changes touching output formula.
- Ensure offline reward logic is deterministic for identical local clock states to avoid exploit claims.

## Game 3 task ledger

### Completed
- Companion folder created at `idle-bakery/` with document plan and production checklist.
- Design document drafted with FTUE, retention, ad policy, and release guardrails.

### Pending
- Confirm monetization offer set after first 5k sessions.
- Finalize prestige reset formula and visible token gain chart.
- Add visual language and onboarding copy for one specific bakery UI style.
- Sync with analytics schema once implementation starts.

## Open questions for next phase
1. Keep `Butter` purchasable via ads only, or allow small-volume optional IAP for day one?
2. Should offline reward cap be hard at 3h or based on building tier?
3. How often should prestige reset become available in first 30 days?
4. Introduce co-op leaderboard from launch or keep local-first until D7?

## Suggested companion docs
- idle-bakery-controls.md: input model, onboarding steps, and accessibility checks.
- idle-bakery-tuning.md: production equations, phase tables, and release workflow.
- idle-bakery-analytics.md: event taxonomy, KPI targets, dashboards, and alert policy.
