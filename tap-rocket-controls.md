# Tap Rocket - Controls Specification

## Intent
Define exact input behavior so every player feels immediate control response with predictable outcomes.

## Quick summary
The control model is one full-screen tap with a possible optional hold bonus for experienced users. Tap-to-burst timing is deterministic with explicit tolerance windows and visible feedback.

## Input contract
- Primary input: full-screen tap
- Tap action: apply upward impulse
- Optional secondary input: short hold for slight stability boost (post-FTUE only)
- Input source: touch and pointer events only
- Ignore multi-touch during active tap lockout

## Tap timing windows
- Tap cooldown: 90ms (hard floor)
- Visual echo: small ring expands for 120ms from contact point
- Input->physics latency budget: 50ms max on modern hardware
- Visual feedback: burst flash + short trail + audio cue within 100ms
- Auto-reset input lock at launch pad for next action by 120ms

## Hold mode rules
- Available only after player completes first 5 successful runs
- Hold threshold: 120-180ms duration
- Hold effect: +12% impulse and +8% vertical tolerance window
- Max hold window: 220ms, above which input is treated as spam and ignored
- Tutorial text appears first time hold is discovered

## Onboarding script
### First run
1. Show rocket with one clear tap target and no hazards.
2. Run: "tap to rise" copy with one large arrow.
3. Spawn one single gate after 3 taps.
4. If missed: show one retry note, no penalty.

### Standard run
- No new hint after first successful obstacle pass.
- Show lane indicator and warning flash for gravity inversion 300ms before activation.

## Tuning and failure handling
- Three late taps in a row triggers one assist overlay in-session.
- Recovery lane: one per 6 runs at beginner, one per 9 at steady state.
- No penalty when recovery lane is used for first-time session users.

## Device behavior
- One-handed hold target: lower-right or lower-center reachable hit region.
- Respect safe-area insets; avoid corners on notched devices.
- Reduce input area when ad overlays are open to avoid accidental taps.

## Accessibility defaults
- Vibrate: weak haptic for every 12th successful tap to avoid fatigue.
- High-contrast hit zone available as user setting.
- Reduce-motion option: remove trail flicker while preserving tap confirmation sound.

## Quality checks
- Tap response acceptance: 95% of taps must register within 50ms budget.
- Touch jitter review: filter within 1 frame to avoid false double taps.
- Failure reason should include tap phase, obstacle phase, and time since previous tap.
