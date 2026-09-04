# Does Driving a Ford Increase Fatal Crash Risk in NYC?

A statistical investigation into whether vehicle make — specifically Ford —
is associated with fatal crash outcomes in New York City, using logistic
regression and decision tree modeling on NYC's Motor Vehicle Collisions
dataset.

**Group 6**
**My contributions:** Coding and analysis, alongside one teammate — specifically the decision tree model (feature importance, confusion matrix, performance evaluation) and the subsample robustness checks (re-running the logistic model across 8 different subsamples to test whether the Ford result held up across contexts).

## Research Question

Does driving a Ford vehicle increase the probability of a fatal crash outcome
in New York City?

## Hypothesis

Ford vehicles were expected to be **statistically insignificant** as a
predictor of fatal crash outcomes — the theory being that driver behavior,
environmental conditions, and vehicle class (not brand) are the real drivers
of crash severity, and that Ford's frequent appearance in raw crash counts
mostly reflects its concentration in trucks and SUVs rather than any
brand-specific risk.

## Data

- **Source:** [NYC Open Data - Motor Vehicle Collisions - Crashes](https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95)
- **Raw size:** 89,102 vehicle-level observations, 27 variables (each row is
  one vehicle involved in a crash, not one crash)
- **After cleaning:** 68,687 observations (~20,415 rows dropped for missing
  values in key fields, plus implausible vehicle years outside 1900-2024)
- **Dependent variable:** `DEATH` — binary, 1 if at least one person was
  killed in the crash

| Metric | Value |
|---|---|
| Overall fatality rate | 0.055% (38 of 68,687) |
| Ford-involved crashes | 8,193 |
| Ford fatality rate | 0.114% |
| Night crashes (8pm-6am) | 15,511 |
| Rush hour crashes | 24,618 |
| Weekend crashes | 16,805 |
| Driver inattention/distraction | 14,691 |

## Methodology

1. **Data cleaning** — dropped rows missing vehicle make, persons killed,
   driver sex, crash date, or vehicle year; filtered implausible vehicle years
2. **Logistic Model 1 (DV = DEATH)** — 18 predictors including `IS_FORD`,
   driver behavior (speeding, distraction, unlicensed), crash conditions
   (night, rush hour, front impact), season, and vehicle type
3. **Logistic Model 2 (DV = INJURY)** — same specification, alternate outcome
   variable, to check whether Ford's insignificance held for a less extreme
   (and less rare) outcome
4. **Subsample models** — Ford's coefficient re-tested across 8 different
   subsamples (male-only, sedan-only, SUV-only, by vehicle age) to check
   whether the null result was stable across contexts
5. **Decision tree** — a non-parametric model to capture non-linear
   relationships and check whether Ford ranked among the top predictors of
   fatality by feature importance

## Key Findings

- **`IS_FORD` was not statistically significant** in the full logistic model
  (p = 0.133) once driver behavior, crash conditions, and vehicle type were
  controlled for — despite being significant in a naive, single-variable model
  (p = 0.034) before controls were added.
- The result held when the outcome was changed from `DEATH` to `INJURY`
  (p = 0.583).
- **Ford's coefficient was unstable and never significant across 8
  subsamples** (male drivers, sedans, SUVs, by vehicle age) — evidence that
  the apparent Ford effect is not robust to how the sample is sliced.
- The decision tree's top predictors of fatality were `NIGHT_CRASH`,
  vehicle type (sedan), month, and multi-vehicle involvement — **Ford did not
  appear among the top features**, reinforcing the regression results.
- The strongest, most consistent predictors of fatal outcomes across every
  model were **crash conditions and driver behavior**, not vehicle brand.

## Model Performance Note

The decision tree achieved 77.2% overall accuracy, but this is misleading
given severe class imbalance — fatalities make up under 0.05% of the dataset.
Precision on the fatal class was just 0.2% (8 of 11 actual fatalities
correctly identified in the test set), which is disclosed explicitly as a
limitation rather than presented as a strong predictive result.

## Limitations

- Dataset covers 2016-2018 only (missing June-July), limiting generalizability
- Data is recorded at the vehicle level, not the crash level
- Findings are specific to NYC and may not generalize to other driving
  environments
- Key variables were unavailable: road type, weather conditions, posted speed
  limits, driver age — all plausible omitted variables
- Extreme class imbalance (38 fatalities out of 68,687 observations) limits
  the reliability of any predictive (as opposed to explanatory) model

## Conclusion

Across every model specification — full logistic regression, an alternate
outcome variable, 8 subsamples, and a decision tree — vehicle make was not a
statistically significant predictor of fatal crash outcomes. The evidence
does not support the claim that driving a Ford increases fatality risk in
NYC; differences in raw fatality counts by brand are better explained by
crash conditions, driver behavior, and vehicle class than by brand itself.

## Tools & Libraries

`Python` · `pandas` · `NumPy` · `statsmodels` (logistic regression) ·
`scikit-learn` (decision tree) · `Matplotlib` · `Seaborn` · `Google Colab`

## Repository Contents

```
├── motor_vehicle_collision_analysis.ipynb   # Full analysis notebook
├── Group_6_Final_Report.pdf                 # Written report with all model outputs
└── README.md
```

## How to Run

```bash
pip install pandas numpy statsmodels scikit-learn matplotlib seaborn
jupyter notebook motor_vehicle_collision_analysis.ipynb
```

The raw dataset (`Motor_Vehicle_Collisions_Crashes.csv`) can be downloaded
from [NYC Open Data](https://data.cityofnewyork.us/Public-Safety/Motor-Vehicle-Collisions-Crashes/h9gi-nx95)
and is not included in this repository due to file size.
