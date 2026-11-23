# One Regular Guy at the Olympics

A Monte Carlo simulation and interactive HTML dashboard that puts one average adult man into 11 Olympic events and measures how far he is from gold. Inspired by the "we need one regular guy at the Olympics" tweet.

## Files

| File | What it is |
|---|---|
| `regular-guy-olympics.html` | Single-file dashboard (no build step). Open in any browser. |
| `regular_guy_olympics_events.csv` | The 11 events with gold-level benchmarks, baseline values and noise assumptions. |
| `regular_guy_olympics_simulations.csv` | 66,000 simulated performances: 2,000 per event for each of 3 fitness levels. |

## Dashboard

- Pick a fitness level and the number of Olympics to simulate, then run it.
- Each row shows his median result against gold, plus his best single fluke.
- Press **Play** on any event for a stick-figure animation (gold in yellow, you in red; runners swing arms and legs, swimmers stroke, the cyclist pedals, jumpers take off and land, divers tumble into the pool): races (100 m to marathon, swimming, cycling) run him against gold until gold finishes, the long jump and shot put show both arcs landing at their marks, the high jump shows both clearing their heights, and the platform dive drops both from 10 m.

## Events

Track and field: 100 m, 400 m, 1500 m, marathon, long jump, high jump, shot put. Swimming: 100 m and 400 m freestyle. Cycling: 40 km time trial. Diving: 10 m platform.

## Dataset columns

`events.csv`: discipline, event, direction (lower or higher is better), unit, gold_benchmark, regular_baseline, distance_m (races only), noise_sd.

`simulations.csv`: sim_id, fitness_level (couch, average, weekend_jogger), event, discipline, unit, result, gold_benchmark, pct_of_gold, gap_to_gold. Times are in seconds, jumps and throws in metres, diving in points.

## Model

1. Baseline for the regular guy is divided (timed events) or multiplied (distance, height, points) by a fitness multiplier: couch 0.88, average 1.00, weekend jogger 1.12.
2. Each simulated performance applies lognormal noise: `result = baseline * exp(sd * Z)`, with Z standard normal and sd between 0.04 (sprints) and 0.20 (diving).
3. `pct_of_gold` is gold divided by his time (or his mark divided by gold's mark), capped at 100.
4. In races, the distance behind at the moment gold finishes is `D * (1 - pct_of_gold)`.

## Caveats

- Gold benchmarks are rounded approximations of recent Olympic-level results, not official records.
- The regular guy's baselines and the noise levels are assumptions about a typical untrained adult male, not measured data. Replace them in `events.csv` and the `E` array in the HTML to test your own assumptions.
- Real athletes also vary, and real fields have 8 to 16 finalists, so this measures the gap to gold rather than a true finishing place. The "podium range" count uses 95% of gold as a rough threshold.
- The CSVs were generated with Python (seed 2026), so the numbers differ slightly from a fresh run in the browser, which is not seeded.
