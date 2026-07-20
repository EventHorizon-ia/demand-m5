# Demand M5

**Research notebook for retail demand forecasting.**
Baseline, feature iteration, and final validated model on the M5 Walmart dataset.

---

## What This Is

This repository contains the complete research pipeline that delivered a
**30 % improvement over the seasonal baseline** on the M5 Forecasting
competition dataset. It is a historical record — the production code lives
in a separate repository.

---

## Key Findings

| Question | Answer |
|----------|--------|
| Is the model better than baseline? | ✅ Yes — WAPE 30.9 % vs 44.4 % (seasonal lag‑7 baseline). |
| Is the improvement statistically significant? | ✅ Yes — confirmed by block bootstrap by date and paired gap test. |
| Does it generalize across stores? | ✅ 7/7 stores show significant improvement. WI_1 has a smaller margin (documented). |
| Were there overfitting risks? | ✅ Checked — rolling features use shift(1), store‑level grouping audited. |

---

## Validation Pipeline

The same rigorous pipeline used across all EventHorizon products:

- Walk‑forward analysis by year (2013‑2016)
- Block bootstrap by date (respects cross‑store correlation)
- Paired gap bootstrap (model vs. baseline on same days)
- Per‑store confidence intervals
- Metrics: WAPE, MAE, improvement over baseline

This pipeline is extracted into a standalone library:
**[honest‑validation‑toolkit](https://github.com/EventHorizon-ia/honest-validation-toolkit)**.

---

## Repository Contents
 
demanda-m5/        
metricas_finais.json # Final metrics (WAPE, ICs, per-store results)     
README.md     

---

## Links

- [Production Code](https://github.com/EventHorizon-ia/eventhorizon-demand)
- [Main Hub](https://github.com/EventHorizon-ia/eventhorizon)

---

*Demand M5 — 30 % better than guessing. Rigorously proven.*
