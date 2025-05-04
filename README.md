# Scoring-project

Our central goal was to predict default as good as possible. We compare different methods as outlined in the code. The code an report is contained in ´Code/Project - Scoring_models´.

## Overview

> **Goal** Predict corporate default 1‑year ahead using WRDS Compustat‑CRSP fundamentals & market data.  
> **Sample** 1978‑2023, 626 k firm‑years, 0.17 % bankruptcies (highly imbalanced).  
> **Split** 70 % train / 30 % test + 5‑fold *time‑series* CV.  
> **Metrics** AUC, balanced accuracy, confusion matrix.

| Model (hold‑out) | Class‑weighting | AUC | FN | FP |
|------------------|-----------------|-----|----|----|
| Logistic (Altman ratios) | – | **0.716** | 77 | 12 542 |
| Logistic (Altman) | ✓ | **0.8922** | 14 | 3 733 |
| LASSO Logistic | – | **0.8133** | 24 | 9 964 |
| LASSO Logistic | ✓ | **0.9041** | 25 | 7 887 |
| Random Forest | – | **0.9401** | **11** | 3 941 |
| Random Forest | ✓ | ~0.94 | 10 | 3 600 |
| Altman (time‑series CV) | ✓ | **0.6125** | 88 | 14 308 |
| Transformed Altman | – | **0.8581** | 29 | 6 787 |
| Transformed Altman (ts‑CV) | – | **0.5363** | 81 | 13 902 |
| Distance‑to‑Default (Schumway) | – | **0.5032** | 97 | 16 021 |
| Iterative DtD (Merton) | – | **0.5109** | 92 | 15 784 |

### Key take‑aways

* **Class imbalance matters** Weighting observations improves prediction performance
* **Ensemble trees rule** Random Forest captures ratio interactions & macro shocks; top features: *EBIT/TA*, *RE/TA*, *σ‑equity*, *market‑beta*.    
* **Structural Merton models** struggle on accounting data alone—market volatility is not enough.

Repo structure
```
├── Code/         # R code & notebooks (Quarto .qmd)
└── data/         # Lopucki bankruptcy data
```

> Reproduce with `make all` (requires R 4.3, `tidyverse`, `glmnet`, `randomForest`, `pROC`).

Note: This is an extended analysis from the group work I did as part of our Scoring course. 

## Data
Data comes from WRDS connection: CRSP and Compustat. These files were merged to create the data.
