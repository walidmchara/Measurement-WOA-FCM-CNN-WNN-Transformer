# WOA-Optimized FCM–CNN–WNN–Transformer

**Publication:** An intelligent global irradiance forecasting framework based on a WOA-optimized FCM–CNN–WNN–Transformer hybrid network for solar electric vehicle energy management  
**Journal:** Measurement (2026)  
**DOI:** https://doi.org/10.1016/j.measurement.2026.121970

## Method

The method combines soft atmospheric-regime clustering (FCM), local convolutional features, wavelet-oriented representation and Transformer attention for irradiance forecasting. WOA can tune selected training hyperparameters.

The project includes chronological data preparation, model fitting, optional hyperparameter selection, evaluation metrics and graphical diagnostics. The architecture family and training parameters are configured in [config.json](config.json).

## Input data

The training script accepts a time-ordered multivariate CSV with a numeric `target` column. The target in this study is **solar irradiance**. Dataset columns and evaluation assumptions are discussed in [data/README.md](data/README.md). Source data must be obtained from the study's experimental dataset and should be kept distinct from the synthetic software check.

## Installation and execution

Requires Python 3.10 or newer. The default device is automatic: CUDA is used
when PyTorch detects it; otherwise training runs on CPU.

```bash
python -m venv .venv
# Linux/macOS:
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
pytest -q
python train.py --smoke --epochs 2 --out outputs/smoke
```

The smoke command uses generated data to check that the pipeline runs; it is
not a scientific experiment. For measured data, supply a CSV with at least
`max(80, lookback + 30)` chronologically ordered observations, a numeric
target, and optional numeric predictors:

```bash
python train.py --csv data/measurements.csv --target irradiance \
  --time-col timestamp --features temperature,humidity,cloud_cover \
  --epochs 40 --device auto --out outputs/experiment
```

Use `--device cpu` to force CPU or `--device cuda` to require CUDA. Training
uses the configured learning rate without search by default. To run the
repository's paper-inspired WOA search over learning rate and dropout, add
`--search paper --trials 5`; that search is an implementation choice, not a
verified reproduction of the article's full optimizer protocol.

The program saves training history, test predictions, error metrics, and a
fitted checkpoint under the chosen output directory. The CSV and the original
study dataset are not included; use data you are permitted to use and follow
its license.

## Evaluation notes

The repository does not include the manuscript's full equations, experimental
configuration, or source dataset. The implementation is therefore a runnable
research prototype, not a verified bit-for-bit reproduction. In particular,
FCM memberships are added as model inputs, the wavelet component is a fixed
Morlet filter bank, and WOA searches only learning rate and dropout. Confirm
these choices, the paper's data splits, features, forecast horizon, and metrics
against the article before making scientific comparisons.

The test interval is chronological. Normalization statistics must be estimated on training data, and all reported comparisons should specify the time span, forecast horizon, seed, target units and metric definitions. [REPRODUCIBILITY.md](REPRODUCIBILITY.md) records the implementation boundaries and outstanding experimental checks.
