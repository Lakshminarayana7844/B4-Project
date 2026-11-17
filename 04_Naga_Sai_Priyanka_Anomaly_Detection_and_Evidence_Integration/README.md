# Part 4: Anomaly Evidence Integration & Verification

**Lead:** Naga Sai Priyanka Truvidhula  
**CRISP-DM Phase:** Evaluation & Deployment  

## Overview
Validates detected unsupervised anomaly events against ground-truth PLC operational states, machine stop signals, and domain timing evidence to identify confirmed performance losses.

## Contents
- **`05 Hypothese on isolationm.ipynb`**: Multi-source validation workflow integrating model outputs, step delays (H1), and conveyor running signals (A0.0).
- **`b4_event_evidence_table.csv`**: Multi-criteria evidence matrix linking each candidate event with overlapping sensor flags.
- **`b4_h1_per_event_flagged.csv`**: Granular list of H1-flagged activations overlapping anomaly events.
- **`b4_h1_per_event_summary.csv`**: Aggregated extra seconds and flagged counts per event.
- **`b4_h4_station_per_event.csv`**: Physical workstation breakdown per anomaly event.
- **`final_8_reported_performance_losses.csv`**: Final audited table of the 8 verified performance loss events totaling 28.560 seconds of loss.
