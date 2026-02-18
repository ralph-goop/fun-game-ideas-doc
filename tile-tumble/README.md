# Tile Tumble Docs

This directory contains the full production pack for Tile Tumble and is the canonical place for puzzle-design decisions.

## Documents in this set

- `tile-tumble.md`: design strategy, onboarding, economy, production planning, and unresolved decisions.
- `tile-tumble-controls.md`: touch input model, ambiguity handling, accessibility, and control QA.
- `tile-tumble-tuning.md`: board tuning knobs, DDA policy, release workflow, and trigger thresholds.
- `tile-tumble-analytics.md`: event model, KPI sheet, alerts, and dashboard strategy.

## Recommended use order

1. Start with `tile-tumble.md` to align on product intent and mechanics.
2. Use `tile-tumble-controls.md` for exact UX behavior.
3. Define balancing and rollout pacing in `tile-tumble-tuning.md`.
4. Finalize measurement and alerting in `tile-tumble-analytics.md`.

## Maintenance checklist

- Keep onboarding, board progression, and ad-policy values synchronized across all files.
- Update companion docs when one knob changes, especially DDA, retry policy, and ad pacing.
- Confirm any production blocker/fail-safe rules in both tuning and analytics guardrails.
