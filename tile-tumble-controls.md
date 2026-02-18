# Tile Tumble - Controls Specification

## Intent
Define interaction behavior so swap actions are predictable, responsive, and accessible from day one.

## Quick summary
Tile Tumble uses a low-friction touch model: one drag to swap adjacent tiles, visual hover feedback, and deterministic fallback when inputs are ambiguous.

## Input contract
- Primary input: drag between orthogonally adjacent tiles.
- Secondary input: tap on objective tiles and boosters.
- Event order: touch-start -> direction lock (first movement axis) -> swap candidate preview -> on-release confirmation.
- Ignore diagonal moves and multi-touch while a swap animation is active.
- Start state: game starts directly in board viewport with one contextual CTA.

## Touch timing and physics
- Drag deadzone: 8px minimum to avoid accidental moves.
- Direction lock timeout: 150ms to avoid jitter and accidental diagonal capture.
- Swap transition: 120-160ms ease-in and quick settle.
- Input latency budget: 80ms max from release to board settle update on modern devices.
- Consecutive taps/drag inputs while an animation is running are queued once and deduplicated.

## Accessibility and platform guidance
- iOS minimum touch target: 44x44 pt for action controls.
- Android minimum touch target: 48dp for action controls.
- Keep ad close button and key board controls at or above these sizes.

## Ambiguity handling
- If input does not form a valid adjacent pair within 500ms, show subtle hint on nearest movable tile.
- If user drags a tile where multiple swaps are possible, lock on dominant axis and ignore micro jitter.
- If two swaps happen in one quick gesture, apply only first legal swap.
- If no legal swap can be executed in 2 seconds of active input, pause and show "Try swiping this direction" tooltip.

## Onboarding script

### First board
1. Show one large legal move highlight for 6 seconds or until action.
2. When swap is made, play a short lock sound and board settle pulse.
3. Display objective icon and "one goal at a time" help bubble.

### Early boards
- Introduce blockers on board 5 with one explanatory chip.
- Introduce boosters on board 8 only after first successful goal completion.
- Do not add hold-to-confirm behavior or advanced actions before board 4.

## Device and UI adaptation
- Hit box targets:
  - Board tile minimum: 44x44 logical px.
  - Top bar actions: minimum 48x48 logical px.
  - Booster button spacing: at least 16px between touch targets.
- Layout should keep the active board within thumb-reachable vertical zone on phone and tablet.
- Respect safe-area insets, especially near lower corners.
- Reduce accidental taps during ads by shrinking board interaction area until ad closes.

## Accessibility defaults
- Add explicit alternate visual cues for swaps and matches.
- Offer motion-reduced mode with reduced sparkle intensity and shorter animation loops.
- Add sound-on-hold and optional vibration patterns for large matches and recoveries.
- Color-blind mode: map tile sets to both color and shape, with optional high-contrast palette.

## Failure handling in controls
- If touch jitter causes an accidental cancellation, show "Not a legal swap" and restore tile position.
- On failed drag, avoid full screen shake; use small corrective pulse.
- Retry mode should be one tap away with no extra setup.

## QA checks
- First-try swap recognition rate > 90% in usability smoke tests.
- Invalid drag rate < 12% after board 3 in test cohorts.
- Tap-to-continue latency should be < 120ms on target devices.
- Color-blind and reduced-motion scenarios should preserve board readability.
- Verify first-tap hint behavior works when player idles 3 seconds on each of first 3 levels.
