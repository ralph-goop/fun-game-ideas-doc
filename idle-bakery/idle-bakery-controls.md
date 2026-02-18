# Idle Bakery - Controls Specification

## Intent
Define a low-risk interaction model that keeps the bakery loop fast and readable for short sessions.

## Quick summary
Idle Bakery should support one tap-and-tap-plus menus model: collect, upgrade, and prestige actions only, with no ambiguous gestures and consistent haptic/visual confirmation.

## Input contract
- Primary inputs:
  - `Collect` tap: converts available batch output to currency.
  - `Upgrade` tap: opens oven/recipe upgrade sheet.
  - `Prestige` tap: starts confirmation flow.
- Secondary actions:
  - Long-press on `Collect` for bulk collect confirmation.
  - Swipe only in shop carousels for production lanes.
- Input source: touch/pointer events.
- Ignore rapid duplicate taps while a collect animation is active.
- Start flow: open directly to `Main Bakery` screen with `Collect` and top upgrade CTA visible.

## Layout and interaction zones
- Primary CTA sizes:
  - Collect button: minimum 54x54 logical px.
  - Upgrade cards: minimum 52x52 px touch targets.
  - Floating reward banners: dismiss only with explicit close tap.
- Keep primary actions within bottom 65% of viewport for thumb reach.
- Respect safe area insets and keep ad overlays non-overlapping with collect/upgrade controls.

## Timing and response
- Input->collect confirmation: <= 100ms on target devices.
- `Collect` haptic: light pulse + short tick sound on successful claim.
- Tap debouncing: 120ms minimum between repeated collect taps when no progress change occurs.
- Upgrade panel open/close animation: 120-180ms only, no more than 220ms on low-end devices.
- No forced confirmation on quick upgrades; show warning only for prestige.

## Onboarding script

### First 15 seconds
1. Pulse `Collect` for 2 seconds.
2. Play one-line prompt: "Tap Collect to bank your first batch".
3. Open recipe card automatically after first collect.
4. After first collect, show upgrade card with one upgrade suggestion.

### Early progression
5. When first upgrade is completed, open offline reward preview tooltip.
6. At first offline return (or mock return in QA), show `Collect Offline` CTA in top-right.
7. Keep tooltip count under 3 per full play session.

## Failure and edge-case handling
- Offline clock drift: clamp elapsed offline time to max offline window and show stale timestamp in warning text.
- Insufficient funds: show one replacement suggestion action and open upgrade row.
- Invalid actions in loading states:
  - Block upgrade actions during stale inventory sync.
  - Re-enable controls once state resolves and show reason.
- Server-conflict recovery: if balance mismatch detected, show soft toast and retry sync.

## Accessibility defaults
- Minimum text size: 16sp equivalent for value labels.
- Motion-reduced mode: disable floating particle motion, keep count-up transitions.
- Color-first and icon-first upgrade states for low-vision users.
- Haptic patterns:
  - Collect: subtle tick and one short vibration.
  - Prestige confirm: two-tick pattern only if user has haptics enabled.

## Menu and settings behavior
- `Settings` reachable without leaving progress state.
- Pause mode should pause ad countdown timers and stop one-shot notifications.
- `Prestige` should include impact preview and expected next reset reward.
- Daily rewards and streak reminders are non-blocking overlays; do not interrupt collect.

## QA checks
- First-collect success in smoke: 98% of new playtest sessions within 10 seconds.
- Mis-tap rate: less than 8% for first 20 sessions after install.
- Control lock correctness: no duplicate collect events in one touch cycle.
- Collect/upgrade ratio should remain stable after frame drops on low-end devices.
- Accessibility pass: 1) touch targets, 2) high contrast, 3) reduced motion mode.
