# Tap Rocket - Detailed Research & Build Notes

## Intent
Single document for Game 2 from the original ideas list: a one-button upward-boost runner with short, repeatable sessions and a clear long-term progression spine.

## Quick summary
Tap Rocket keeps control to a single tap while layering replay value with obstacle rhythm, clear skill progression, and controlled failures. The core loop focuses on small perfect taps into forgiving-but-shaping tunnels, then quick restarts with immediate performance visibility.

## Source-grounded observations
A few practical patterns emerged from design references and recent deep research:
- The first 7-10 seconds matter most for Day 0 retention, so tutorial content should be invisible and gameplay-first.
- One-button interaction lowers entry friction for casual players, but control semantics must be taught explicitly when hold mechanics are used.
- In Flappy Bird, simple and predictable physics (consistent jump impulse + gravity response) let players learn through short repetition.
- One-touch endless runners (Jetpack Joyride and Flappy Bird style) work best when early onboarding windows are forgiving and low-risk.
- Responsive input and clear feedback are essential; delayed or opaque control outcomes kill retry intent quickly.
- Fairness perception comes from conservative hitboxes and visible near-miss moments more than strict pixel-perfect collision rules.
- Dynamic difficulty adjustment (DDA) can improve flow and reduce rage exits compared to static ramps.
- Rewarded Video revives usually outperform forced interstitials when offered at choke points like run-end failures.
- Strong early retention benchmarks for this class of game are close to D1 = 35-40%.

Sources used:
- https://www.gamedeveloper.com/design/admiring-the-game-design-in-hyper-casual-games
- https://www.gameanalytics.com/blog/hyper-casual-game-common-mistakes
- https://www.gameanalytics.com/blog/monetizing-hyper-casual-games
- https://www.pocketgamer.biz/the-key-to-success-how-top-100-downloaded-games-implement-interstitial-ads-and-related-in-app-purchases/
- https://www.gamedev.net/tutorials/game-design/game-design-and-theory/admiring-the-game-design-in-hyper-casuals-r5146/
- https://dspace.cuni.cz/bitstream/handle/20.500.11956/148732/120396947.pdf?sequence=1&isAllowed=y

## Refined game profile
- Name: Tap Rocket
- Genre: One-Button Runner
- Core action: tap to fire upward boost bursts while auto-forward movement continues
- Primary appeal: short bursty reaction loops with visible progress and tight retry loops
- Device/UX: portrait phone, single tap gesture for all active play

## Core loop (v2)
1. Player starts with a short runway and an unmistakable tap target.
2. Player taps once per obstacle pattern to adjust altitude/path.
3. Rocket runs automatically through a hazard corridor with moving obstacles and gravity modifiers.
4. Coin/path bonus pickups are collected for score and upgrade progression.
5. Distance or segment goal is reached, then run ends or transitions to next challenge.
6. On hit, show cause of failure, offer immediate retry, optional rewarded retry, and quick-restart.
7. Returning players see a best-distance target and a best-of run with simple next-attempt goal.

## Design targets for build quality
- First-play window: keep core feel clear and successful in the first 7-10 seconds.
- Session target: 60-90 seconds for first loop; 120-second long-form runs by segment 8.
- First-run onboarding target: player can safely clear first obstacle with >=85% success within 30 seconds.
- First 10s safety rule: no run-ending failure before at least one successful tap and lane stabilization.
- Forgiveness target: no single obstacle should be unrecoverable once entered.
- Visibility target: every obstacle type must be readable in one glance at tap timing windows.
- Retention rhythm: one short-term objective for first 3 runs, one weekly rhythm objective (streak/route milestone), and one long-term cosmetic milestone.
- Retention benchmark: target D1 session completion to 2 runs at 45%+ and D1=35-40% overall target.

## Movement and obstacle systems
### Control scheme
- Action input: single tap (full-screen touch), no swipes.
- Optional advanced mode: short hold for small safety boost (introduced later only after users complete early levels).
- Tap cooldown: brief minimum gap (e.g., 80-120ms) prevents accidental double input spam and keeps feel intentional.
- Launch flow: start into active game loop first, overlaying only one short guidance callout that fades on first input.

### Corridor design
- Auto-speed starts low; accelerates subtly at fixed distance checkpoints.
- Obstacles grouped by zones (A, B, C) to avoid pure randomness spikes:
  - Zone A: slow gates + simple arches
  - Zone B: alternating ceilings/floors and lateral walls
  - Zone C: moving hazards + gravity wells
- Gravity wells invert gravity direction briefly and visually telegraph with warning ring and color cue.
- Pattern bank approach: authored obstacle chunks with guaranteed safe path and explicit difficulty tags.
- Early timing ramp (recommended):
  - 0-10s: zero risk, wide lane, tutorial only
  - 10-30s: low density static hazards
  - 30-60s: medium density moving hazards
  - 60s+: high density mixed hazards and challenge lanes

### Win/lose condition variants
- Run 1-3: timed distance targets
- Run 4+: segment clear goals (e.g., pass 3 gate stacks before fail)
- Optional survival mode: score ladder with soft rank badges for long-run distance

### Failure shaping
- If player taps too late repeatedly (e.g., 3 consecutive hits), offer one automatic stability assist before restart in daily/session contexts.
- Spawn a recovery lane every N runs so players can recover from mistakes without resetting progression.
- Input buffering/coyote behavior: accept tap slightly before obstacle state transitions where intent is clear.

## Progression and difficulty tuning
### Difficulty curve
- Tier 0 (runs 1-2): wide gate spacing, fixed gravity, generous window.
- Tier 1 (runs 3-5): obstacle variety increases, no more than one hard lane change per 10s.
- Tier 2 (runs 6-10): introduce one gravity modifier and timed hazard pairings.
- Tier 3+: add periodic challenge lanes + optional risk/reward boosters.

### Difficulty controls
- Tune via measured metrics:
  - `tap_to_obstacle_ratio`: taps per 10s of run time
  - `first_retry_survival` and `median run duration`
  - `obstacles_hit_by_type`
- Keep death reasons stable across sessions: no sudden 60-90% difficulty spikes at tier boundaries.
- Use conservative speed bumps; pair each rise with at least one readable counter-arc.
- Add DDA: shift speed/hazard density based on player outcome of last 3 runs to keep flow in target zone.

## Fun mechanics (research-grounded)
- Predictability with pressure: deterministic movement rules + noisy obstacle variation.
- Retry loop dopamine: immediate restart, score reset or retained progress, and one visible best this attempt goal.
- Skill communication: each run should teach one timing intuition (tap cadence, gravity window, lane timing).
- Twist economy: collecting a rare coin in danger zones grants one stored safeguard for future runs.
- Meta goals: light mission track (three slots such as "collect 500 coins", "make 10 perfect gates") supports repeatability.

## Additional idea pack for Tap Rocket
### 1) Orbit Wells
Temporary gravity-well zones that pull sideways before inversion. They reward route planning, not raw reflexes.

### 2) Route Contracts
Before each run, players choose one lane contract:
- speed favor: gain score for long smooth boosts
- control favor: easier obstacle windows but less reward density
- risk favor: denser hazards with higher coin multipliers

### 3) Meteor Drift
A periodic obstacle field where only one exact tap rhythm works, creating memorable sequence chunks.

### 4) Night Run
High-contrast visual mode with reduced background noise but stricter tolerance windows.

### 5) Safe Harbor
At fixed distances, introduce a one-run sanctuary lane where damage can be avoided by choosing a precise low-risk arc.

## Economy and retention ideas
- Soft currency: Fuel (common) for short-term buffs and recoveries.
- Premium currency: Plasma Core (premium) for one emergency revive, booster skip, or cosmetic trail.
- Reward loop:
  - Rewarded video for one extra retry after failure
  - Rewarded video for one map preview after first 5 fails per session
  - Limit revive ads to one per run to preserve run stakes
- Progression gates:
  - Unlock ship skins by segment milestones
  - Unlock lane themes weekly in a predictable cadence
- Ad placement policy:
  - No hard ads in first 3-5 minutes of total gameplay
  - Session 1: avoid interstitials; Session 2+: after 2-3 runs with 30-45s cooldown
  - Keep rewarded options at game-over or optional non-intrusive preview moments
- Missions for retention:
  - 3-slot mission board (coin, distance, timing) every 24h
  - Daily rewards with low friction claims to increase return likelihood

## Testing checkpoints
- FTUE test: can a new player complete a no-obstacle opening and identify at least one recovery opportunity in <12 seconds?
- FTUE clarity check: can core input be understood and executed in <7 seconds after first launch?
- Onboarding clarity: first obstacle pass success rate target 85% for first-timers after tutorial.
- Retry health check: at least 70% of failed players should restart within one attempt.
- Difficulty calibration: no more than 2 consecutive run drops in median distance after each new tier launch.
- Session stability test: median 2-minute retention from first open to 3rd run.

## Production notes from research
- Use deterministic seed-based obstacle sequences for debug and A/B tuning.
- Keep obstacle and gravity event data in config-driven tables (zone, spawn rate, hazard type, tolerance window).
- Add analytics from round one: tap timing, lane entry position, first obstacle fail reason, retry source, retry mode used.
- Test with different screen sizes and thumb reach; one-button should work equally one-handed.
- Keep all gravity inversion mechanics behind a readable tutorial callout first time encountered.
- Validate generated obstacle chunks so each has a guaranteed recovery option and no unavoidable failure.
- Prefer authored pattern chunks over fully random spawns in early tiers.

## Active follow-up tasks for Game 2
- Incorporate the deep-research outputs from `trun_04740bf111ed4e89bfb8a4d4e2268a21` into concrete tuning thresholds in companion docs.
- Set and monitor mission conversion and D1 metrics against explicit targets.
- Finalize DDA formula and guardrails with data from first 5k run cohort.
- Add and version explicit tuning tables (`tap-rocket-tuning.md`) for `tap`, `obstacle_density`, and `difficulty_index` by segment.
- Add a standard `run_state` enum and retry reason taxonomy in `tap-rocket-analytics.md` and implement in telemetry.
- Keep this file and three companion docs in sync whenever assumptions are adjusted.

## Open questions for next phase
1) Keep hold-for-stronger-boost disabled at launch and unlock only after explicit milestone?
2) Do we want one lane or two-lane choice after death recovery events?
3) Should rewards prioritize short-term replays or long-form progression by default for initial audience?
4) Do we need social features from day one (ghost runs / leaderboards), or launch with local-only ranking first?
5) What DDA cap values feel safe on day one (speed, obstacle_density, gravity intensity)?

## Companion docs
- tap-rocket-tuning.md: launch parameters, phase tables, and DDA math.
- tap-rocket-controls.md: input timing, control response, and onboarding script.
- tap-rocket-analytics.md: KPIs, event taxonomy, and experiment alerts.