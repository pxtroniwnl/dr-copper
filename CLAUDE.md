# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**DR-COPPER** is an academic time-series analysis project for copper futures (HG=F) from the Universidad Tecnológica de Bolívar. It builds a hybrid forecasting pipeline comparing SARIMAX, Prophet, LSTM, and GARCH models, using UUP (Dollar Index ETF) as an exogenous variable.

## Execution Environment

All analysis lives in Jupyter Notebooks (`.ipynb`). The project runs on **Google Colab** (notebooks include a Colab badge and use `!pip install` cells). Package management locally uses `uv`.

To run notebooks locally:
```bash
uv run jupyter notebook
```

## Notebook Pipeline (5 Phases)

| File | Phase | Status |
|---|---|---|
| `dr_copper_eda.ipynb` (renamed target: `01_eda_y_diagnostico.ipynb`) | EDA, ADF/KPSS tests, STL decomposition, ACF/PACF, ARCH effects | Done |
| `02_data_prep.ipynb` | HG=F + UUP alignment, missing values, MinMaxScaler, Train/Test split | Done |
| `03_modelado_media.ipynb` | SARIMAX, Prophet, LSTM training and directional forecasting | In progress |
| `04_modelado_varianza.ipynb` | GARCH(1,1) on log returns for volatility clustering and VaR | Pending |
| `05_backtesting_y_eval.ipynb` | Out-of-time evaluation, Ljung-Box test on residuals | Pending |

## Stack

- **Data**: `yfinance` (tickers: `HG=F` for copper, `UUP` for dollar proxy)
- **Stats**: `statsmodels` (ADF, KPSS, ACF/PACF, decomposition), `pmdarima` (Auto-ARIMA), `arch` (GARCH)
- **ML/DL**: `prophet`, `tensorflow`/`keras` (LSTM)
- **Preprocessing**: `scikit-learn` (MinMaxScaler, RMSE/MAE/MAPE)
- **Viz**: `matplotlib`, `plotly`

## Critical Constraints

- **Validation**: Use only chronological splits (`TimeSeriesSplit` or out-of-time). K-Fold cross-validation is forbidden — it leaks future data.
- **GARCH input**: Must use **log returns** (`np.log(price/price.shift(1))`), not raw prices or simple differences.
- **LSTM scaling**: Fit `MinMaxScaler` only on the training set; apply the same scaler to the test set.
- **Baseline**: All models must be benchmarked against the Naïve model ($P_{t+1} = P_t$). A model is only justified if it reduces RMSE by ≥10% over Naïve.
- **Business days alignment**: When merging HG=F and UUP, align on business days and impute gaps before feature engineering.

## Success Metrics

1. RMSE ≥10% below Naïve for mean models (LSTM, SARIMAX, Prophet)
2. Ljung-Box p-value > 0.05 on model residuals (white noise)
3. GARCH correctly models the conditional heteroskedasticity confirmed in Phase 1
