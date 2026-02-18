# Tap Rocket Docs

This directory contains the full production pack for Tap Rocket and serves as the implementation reference for one-button runner systems.

## Documents in this set

- `tap-rocket.md`: design strategy, obstacle system, economy, and open questions.
- `tap-rocket-controls.md`: tap/hold input behavior, timing windows, and accessibility checks.
- `tap-rocket-tuning.md`: phase-by-phase tuning controls, DDA rules, and release workflow.
- `tap-rocket-analytics.md`: event schema, KPI targets, dashboards, and operational reporting.

## Recommended use order

1. Start with `tap-rocket.md` to align the game's core loop and constraints.
2. Set interaction details in `tap-rocket-controls.md`.
3. Apply tuning policy in `tap-rocket-tuning.md`.
4. Finalize measurement in `tap-rocket-analytics.md`.

## Maintenance checklist

- Keep movement/timing constants aligned between design and tuning files.
- Ensure analytics fields are updated whenever a new retry, reward, or DDA behavior is added.
- Verify ad pacing and cooldown guardrails are reflected in all core game docs.
