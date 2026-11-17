# Part 3: Feature Selection & Isolation Forest Modeling

**Lead:** Bala Sai Kiran  
**CRISP-DM Phase:** Data Preparation & Modeling  

## Overview
Prepares the machine learning feature space through rigorous signal screening and implements an unsupervised ensemble anomaly detection model.

## Contents
- **`03 feature_selection.ipynb`**: Audits 195 candidate process variables:
  - Excludes 40 constant signals (zero variance during the recording).
  - Excludes 3 communication mirror duplicates (A200.4 -> A0.2, A200.5 -> A0.3, A200.6 -> A0.4).
  - One-hot encodes 7 categorical state codes into binary indicators.
  - Min-Max normalizes 5 numeric sensor/counter channels.
  - Yields an audited 5,945 × 172 modeling matrix.
- **`04 Isolation forest.ipynb`**: Unsupervised anomaly detection:
  - Hyperparameter grid evaluation across tree counts, contamination levels, and subsampling.
  - **40-Forest Seed-Averaged Ensemble:** Eliminates isolation tree stochasticity by averaging anomaly scores over 40 distinct random seeds (100 trees each, subsample 256).
  - Score threshold at -0.50 with a 5.0-second gap-closing window, producing 13 candidate anomaly intervals.
- **`feature_exclusion_review.txt`**: Documented rationale for each of the 43 excluded signals.
- **`b4_if_tuning_results.csv`**: Parameter tuning grid logs.
- **`b4_if_score_avg40.csv`**: Seed-averaged anomaly score time series.
- **`b4_if_intervals_avg.csv`**: Contiguous candidate anomaly intervals.
- **`b4_if_events_gap5.csv`**: 13 consolidated candidate anomaly events.
