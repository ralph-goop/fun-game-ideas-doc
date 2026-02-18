# Tile Tumble - Detailed Research & Build Notes

## Intent
Single document for Game 1 from the original idea set: a simple but rich match-3 tile puzzle game.

## Quick summary
Tile Tumble is a casual puzzle game with a swap-to-match core loop. It works because match-3 patterns are instantly understandable, highly rewarding, and easy to access on mobile through one hand and low friction. This doc turns the original concept into a build-ready design sheet with research-backed ideas and a fast iteration plan.

## Research-backed findings
- Casual match-3 and tile puzzles are most successful when level fields and rules are carefully crafted by hand, with manual level design used to shape difficulty and progression.
- A 2D grid board with simple tap/drag/swipe actions is standard in this genre and supports quick learning.
- Tile puzzle players stay longer when first-run friction is low: clear one core mechanic, introduce helpers only as separate steps, and avoid sudden one-screen complexity.
- Match-3 games can maintain engagement without punishing failure heavily: wrong attempts should feel low cost, and accidental moves should still feel harmless.
- Positive feedback loops are important: visible progress, cascades, clear reward pulses, and short progress loops around each board.
- For long-term retention, introducing weekly/dynamic content and tuning by player progress data is more effective than static hardcoded pacing.
- For level balance, pass-rate alone is less useful than player effort data (such as attempts per level and time-to-success).

### Sources used
- https://www.gamedeveloper.com/design/smart-casual-the-state-of-tile-puzzle-games-level-design-part-1
- https://medium.com/design-bootcamp/design-analysis-of-match-3-games-fb63879ecd8f
- https://gamedesignskills.com/game-design/casual/

## Refined game profile
- Theme: 