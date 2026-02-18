# Game Ideas Documentation

- `game-ideas.md` remains the root idea register and should stay at repository top level.
- Full implementation and launch docs are organized by game directory below.

## Repository structure

- `game-ideas.md`: short concept list and seed ideas.
- `tile-tumble/`
  - `tile-tumble.md`: design + build notes.
  - `tile-tumble-controls.md`: input and UX control spec.
  - `tile-tumble-tuning.md`: progression and balance playbook.
  - `tile-tumble-analytics.md`: telemetry and KPI instrumentation.
- `tap-rocket/`
  - `tap-rocket.md`: design + build notes.
  - `tap-rocket-controls.md`: input and UX control spec.
  - `tap-rocket-tuning.md`: progression and balance playbook.
  - `tap-rocket-analytics.md`: telemetry and KPI instrumentation.
- `idle-bakery/`
  - `idle-bakery.md`: design + build notes.
  - `idle-bakery-controls.md`: input and UX control spec.
  - `idle-bakery-tuning.md`: progression and balance playbook.
  - `idle-bakery-analytics.md`: telemetry and KPI instrumentation.

## Documentation conventions

- Keep the idea list file at repo root to preserve a single source for quick discovery.
- Keep game-specific references in their directories as companion sets.
- Preserve section ordering across companion docs where possible:
  - `Intent`
  - `Quick summary`
  - detailed sections
  - `Testing`, `Reporting`, and `Task ledger` where present.
- Prefer concrete thresholds, guardrails, and follow-up actions over vague adjectives.

## Notes for contributors

- Use explicit headings, numbered release/tuning steps, and stable event fields.
- Include measurement and rollback rules in every tuning/analytics change.
- Before pushing large reorganization updates, confirm directory links and references are still valid from root.
