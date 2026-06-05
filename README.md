**Projecting MLB Hitters' Next-Season OPS**

Forecasting an MLB hitter's OPS for an upcoming season using only information knowable beforehand: traditional stats, age, and park factors, plus Statcast expected stats (xwOBA). Built in R.

**The idea**

OPS is defined as on-base plus slugging, so predicting a season's OPS from its own components is circular and not really prediction at all. This project treats it as a genuine forecasting problem instead. Every feature comes from prior seasons, and the model is judged on seasons it never saw. It also tests one question the Statcast era makes possible: do "expected" stats, meaning what a hitter should have produced based on how hard and at what angle he hit the ball, forecast the future better than what he actually produced?

**Key findings**

Quality of contact is a better leading indicator than results. Prior-year xwOBA correlates 0.466 with next-season OPS, ahead of prior-year actual OPS at 0.423.
The model beats the standard baselines. On held-out seasons (2023 to 2025) the Statcast-era regression reaches RMSE 0.080 and R-squared 0.24, ahead of the Marcel projection system (0.087, 0.11) and far ahead of "repeat last year," which posts a negative R-squared because it re-bets last season's luck.
Statcast adds a measured edge. Adding xwOBA on top of the traditional features lifts adjusted R-squared from 0.255 to 0.264, a small but repeatable gain.
The ceiling is low because the problem is hard. The model regresses extremes toward the mean by design, so it will not call a breakout. An R-squared near 0.25 is a realistic ceiling for a single-season forecast.

**What's in this repo**

.
├── README.md
├── mlb_ops_story.Rmd        # primary analysis: an interpretable regression, built one layer at a time
├── mlb_ops_story.pdf        # rendered report (the version shared on LinkedIn)
├── extended/
│   ├── mlb_ops_projection.R # full-history pipeline: linear, elastic net, gradient-boosted trees, SHAP
│   └── mlb_ops_statcast.R   # Statcast-era comparison: how much do expected stats add?
└── figures/                 # generated plots
The primary analysis (mlb_ops_story.Rmd) is the polished narrative: it builds an ordinary least squares model up one layer at a time and knits to a self-contained HTML report.
The two scripts under extended/ are the broader modeling exploration. They predate the final regression and run on the full Lahman history rather than the Statcast era, so their numbers differ from the headline report. They are kept to show the wider toolbox: regularization, tree-based models, model comparison, and SHAP interpretability.

**Reproducing it**

R 4.x with: Lahman, dplyr, tidyr, ggplot2, broom, glmnet, xgboost, baseballr, httr, readr, purrr.
The Statcast portions pull from Baseball Savant and the Chadwick register, so the first run needs an internet connection. Results are cached to a local cache/ folder, so re-runs are fast.
Knit mlb_ops_story.Rmd to HTML in RStudio. To produce the PDF, install a LaTeX engine once with tinytex::install_tinytex().

**Method notes**

Every predictor is lagged to prior seasons only, which is what avoids the circular use of a stat's own components.
The 2020 season (60 games) is excluded everywhere, since its rates are not comparable to full seasons.
Player ids are linked across Lahman and Baseball Savant through the Chadwick register, matching 100 percent of qualified hitters in the sample.

**Data**

Lahman Baseball Database: traditional stats, biographical data, park factors.
Baseball Savant (Statcast): expected statistics.
Chadwick Bureau register: the id crosswalk between sources.

**Author**

Evan Heffelfinger
