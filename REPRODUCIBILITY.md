# Experimental reproducibility

**Study:** An intelligent global irradiance forecasting framework based on a WOA-optimized FCM–CNN–WNN–Transformer hybrid network for solar electric vehicle energy management  
**DOI:** https://doi.org/10.1016/j.measurement.2026.121970

## Scope of implementation

The checked-in code provides a runnable implementation family: train-fitted
FCM memberships are appended to the input features, convolutional features and
a fixed Morlet filter bank feed Transformer attention, and the optional WOA
search selects learning rate and dropout. These are implementation facts, not
confirmation that the corresponding equations or search protocol match the
article.

The repository does not contain the full manuscript, source data, or original
experiment configuration. Before numerical comparison, verify the WOA update
equations and search budget, fuzzy-regime treatment, network dimensions,
feature/target definitions, forecast horizon, metrics, and train/test dates
against the article. Paper-level agreement is not established by this code.

## Data handling and evaluation

Measured data are supplied as an external time-ordered CSV rather than packaged with the source code. Normalization statistics and fuzzy-regime parameters, where used, must be fitted on training observations only. Input windows must not include future target values.

The loader sorts rows by the selected time column, requires numeric finite
features and target values, and rejects missing or unknown feature columns.
`--features` should explicitly name the predictors used in the study; without
it, all numeric columns other than the time column are selected, with the
target included as an autoregressive input. The first prediction uses only
observations preceding its target timestamp.

The default training workflow uses chronological partitions for model development, residual calibration (when applicable), validation and final evaluation. Multiple cities, sites, meteorological regimes or horizons need separately documented holdout protocols; a successful single-series run is not evidence of generalization.

## Reporting requirements

A repeatable experiment records the source dataset and its license, feature definitions, sampling period, target units, forecast horizon, random seeds, optimizer budget, train/test boundaries and all metric definitions. The software check `python train.py --smoke` uses simulated signals and serves only to validate execution.

Paper-level numerical agreement has not been independently established. The results of new runs should be reported as new experiments, with explicit comparison settings rather than attributed to the published results.
