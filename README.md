# Chess Analytics

Analysing ~5,700 of my own rated chess.com games to work out what's actually
costing me rating — and testing the assumption that it's openings.

I've been around 1500 rapid and 1250 blitz for about a year. The standard advice
at this level is to study openings. This project checks whether the data agrees.

## Findings so far

**It isn't openings, colour, or nerves.**

| Question | Answer |
|---|---|
| Is the rating genuinely stuck? | No — 53.9% rapid win rate, and 670 → 1500 over 18 months. Deceleration, not a wall |
| Does colour matter? | Barely. 54.9% white, 52.9% black — normal first-move advantage |
| Board or clock? | Board. Resignations are 70% of losses in both rapid and blitz |
| Is time pressure real? | In blitz yes — 15.3% of losses are timeouts vs 1.5% in rapid. But secondary |
| Do I collapse in some games? | No. Losses run 9-10 accuracy points below wins at *every* percentile — the whole distribution shifts, it isn't occasional disasters |
| Do stronger opponents rattle me? | No. Accuracy is flat (73-75) across opponent strength while win rate falls 63% → 39%. I play one level of chess regardless of opponent |

**Where that leaves it:** the losses aren't situational. They're the baseline
level of play meeting someone better. Diagnosing further needs move-level engine
analysis — which openings, which phase, which positions.

## Data

Chess.com's public API — no authentication required. Games are exposed one month
at a time via monthly archive URLs. Two years of rated standard chess, Feb 2025
to Sep 2026. Chess960 and unrated games excluded.

The API rejects requests without a `User-Agent` header, and rate-limits on
concurrent requests, so archives are fetched serially with a delay.

## Running it

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Then open `src/explore.ipynb` and set `username` to your own chess.com handle.

## Roadmap

- [x] **V1** — API ingestion, normalisation, clean dataset
- [x] **V2** — exploratory analysis (win rate, colour, loss types, accuracy)
- [ ] **V3** — load to SQLite, rewrite analysis as SQL, focus on blitz where the
      sample is 4x larger and clock data is meaningful
- [ ] **V4** — Stockfish evaluation: centipawn loss by phase, blunder rate by
      clock remaining, conversion from winning positions
- [ ] **V5** — coaching agent generating a study plan from the findings
- [ ] **V6** — dashboard

## Notes

A recurring theme: most of the hypotheses were wrong. Openings, colour and
opponent-strength psychology all looked plausible and none survived contact with
the data. Kept them in the notebook rather than deleting them — the eliminations
are the useful part.