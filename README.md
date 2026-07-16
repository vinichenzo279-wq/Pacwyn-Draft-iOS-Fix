DRAFT SOLVER — iOS VERSION
FUNDAMENTAL DIFFERENCES AND LIMITATIONS

Based on Draft Solver v21.10.0.1 and v21.10.0.1-iOS.

OVERVIEW

The iOS build is a safety-focused version designed to avoid the Safari crashes caused by very large or worker-heavy searches. It keeps the same current player database and the same core squad scoring rules, but some demanding features are reduced, disabled, or replaced with lighter alternatives.

1. SOLVER DIFFERENCES

• Single-threaded only
  The regular version can divide large searches across multiple browser workers. iOS does not use workers, so every solve runs on one core. The answer should be the same when the search completes, but very large drafts can take longer.

• Hard 8-million-node safety limit
  The main XI solver, pick helper, bench search, and manager search are each limited to a maximum of 8M nodes. The regular version can search much further and can attempt a complete “search to the end.”

• A capped result may not be proven optimal
  When an iOS search reaches its limit, the app returns the best solution found within that budget. Most ordinary drafts finish below the limit and are fully solved, but unusually large card pools may stop before every possibility has been checked.

• No Stop button or live worker progress
  Regular worker-based searches can be stopped and can report live node counts or partial “best so far” results. The iOS solver runs synchronously, so the screen may temporarily feel frozen during a difficult search and the active solve cannot be safely interrupted from inside the app.

• No worker-backed deep-search features
  iOS does not include the regular version’s worker-based bench/manager processing, out-of-position worker search, frontier or shard processing, partial-result merging, or checkpoint-style deep-search features. Its confirmation prompts and safety brakes remain because the search cannot be stopped once the synchronous work begins.

2. CARD-WEIGHT / NO-HOLD CALIBRATOR

• Simpler draft sampling
  The iOS calibrator samples cards from the available database by position and card supply. The regular version builds a more controlled draft-card mix, including configurable Icon/Hero offer averages, variation, and limits on how many special cards may enter the final XI.

• No advanced Icon/Hero mix controls
  The regular version lets calibration model or completely disable specific Icon/Hero offer scenarios. Those controls are not present on iOS, so iOS calibration follows the natural mix in the database instead.

• Less precise in special-card edge cases
  The regular calibrator rejects simulated squads that exceed its Icon/Hero limits and conditionally avoids measuring factors that are irrelevant to the chosen setup. The lighter iOS method does not perform those newer checks, so fitted weights can differ most when the database or intended draft has an unusual number of Icons or Heroes.

• Lighter default and different responsiveness
  iOS defaults to 300 simulated drafts, while the regular build currently defaults to 500. Runs of 1,000 drafts or fewer use a synchronous fit after the status message appears, which can briefly block the interface. Larger runs switch to a chunked process that yields between stages.

3. FORMATION CALIBRATOR

The formation calibrator is the largest functional difference between the two versions.

• iOS uses a lightweight position-quality calculation
  It values the strongest players at each primary position, applies a decreasing weight as it goes deeper down that position’s player list, and averages the values of the eleven positions used by each formation.

• The regular version simulates complete drafts
  It generates full 11-position, five-options-per-pick draft boards, uses primary positions, models the pivot-normal and special-card offer mix, applies Icon/Hero XI limits, builds an actual XI, includes the manager score, and ranks formations using average, best, and worst simulated outcomes.

• Practical limitation
  The iOS formation ranking is much lighter and faster, but it does not fully model five-card offer randomness, interactions between choices across an XI, special-card limits, manager impact, or the spread between good and bad draft outcomes. Its ranking should therefore be treated as a useful position-depth estimate rather than the same full simulation produced by the regular version.

4. PIVOT ANALYZER

The Pivot Analyzer itself is not using the old iOS scoring anymore. Its current Consistent, High, and Max calculations were ported to match the regular v21.9 analyzer, including the current supply, density, reliability, and pivot-specific ceiling rules.

This should not be confused with the two calibrators above: the displayed Pivot Analyzer rankings are aligned, while the card-weight and formation calibration methods still differ.

5. WHAT REMAINS THE SAME

• The current iOS and regular builds contain the same synchronized player database.
• Core squad scoring, chemistry calculation, and normal solver objective are intended to match.
• A solve that completes without hitting the iOS cap should produce the same best score for the same draft and settings.
• Normal drafting, database editing/importing, pivot selection, and the current Pivot Analyzer remain available.

PRACTICAL SUMMARY

The iOS version is suitable for normal drafts and everyday use. Its main compromises are speed, the 8M search ceiling, lack of stoppable worker searches, a simpler card-weight calibrator, and a lighter formation-ranking method.

Use the regular version when a very large draft must be searched deeply, when a mathematically complete result is required after iOS reaches its cap, or when calibration needs controlled Icon/Hero mixes and full formation-draft simulation.

