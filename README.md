# CHALLENGE 03:
## THE ZOLDYCK ASSASSINATION CONTRACT BOARD

## Story

The Zoldyck Estate. Kukuroo Mountain. Home to the world's most feared family of assassins.

But even the Zoldycks outsource.

Deep within the estate, past the Testing Gate, past Mike the guard dog, there is a room few ever see: the Contract Board. A massive display where assassination contracts from across the world are posted, updated, and claimed.

You stand before it now.

Silva Zoldyck's voice rumbles from behind you:

> "You're not family. But you've been granted access. Prove yourself worthy."

The board flickers. Fifty contracts shimmer into view, each one showing a target, a price, a deadline, and a location.

> "Rules are simple," Silva continues. "You can hold five contracts at a time. No more. Each contract requires specific skills. Some skills you have. Some... you'll need to earn."

Zeno appears beside him, stroking his beard.

> "Travel takes time, boy. The world is not small. Choose your route wisely, or you'll spend all your time in transit and none in execution."

Silva adds:

> "You have until the next Hunter Exam begins. Two hundred days. Every contract you complete adds to your reputation and your purse. Every contract you fail subtracts. Fail too many, and Mike gets a new chew toy."

Kalluto slips into the room silently and places a small device in your hand: a location tracker and skill analyzer.

> "Don't embarrass us."

## Task

Build an optimization engine that:

1. Selects the optimal contract sequence.
   Which contracts to accept, and in which order, to maximize gold.
   It must account for travel time, execution time, deadlines, and the 5-contract limit.
   It must also account for skill improvement unlocking better contracts.

2. Plans travel routes.
   Given the current location and active contracts, compute the best travel order.
   Contracts in the same city or nearby cities should be chained efficiently.
   The route must always respect deadlines.

3. Manages skill progression.
   Track which skills improve after each completed contract.
   Predict which contracts become available after those gains.
   Balance skill farming on easier contracts against higher-gold difficult contracts.

4. Handles uncertainty.
   Some contracts have hidden complications: a 20% chance that execution takes 1.5x longer.
   Some contracts are traps: the real difficulty is secretly one tier higher.
   Traps are only detectable after accepting but before traveling, and abandoning them causes a reputation penalty.

5. Produces a timeline.
   Provide a day-by-day breakdown of location, active contracts, completed work, and total gold at the end.

## Constraints

- You can only be in one city per day.
- You can only hold 5 active contracts at once.
- Contracts must be accepted before traveling to their location.
- Deadlines are hard. Arriving late means the contract fails.
- Failed contracts reduce future rewards by 10% per failure, stacking over time.
- Trap contracts can be abandoned after detection, but each abandon carries a reputation penalty.

## Repository Structure

This repository currently contains:

```text
zoldyck-challenge-03/
├── README.md
├── CHALLENGE_EXPLAINED_AR.md
├── ENGINE_LINE_BY_LINE_AR.md
├── data/
│   ├── contracts.json
│   ├── map.json
│   └── profile.json
├── src/
│   └── engine.py
└── output/
    ├── map_visualization.html
    ├── optimal_path_report.txt
    ├── skill_progression_log.txt
    └── strategy_document.txt
```

## Output

The challenge asks for a repository containing:

1. Runnable optimization engine code that accepts contracts JSON, map JSON, and a starting profile.
2. `optimal_path_report.txt` with the day-by-day plan.
3. `skill_progression_log.txt` showing skill gains and newly unlocked contracts.
4. `strategy_document.txt` explaining the algorithm and tradeoffs.
5. A map visualization, which is optional but valued.

This repository includes all of the above.

## Run

Requirements:

- Python 3.8+
- No external dependencies

Run from the project root:

```bash
python3 src/engine.py .
```

This writes the output files to `output/`.

## Implementation Summary

The engine in [`src/engine.py`](./src/engine.py) uses a greedy heuristic:

- It scores each feasible contract using gold, travel cost, execution time, deadline pressure, and trap risk.
- It accepts the best available contracts up to the active limit.
- It chooses the next destination using a route score that favors good gold-per-day outcomes while respecting deadlines.
- It updates skills after each successful contract and immediately checks which new contracts become unlocked.
- It simulates uncertainty through execution complications and trap detection.

## Notes

- The engine is data-driven: it reads `contracts.json`, `map.json`, and `profile.json` directly, so it can run on different input sets without code changes.
- The current dataset in [`data/contracts.json`](./data/contracts.json) contains 25 contracts.
- The additional Arabic markdown files are explanatory notes and are not required for running the solution.

## Decode This Passage

> "Gold glitters but time rusts. The shortest path is not always the richest. A skill honed on easy prey pays dividends on the mighty. The trap that catches the hunter was never a trap. It was a lesson priced in blood. Five fingers hold five fates. The sixth is the one you haven't chosen yet. Travel light, strike fast, and remember: the Exam waits for no one, not even the best. The board resets. Your score does not."

Interpretation:

- Raw gold is not enough; time efficiency matters.
- The locally shortest route is not always the globally best strategy.
- Early easy contracts can be valuable because they unlock stronger contracts later.
- Trap handling is part of optimization, not just failure avoidance.
- The 5-contract limit is a strategic resource.
- Every acceptance decision closes off other opportunities, so sequencing matters.
