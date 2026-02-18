# Idle Bakery Docs

This directory contains the production pack for Idle Bakery and is the canonical reference for idle-loop systems.

## Documents in this set

- `idle-bakery.md`: design strategy, value loop, monetization guardrails, and open questions.
- `idle-bakery-controls.md`: input model, onboarding taps, menu behavior, and accessibility checks.
- `idle-bakery-tuning.md`: progression formulas, phase tables, balancing workflow, and release playbook.
- `idle-bakery-analytics.md`: event model, KPI targets, dashboards, and alert policy.

## Recommended use order

1. Start with `idle-bakery.md` to lock the design and operating assumptions.
2. Use `idle-bakery-controls.md` to define player-facing interaction details.
3. Apply balancing controls in `idle-bakery-tuning.md`.
4. Implement and guard the event model in `idle-bakery-analytics.md`.

## Maintenance checklist

- Keep onboarding milestones, session targets, and economy multipliers aligned across all files.
- If any upgrade, ad trigger, or prestige rule changes, update tuning and analytics together.
- Reconcile ad cooldown and segmentation rules across design + analytics immediately after each release.
