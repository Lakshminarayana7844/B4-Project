# B4 Project - Performance loss on a training production line

Industrial AI for Manufacturing - Mini Project 1 (B4) - method: CRISP-DM.

One recorded batch from a training factory cell, logged by two PLC systems
(Siemens S7: oven, drill, turntable, sorting - about one row every 76 ms;
Beckhoff: warehouse, robot, operator screen - about one row every 93 ms).
Question: where does the line lose time - brief pauses, slow steps, or waiting
between stations?

## Hypotheses

| ID | Hypothesis |
|----|------------|
| L1 | Some machine steps take longer than usual |
| L2 | The robot's transfers cause waiting |
| L3 | Short pauses increase the time between completed parts |
| L4 | One station causes more delay than the others |

(Data-gap checks are kept as data-quality checks in Part 1, not as a hypothesis.)

## The project in four parts

| Part | Folder | What it does |
|------|--------|--------------|
| 1 | `part1_data_preparation/` | Load both logs, join them directly on a fixed 85 ms grid (last recorded value, never a future one), keep the source and age of every value, check sampling, gaps and the both-PLC view of the stops |
| 2 | `part2_rule_based_detection/` | Steps, stops and cycles measured on the original timestamps; the L1-L4 hypothesis tests |
| 3 | `part3_clustering_model/` | k-means over the joined table; long episodes are review windows, not faults |
| 4 | `part4_evaluation/` | Compare both methods, conclusions per hypothesis, final Excel report |

Each part is one notebook. **Run them in order** (1 -> 2 -> 3 -> 4): later parts
read the tables the earlier parts wrote into `results/`.

## How to run

```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter lab   # then open each part's notebook in order and Run All
```

## Repository layout

```
data/raw/         original files, never changed
data/processed/   the joined 85 ms table (written by Part 1)
part1_data_preparation/    01_data_preparation.ipynb
part2_rule_based_detection/ 02_rule_based_detection.ipynb
part3_clustering_model/    03_clustering_model.ipynb
part4_evaluation/          04_evaluation.ipynb
results/          all result tables, figures/ and performance_loss_findings.xlsx
```

## First results

- 4 stops, 31.8 s in total (6.3 % of the recording): 3 light-curtain stops
  and 1 operator-screen pause.
- 14 slow steps, 9 of them slow by themselves: warehouse belt 21.9 s lost,
  drill 5.1 s instead of 1.0 s, robot grip once just over the 2 s limit.
- 8 complete cycles, normal 53.4 s; the robot waited 6.0 s once (cycle 7),
  for the slow warehouse belt.
- The oven starts 14 to 27 s after the robot's release in 3 of 9 transfers
  (cause unknown - no "ready" signal in the dictionary).
- Clustering agrees: 5 of its 10 longest episodes overlap rule-based events.

## Assumptions and limits

- A step is slow if it is at least 25 % and at least 2 s longer than its
  median, with at least 5 repeats (own choice, agreed in the team).
- Durations are measured on the recorded timestamps, without grid rounding;
  the logs still sample every 76 / 93 ms, so borderline cases can flip.
- Cluster scores are weak (silhouette 0.15): clusters are coarse operating
  states, not confirmed events.
- The two PLC clocks are assumed to agree; this was never checked.
- No labelled ground truth: the findings rest on two different methods
  agreeing, not on proven accuracy.
- The recording is 8:25 although the documents say 10 minutes.
