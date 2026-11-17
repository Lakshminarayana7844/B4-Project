# Hypothesis Testing and Feature Selection

My part of the B4 mini-project. We look for performance losses in two PLC logs (a Beckhoff and a Siemens S7) recorded on a small Fischertechnik training line, about 8.5 minutes of production. Neeraj prepared and merged the data onto a common 85 ms grid, I tested the timing hypotheses and prepared the features, and Priyanka ran the Isolation Forest anomaly detection.

## What is in this folder

- **02 Hypothese_testing.ipynb** - tests four timing hypotheses on the merged data:
  - H1: some machine steps take longer than usual - supported. 13 of 487 compared steps were 25%+ slower than their own median and at least 2 s longer.
  - H2: robot transfers cause waiting - not confirmed, though a few long waits were observed.
  - H3: short pauses increase the time between finished parts - not confirmed, but 3 long gaps were found.
  - H4: one station causes more delay than the others - the warehouse carries the largest flagged extra time (about 29.8 s), so it is the first place to investigate.
- **03 feature_selection.ipynb** - audits the 195 process signals, removes 43 constant or duplicated ones, one-hot encodes the state-code columns and scales the 5 numeric ones. The result is a clean 5,945 x 172 matrix used by the anomaly detection.
- **b4_h3_gaps.csv** - the measured gaps between sorting pusher starts behind H3.
- **feature_exclusion_review.txt** - the 43 excluded signals with the reason for each.
- **B4_Technical_Report.docx** (also as PDF) - the full team report.
- **B4_Presentation.pptx** - our presentation of the whole project.

All flagged steps and events are candidates to investigate, not proven faults.
