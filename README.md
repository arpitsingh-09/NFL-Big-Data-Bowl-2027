# Bend Before Sacks

**Movement leads pressure for every rookie rusher, and Combine tracking previews it for interior linemen.**

Submission notebook for the **NFL Big Data Bowl 2027**. It reproduces every number and figure in our writeup, using regular season games only.

## The question

Can we tell how good a rookie pass rusher is from *how he moves*, before the sacks and pressures pile up? And can Combine tracking data preview that movement?

## Key findings

1. **Movement is linked to pressure.** Bend speed, depth toward the QB at 1 to 2 seconds, and get-off all relate to pressure rate after controlling for size, roster position and draft pick.
2. **It is not just a result of winning the rep.** The link holds when movement is measured only on rushes without pressure, and when movement is measured on odd rushes and pressure on even rushes.
3. **Movement is a leading indicator.** Movement measures stabilize in roughly 20 to 40 rushes, while pressure rate needs 100 or more. Early-season bend speed predicts later pressure at least as well as early pressure itself.
4. **Size is a trap.** Raw 40 times mostly restate body weight, so all comparisons are size adjusted.
5. **Combine tracking helps for interior linemen.** A *Trench Burst Index* built from Combine tracking relates to NFL pressure on interior snaps (odds ratio about 1.30 per SD faster), but not on edge snaps.
6. **Negative control.** The same 40 yard trace index is applied to offensive linemen as a sanity check.

### Caveats

* Samples are small: about 37 interior linemen have a Trench Burst Index, and individual draft classes have 11 to 14 players.
* A cross validated block test did not show that Combine tracking improves prediction beyond size alone, and no single Combine metric passed the family wise permutation test.
* Only 10 players ran the Hoop drill twice, so rep to rep reliability is thin.
* Treat the Combine results as suggestive, one clue alongside film and other evidence.

## Metric dictionary

| Metric | Definition |
|---|---|
| Get-off | Seconds from the snap until the rusher crosses the line |
| Speed at t | Rusher speed t seconds after the snap |
| Depth toward QB at t | Upfield displacement from the snap position, using play direction |
| Bend speed | Speed at the frame of the sharpest turn between 0.3 and 3.0 s (turn rate is speed times change in direction) |
| Pressure | A recorded time to pressure on that rush; player rates are shrunk with a beta prior |
| Pressure over expected | Actual minus a LightGBM expected pressure trained with players held out |
| Hoop Clock | Time from first movement above 1 yd/s to the end of the Run the Hoop rep |
| Trench Burst Index | Average of four standardized Combine tracking measures: Hoop Clock, 40 yard trace top speed, 40 yard trace peak acceleration in the first second, and tracked 10 yard time |
| Controlled correlation | Spearman partial correlation after Combine weight, interior or edge roster position, and draft pick |

## Notebook contents

| Section | What it does |
|---|---|
| 1. Setup and helpers | Loads data, builds the player table and size controls |
| 2. Rush signature and context model | Builds per rush movement features and the expected pressure model |
| 3. Cause or consequence? | Non-pressure and cross fit tests |
| 4. Leading indicator | Stabilization curves and weeks 1 to 6 vs 7+ forecast |
| 5. The size trap | Weight vs the stopwatch 40 |
| 6. Combine tracking measures | Features from the Combine drills and the Trench Burst Index |
| 7. Edge vs interior | Results split by alignment, draft class validation, scouting board |
| 8. Rush level model | GEE logistic model with expected pressure as a covariate |
| 8b. Tracking vs the stopwatch | What tracking adds beyond the stopwatch 40 |
| 9. The Hoop Clock | Fast vs slow Run the Hoop rep |
| 10. In season grade | Bend Over Expected and a scouting card |
| 11. Animation | Combine hoop rep next to an NFL rush |
| 12. Robustness | Extra controls and subsets |
| 13. Negative control | Offensive line check |

## Data

The data is **not included** in this repository. It comes from the NFL Big Data Bowl 2027 competition on Kaggle. You need these files:

* `players.csv`
* `combine_results.csv`
* `games.csv`
* `combine_tracking.csv`
* `player_play.csv`
* `game_tracking_2023.csv`, `game_tracking_2024.csv`, `game_tracking_2025.csv`

The notebook looks for them under `/kaggle/input/`. To run it elsewhere, change the `DATA = ...` line in the setup cell to point at the folder holding the files.

## How to run

**On Kaggle (easiest):** attach the competition dataset to a notebook, upload `bend-before-sacks-dl-combine-tracking.ipynb`, and run all cells. Runtime is about six minutes on a standard CPU session. No GPU is needed.

**Locally:**

```bash
pip install numpy pandas scipy scikit-learn lightgbm statsmodels matplotlib seaborn jupyter
jupyter notebook bend-before-sacks-dl-combine-tracking.ipynb
```

Then update the `DATA` path as described above.

## Outputs

* Figures saved as `fig3.png`, `fig5.png`, `fig6.png` and the scouting card
* An animated GIF comparing a Combine hoop rep with an NFL rush
* A scouting board of interior defensive linemen ranked by size adjusted Trench Burst Index
* An in season Bend Over Expected grade for rookie rushers

## Repository structure

```
.
├── README.md
└── bend-before-sacks-dl-combine-tracking.ipynb
```

## Acknowledgments

Data provided by the NFL and NFL Next Gen Stats through the NFL Big Data Bowl.

## License

Add a license here (for example MIT). Note that the competition data has its own terms of use and should not be redistributed.
