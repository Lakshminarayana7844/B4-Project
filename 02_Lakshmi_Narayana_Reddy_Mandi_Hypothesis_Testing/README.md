# Part 2: Hypothesis Testing

**Lead:** Lakshmi Narayana Reddy Mandi  
**CRISP-DM Phase:** Business Understanding & Exploratory Data Analysis  

## Overview
Formulates and quantitatively tests four rule-based timing hypotheses on the synchronized 85 ms production dataset to detect baseline cycle irregularities and performance losses.

## Contents
- **`02 Hypothese_testing.ipynb`**: Analysis notebook testing H1 through H4.
  - **H1 (Slow Step Durations):** Supported. 13 of 487 analyzed machine step activations exceeded their benchmark median duration by >= 25% and >= 2.0 seconds.
  - **H2 (Robot Transfer Waiting):** Not confirmed. Vacuum gripper handoffs occur sequentially without systematic buffering idle times.
  - **H3 (Inter-part Bottleneck Gaps):** Not confirmed. Sorting pusher intervals show regular spacing, with 3 long gaps corresponding to production pauses.
  - **H4 (Station Delay Contribution):** High Bay Warehouse exhibits the highest cumulative excess duration (~29.8 s).
- **`b4_h3_gaps.csv`**: Measured inter-part intervals across sorting station pushers.
