# Part 1: Data Preparation & Clock Synchronization

**Lead:** Neeraj Bhuma  
**CRISP-DM Phase:** Data Understanding & Data Preparation  

## Overview
This module ingests raw telemetry from two independent industrial controllers operating on a Fischertechnik modular production line:
- **Beckhoff CX5010 (TwinCAT):** High-speed event-driven logging (~20 ms resolution, 150 process variables).
- **Siemens S7-1500:** Cyclic fieldbus logging (~100 ms polling, 52 process variables).

## Contents
- **`01 data_prepration.ipynb`**: Complete data ingestion, clock offset identification (mean lag: 1.52 ms), and backward `merge_asof` alignment.
- **`merged_85ms.csv`**: Audited master dataset resampled onto a continuous 85 ms uniform grid (N = 5,945 time steps, 202 columns, zero missing values, zero look-ahead bias).
- **`Raw_Data/`**: Original unprocessed CSV and Excel signal description files.
