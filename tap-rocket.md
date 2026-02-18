# Tap Rocket - Detailed Research & Build Notes

## Intent
Single document for Game 2 from the original ideas list: a one-button upward-boost runner with short, repeatable sessions and a clear long-term progression spine.

## Quick summary
Tap Rocket keeps control to a single tap while layering replay value with obstacle rhythm, clear skill progression, and controlled failures. The core loop focuses on small perfect taps into forgiving-but-shaping tunnels, then quick restarts with immediate performance visibility.

## Source-grounded observations
A few practical patterns emerged from design references and postmortem-style analysis:
- One-button interaction lowers entry friction and is especially effective for casual conversion, but users may misinterpret press/hold intent, so control semantics must be taught explicitly when hold mechanics are used.
- In Flappy Bird, simple, predictable physics (consistent jump impulse + gravity response) helped players learn by repeating runs instead of guessing.
- One-touch endless runners are a known mobile pattern (examples include Jetpack Joyride and Flappy Bird), and short early onboarding windows are critical before adding high-density obstacles.
- Simple controls should be responsive and immediate; delayed or opaque outcomes quickly feel like input lag and kill retry intent.
- Retention improves when failure is followed by easy recovery and frequent micro-goals (rather than only a hard death wall).

Sources used:
- https://blog.stevewetherill.com/2022/01/ea-air-hockey-designing-one-button.html?m=1
- https://abagames.github.io/joys-of-small-game-development-en/restrictions/one_button.html
- https://medium.com/@thomaspalef/game-design-analysis-of-flappy-bird-and-swing-copters-5c6df9fc10f0
- https://www.gamedeveloper.com/design/endless-runner-games-how-to-think-and-design-plus-some-history-
- https://gamedesignskills.com/game-design/player-retention/

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
- Session target: 60-90 seconds for first loop; 120-second long-form runs by segment 8.
- First-run onboarding target: player can safely clear first obstacle with >=85% success within 30 seconds.
- Forgiveness target: no single obstacle should be unrecoverable once entered.
- Visibility target: every obstacle type must be readable in one glance at tap timing windows.
- Retention rhythm: one short-term objective for first 3 runs, one weekly rhythm objective (streak/route milestone), and one long-term cosmetic milestone.

## Movement and obstacle systems
### Control scheme
- Action input: single tap (full-screen touch), no swipes.
- Optional advanced mode: short hold for small safety boost (introduced later only after users complete early levels).
- Tap cooldown: brief minimum gap (e.g., 80-120ms) prevents accidental double input spam and keeps feel intentional.

### Corridor design
- Auto-speed starts low; accelerates subtly at fixed distance checkpoints.
- Obstacles grouped by zones (A, B, C) to avoid pure randomness spikes:
  - Zone A: slow gates + simple arches
  - Zone B: alternating ceilings/floors and lateral walls
  - Zone C: moving hazards + gravity wells
- Gravity wells invert gravity direction briefly and visually telegraph with warning ring and color cue.

### Win/lose condition variants
- Run 1-3: timed distance targets
- Run 4+: segment clear goals (e.g., pass 3 gate stacks before fail)
- Optional survival mode: score ladder with soft rank badges for long-run distance

### Failure shaping
- If player taps too late repeatedly (e.g., 3 consecutive hits), offer one automatic stability assist before restart in daily/session contexts.
- Spawn a recovery lane every N runs so players can recover from mistakes without resetting progression.

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

## Fun mechanics (research-grounded)
- Predictability with pressure: deterministic movement rules + noisy obstacle variation.
- Retry loop dopamine: immediate restart, score reset or retained progress, and one visible best this attempt goal.
- Skill communication: each run should teach one timing intuition (tap cadence, gravity window, lane timing).
- Twist economy: collecting a rare coin in danger zones grants one stored safeguard for future runs.

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
- Progression gates:
  - Unlock ship skins by segment milestones
  - Unlock lane themes weekly in a predictable cadence
- Minimal ad policy: avoid interruption on tap moments; place offers only on game-over and pause.

## Testing checkpoints
- FTUE test: can a new player complete a no-obstacle opening and identify at least one recovery opportunity in <12 seconds?
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

## Active follow-up tasks for Game 2
- Complete deep-research run `trun_04740bf111ed4e89bfb8a4d4e2268a21` and fold any additional quantitative recommendations into these docs.
- Add missing source-backed thresholds if research returns hard numbers for:
  - onboarding conversion and FTUE timings
  - rewarded ad cadence and opt-in conversion
  - interstitial placement cooldowns and acceptable interruption windows
- Replace placeholder assumptions in controls/tuning with confirmed values from validated playtest telemetry.
- Add a shared `run_state` enum and retry mode taxonomy in `tap-rocket-analytics.md` once final schema decision is made.
- Add QA acceptance criteria for every open item in `tap-rocket-controls.md` and `tap-rocket-tuning.md`.
- Track any new tasks from companion docs so all Game 2 work remains in one linked cluster.

## Open questions for next phase
1) Should tap input include hold-for-stronger-boost from release, or keep strictly tap-only for predictability?
2) Do we want one lane or two-lane choice after death recovery events?
3) Should rewards prioritize short-term replays or long-form progression by default for initial audience?
4) Do we need social features from day one (ghost runs / leaderboards), or launch with local-only ranking first?

## Companion docs
- tap-rocket-tuning.md: launch parameters and phase-based difficulty tables.
- tap-rocket-controls.md: input timing, control response, and onboarding script.
- tap-rocket-analytics.md: KPIs, event taxonomy, and experiment alerts.