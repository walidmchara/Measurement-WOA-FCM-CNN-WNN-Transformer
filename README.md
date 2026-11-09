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

```bash
python -m venv .venv
pip install -r requirements.txt
pytest -q
python train.py --smoke --epochs 2 --out outputs/smoke
```

With measured time-series data:

```bash
python train.py --csv path/to/measurements.csv --target target --time-col time --epochs 40 --out outputs/experiment
```

The program saves training history, test predictions, error metrics, and a fitted checkpoint under the chosen output directory. Optional parameter search is supported through `--search paper --trials N`, subject to the method-specific qualifications below.

## Evaluation notes

The optimization space, full model configuration and experimental partitions must be checked against the article before direct numerical comparison.

The test interval is chronological. Normalization statistics must be estimated on training data, and all reported comparisons should specify the time span, forecast horizon, seed, target units and metric definitions. [REPRODUCIBILITY.md](REPRODUCIBILITY.md) records the implementation boundaries and outstanding experimental checks.
