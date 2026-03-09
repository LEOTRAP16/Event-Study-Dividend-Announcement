# Event Study: Dividend Decrease Announcements & Abnormal Returns

> Empirical finance project measuring the stock market reaction to dividend cut announcements using the market model and cumulative abnormal returns (CARs).

## Overview

This project implements a full **event study methodology** on US equity data to test whether dividend decrease announcements generate statistically significant abnormal returns. We follow the standard Brown & Warner (1985) framework: estimate normal returns using the market model over a pre-event estimation window, compute abnormal returns around the event date, and test for significance across multiple event windows.

## Research Question

*Do dividend cut announcements generate statistically significant abnormal returns, and over what horizon does the market react?*

## Dataset

- **Returns data:** CRSP daily stock returns (`daily_returns_new.dta`)
- **Events data:** Dividend decrease announcement dates (`dividend_events.csv`)
- **Coverage:** US equities identified by PERMNO — 1,662 firm-event observations
- **Note:** CRSP data requires a subscription. The code is fully replicable with any dataset matching the required format (columns: `PERMNO`, `date`, `DlyRet`, `DlyCap`).

## Methodology

**1. Estimation window:** (−250, −30) trading days relative to event date
- OLS market model: `R_it = α + β · R_mt + ε_it`
- Market return = equal-weighted average across all firms on each date
- Minimum 30 observations required per firm for estimation

**2. Abnormal returns:** `AR_it = R_it − (α̂ + β̂ · R_mt)`

**3. Event windows tested:**

| Window | Description |
|---|---|
| (−1, +1) | Immediate reaction |
| (−5, +5) | Short-term reaction |
| (−10, +10) | Medium-term reaction |

**4. Statistical tests:**
- Daily AR t-tests for each day in the event window
- CAR t-tests for each event window (clustered by event)
- CAAR plotted with 95% confidence intervals

## Results

### Daily Abnormal Returns (pre-event window)

| Day | Mean AR | t-stat |
|---|---|---|
| −5 | −0.049% | −0.48 |
| −4 | −0.091% | −0.68 |
| −3 | −0.023% | −0.18 |
| −2 | −0.104% | −0.83 |
| −1 | −0.141% | −1.25 |

Pre-event abnormal returns are small and insignificant, consistent with no anticipation of the dividend cut.

### CAR Summary Table

| Window | Mean CAAR | t-stat | p-value | Significance | N |
|---|---|---|---|---|---|
| [−1, +1] | **−1.90%** | −7.70 | 0.000 | *** | 1,662 |
| [−5, +5] | **−2.52%** | −7.87 | 0.000 | *** | 1,662 |
| [−10, +10] | **−2.52%** | −7.87 | 0.000 | *** | 1,662 |

### Key Findings

- Dividend cut announcements generate **strongly significant negative CARs** across all event windows
- The market reaction is concentrated around the announcement date: CAAR of −1.90% in the (−1,+1) window
- The effect does not grow beyond the (−5,+5) window, suggesting rapid price adjustment
- Pre-event ARs are statistically insignificant, supporting the absence of information leakage

## Repository Structure

```
├── Group_2_assignment_1.ipynb    # Full pipeline: data loading, market model, AR/CAR, t-tests, CAAR plot
├── daily_returns_new.dta         # CRSP daily returns (not included — requires CRSP subscription)
├── dividend_events.csv           # Dividend decrease announcement events
└── README.md
```

## Requirements

```
pandas
numpy
matplotlib
scipy
statsmodels
tqdm
```

Install with:
```bash
pip install pandas numpy matplotlib scipy statsmodels tqdm
```

## References

- Brown, S. J., & Warner, J. B. (1985). *Using daily stock returns: The case of event studies.* Journal of Financial Economics, 14(1), 3–31.

## Author

Leonardo Trapani — *Empirical Methods for Finance*, Erasmus School of Economics, 2025
