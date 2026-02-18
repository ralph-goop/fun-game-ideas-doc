# Tile Tumble - Detailed Research & Build Notes

## Intent
Single production-focused document for Game 1 from the original ideas list: a mobile-first match-3 game with deterministic puzzles, gentle pacing, and a release-ready retention loop.

## Quick summary
Tile Tumble keeps the classic swap loop and adds modern retention and anti-friction systems: short onboarding, forgiving early failure, clear board goals, and monetization that rewards optional replay instead of punishing drop-off.

## Source-grounded observations
Research and industry practice for hyper-casual and match-3 loops suggest:
- Manual board authorship is the strongest control mechanism for puzzle pacing, while small random bonuses can improve replayability.
- One-handed touch (tap + drag) with a visible first action is the single largest early-retention lever in mobile onboarding.
- Early progression should be low risk: avoid hard fail states before players see multiple clear win conditions.
- Match confidence improves with immediate, short feedback loops that close each action in under a second.
- Repeat frustration lowers when failures are recoverable through retries, not through hidden penalties.
- Retention tuning is most stable when difficulty changes are small and measured against board-level KPIs.
- Manual-only design is too slow for 10k+ level scale, while uncontrolled generation introduces solvability risk. Hybrid manual+procedural with solver validation gives quality and throughput.
- Fair DDA should reduce rage exits, not increase monetization friction by making levels suddenly harder for paying users.
- Interstitial ads are best placed at natural breaks and paced for retention; rewarded ads are strongest when used as optional recovery.
- Technical quality thresholds (ANR/crash) materially impact store visibility and retention in 2026 mobile ecosystems.

Sources used:
- https://www.gamedeveloper.com/design/admiring-the-game-design-in-hyper-casual-games
- https://www.gamedeveloper.com/design/smart-casual-the-state-of-tile-puzzle-games-level-design-part-1
- https://www.gameanalytics.com/blog/hyper-casual-game-common-mistakes
- https://gamedesignskills.com/game-design/casual/
- https://www.gamigion.com/match-3-level-design-principles/
- https://www.mdpi.com/2079-9292/12/19/4098
- https://android-developers.googleblog.com/2022/10/raising-bar-on-technical-quality-on-google-play.html
- https://support.google.com/googleplay/android-developer/answer/9844486?hl=en

## Refined game profile
- Name: Tile Tumble
- Genre: Match-3 / casual puzzle
- Core action: select and swap adjacent tiles to create matches of three or more
- Primary appeal: fast recognition, short replay loops, and clear objectives
- Device/UX: portrait phone/tablet, one-handed friendly board interaction
- Economy style: soft currency pacing plus rewarded-video helpers and cosmetics progression

## Core loop (v2)
1. Player opens a board with one visible objective and one visible score target.
2. Player makes one adjacent swap.
3. Matches resolve, points are awarded, gravity settles, and cascades continue.
4. Player gets immediate feedback through audio, particles, and score text.
5. Board difficulty pressure rises through move limit, blocker count, or objective complexity.
6. Board ends with a short summary and clear next-step action.
7. Player can retry instantly, or continue to next board when objectives are met.

## First 3-level onboarding plan

### Level 1: Hook (10-12 moves)
- Objective: score goal or clear-X tiles.
- No blockers.
- Board launch guarantees at least one legal move.
- First move uses ghost-hand guidance; if idle for 3 seconds, one subtle hint appears.
- Target outcome: near-100% completion, clear success loop.

### Level 2: Objective teaching (12-14 moves)
- Introduce one special tile behavior with a spotlight on formation flow.
- No blockers; reinforce “create objective to progress.”
- Target outcome: ~95% completion.

### Level 3: First friction (14-16 moves)
- Introduce first 1-HP blocker in a non-critical tile lane.
- Keep recovery path obvious; no forced fail states.
- If failed, offer one optional rewarded continue with move/top-up grant.
- End board with saga-map continuation cue to establish long-term objective.

## Design targets for build quality
- FTUE window: player should complete first legal move within 8 seconds.
- First board completion target: 85%+ for new players in seeded cohort.
- Session target: 90-120 seconds for easy boards, 120-180 seconds for challenge boards.
- Anti-frustration target: no single board with rage-abort share above 12%.
- Learnability target: expose all core move types by level 4.
- Distinctiveness target: board readability passes color-blind and low-vision presets.
- Retention rhythm: visible milestones at D1, D3, D7, and week 2.

## Board rules and mechanics

### Suggested base board
- Launch profile: 8x8 grid, 6 tile types, top-fill gravity.
- Match rules: 3, 4, and 5 tile lines, with stable scoring multipliers.
- Cascades are rewarded with chain-based bonus tiers.
- Retry policy: one free immediate retry, then one rewarded ad retry before currency spend.
- No board should spawn with zero legal swap.

### Production pipeline (manual vs procedural)
- FTUE and all progression anchors (levels 1-20): manual.
- Midgame and bulk content (21+): hybrid generation, then designer selection and pass.
- Auto-validation checklist before release:
  - solver win-rate pass under the move limit
  - solvability check and guaranteed-opener requirement
  - difficulty score within target band
  - dead-board prevention and auto-reshuffle test

### Win and lose condition variants
- Easy: hit score target under move limit.
- Core campaign: clear blockers/objectives.
- Mid-tier: route objective plus score target.
- Advanced: dual objective boards (clear + speed/chain constraints) with lower failure harshness.

### Difficulty shaping rules
- Level 1-4: no blockers, generous board, visible objective.
- Level 5-12: one blocker family and moderate pressure.
- Level 13-20: mixed objectives, light risk modifiers.
- Level 21+: rotating pressure families and optional boosters.
- All boards are authored and validated by solveability checks before release.

### Anti-frustration controls
- No no-move deadlocks for new players.
- If a board creates repeated failure in a row, inject a guaranteed recovery assist.
- Keep board failure states soft; preserve player progress and present a clear retry path.

## Fun mechanics
- Pattern recognition is kept central; novelty comes from objective and event layers.
- Clear short goals reduce abandonment on difficult boards.
- Cascades remain the strongest retention signal; early boards target shorter but frequent chain moments.
- Loss aversion is reduced with hearts, move refunds, and limited retries.
- Board journal style accomplishments provide low-friction social bragging.

## Optional mechanics pack

### 1) Seasonal tile moods
Add recurring tile behavior changes every ~12 boards:
- Frost mood: neighboring tiles lock briefly after any match and require a thaw tool.
- Ember mood: a 2-step sparkle score bonus after a large match.
- Crystal mood: two-by-two burst when a 4-match lands.

### 2) Clutter meter
Visible clutter meter tracks board quality cleanup speed and encourages soft urgency without timers.

### 3) Neighborhood goals
Three-board mini-goals encourage continuity (example: light 3 lantern tiles over a small stretch of boards).

### 4) Rotating event board
Daily board with a guaranteed 4-match window and one rotating rule modifier.

### 5) Board journal
Collect first-time accomplishments such as first 4-match, first 10-chain board, or no-hint clear.

## Economy and monetization
- Soft currency: Spark for retries, minor boosters, and one-time helper utility.
- Premium currency: Ember for skip helpers, cosmetic themes, and convenience tools.
- Rewarded ads should only be offered at meaningful checkpoints.
- Launch ad guardrails:
  - No interstitial ads before board 4 for new users.
  - First interstitial only after 2 board completions.
  - Interstitials only between board sessions (never in active play).
  - Enforce 3-5 minute interval and hard cap 1-3 per session.
  - After rewarded video, reset interstitial cooldown to reduce fatigue.
- Progression unlocks favor content and cosmetic value first, then utility.
- Use rewarded video as primary monetization, with interstitials later and only at session-safe points.

## Content and level plan
- Phase 1 (week 1): board engine, core loop, onboarding, 40-50 authored boards, one seasonal system.
- Phase 2 (week 2): +30 boards, clutter meter, neighborhood goals, retry economy.
- Phase 3 (week 3+): weekly ops with 2 board families and 1 rotating mechanic.

## Technical quality gates
- Keep player-perceived ANR under 0.47% and crash rate under 1.09%.
- If any device segment shows >8% issue rate, gate release to that segment or hotfix quickly.

## Testing checkpoints (before release)
- FTUE test: first objective understanding in under 8 seconds.
- FTUE success: first board first-attempt completion target 85% for first 1,000 sessions.
- Retry health: rage-abort rate below 12%, retry-to-retry latency trending down.
- Retention proxy: level 10 completion and board swap-to-completion ratio should not degrade at release.
- Accessibility checks: color-blind contrast and touch target size must pass manual QA.

## Production notes from research
- Use data-driven board definitions from day one (`board_id`, `goal_type`, `pressure_type`, `retry_profile`).
- Keep fail states soft for first 10 boards, then introduce tighter pressure gradually.
- Track outcomes by board_id and attempt context before any global tuning changes.
- Validate each new mechanic with controlled A/B flags and release notes.

## Game 1 task ledger

### Completed
- `tile-tumble.md` rewritten into a production-oriented design and tuning blueprint.
- Companion docs planned and added for controls, tuning, and analytics.
- Explicit anti-frustration, ad guardrail, and onboarding targets defined.

### Pending
- Tune exact DDA, reward, and ad-frequency values from first 5k sessions.
- Decide leaderboard scope (local vs global) for launch.
- Confirm board-journal sharing scope and privacy defaults.

## Open questions for next phase
1. Start with clean geometric visual language or themed world art?
2. Launch leaderboard as local campaign-only or global rank from day one?
3. Are ad-free premium upgrades allowed at launch, or delayed to later phases?
4. How frequently should seasonal moods rotate in early weeks?

## Suggested companion docs
- tile-tumble-controls.md: touch contract, touch filtering, onboarding script, and accessibility.
- tile-tumble-tuning.md: board bands, difficulty bands, DDA, and tuning workflow.
- tile-tumble-analytics.md: event model, KPI sheet, dashboards, and alert thresholds.
