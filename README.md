# Demand M5

**Research repository for retail demand forecasting on the M5 Walmart dataset.**

This repository documents the baseline, feature iteration, validation and
benchmark results behind **EventHorizon Demand**.

It is a research record. The production code lives in a separate repository:
[eventhorizon-demand](https://github.com/EventHorizon-ia/eventhorizon-demand).

---

## What This Repository Contains

This repository contains the research pipeline and evaluation artifacts for
two model versions:

| Model | Feature set | Training data | Evaluation period | Status |
|---|---|---|---|---|
| **Legacy model** | Includes `lag_365` | Before weekend-feature bug fix | 2015 onward | Historical reference |
| **Current MVP** | No `lag_365` | Through 2014 | 2015 onward | Current MVP result |

The current MVP removes `lag_365` to support use cases with shorter sales
history. It was retrained after a weekend-feature bug fix.

---

## Current MVP Result

| Metric | EventHorizon Demand MVP |
|---|---:|
| Overall WAPE | **31.03%** |
| Validation setup | Temporal holdout: train ≤ 2014, evaluate ≥ 2015 |
| Feature set | No `lag_365` |
| Baseline | Naive lag-7 |
| Store-level result | Better than baseline in 7/7 stores |
| Market validation | Ongoing |

### Store-level WAPE

| Store | EventHorizon Demand MVP | Naive lag-7 baseline | Paired WAPE gap |
|---|---:|---:|---:|
| CA_1 | 31.49% | 46.95% | 10.10–22.00 pp |
| CA_2 | 35.04% | 44.93% | 5.69–14.85 pp |
| CA_3 | 28.72% | 40.38% | 6.54–17.44 pp |
| TX_1 | 28.50% | 42.03% | 9.06–19.33 pp |
| TX_2 | 28.10% | 43.91% | 10.84–21.72 pp |
| WI_1 | 40.97% | 50.59% | 4.71–15.17 pp |
| WI_2 | 37.19% | 52.12% | 10.48–20.05 pp |

The intervals are 95% percentile intervals from a moving block bootstrap with
seven-day blocks and 1,000 resamples.

The paired gap is calculated as:

```text
gap = naive lag-7 WAPE - EventHorizon Demand WAPE
```

It is computed within each bootstrap resample, preserving the pairing between
model and baseline.

---

## Legacy Result

| Model | Overall WAPE | Feature set | Status |
|---|---:|---|---|
| Legacy model | 30.93% | Included `lag_365` | Historical reference |
| Current MVP | 31.03% | No `lag_365` | Current MVP |

The legacy model achieved 30.93% WAPE, but it was trained before a
weekend-feature bug fix and used `lag_365`.

It is retained as historical context only. It must not be interpreted as the
performance of the current MVP.

---

## Validation

### Temporal holdout

The main MVP result uses a temporal holdout split:

| Split | Period |
|---|---|
| Training | Through 2014 |
| Embargo | One day |
| Validation | 2015 onward |

The maximum forecast horizon is one day. The embargo prevents training labels
from using price information that overlaps the validation period.

### Annual walk-forward stability

To assess temporal stability, the model was retrained annually and evaluated
on the following year:

| Test year | Protocol |
|---|---|
| 2013 | Train on earlier years; evaluate on 2013 |
| 2014 | Train through 2013; evaluate on 2014 |
| 2015 | Train through 2014; evaluate on 2015 |
| 2016 | Train through 2015; evaluate on 2016 |

This expanding-window walk-forward protocol retrains the model for each test
year, using only information available before that year.

### Block bootstrap

Uncertainty was estimated with a moving block bootstrap:

- Seven-day blocks
- 1,000 resamples
- Date-based resampling
- Percentile intervals for model WAPE, baseline WAPE and paired WAPE gap

The paired gap preserves the comparison between EventHorizon Demand and the
naive lag-7 baseline within each bootstrap resample.

---

## Honest Limitations

- The main 31.03% WAPE result comes from a temporal holdout evaluation.
- The annual walk-forward analysis assesses temporal stability, but its yearly
  results should not be pooled as a single out-of-sample benchmark.
- Results are specific to the M5 Walmart benchmark.
- Technical accuracy does not establish business ROI.
- The trained model weights are not publicly distributed.

---




---

## Links

- [EventHorizon Demand — production code](https://github.com/EventHorizon-ia/eventhorizon-demand)
- [EventHorizon-AI Hub](https://github.com/EventHorizon-ia/EventHorizon)
- [Honest Validation Toolkit](https://github.com/EventHorizon-ia/honest-validation-toolkit)

---

*Demand M5 — transparent research, honest validation and documented
limitations.*
