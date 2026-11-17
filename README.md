# Detection and Analysis of Performance Loss in a PLC-Controlled Production System Using Unsupervised Learning

**Mini Project 1 · Group A**  
**Hochschule Albstadt-Sigmaringen**  
**Course:** Applied AI in Production & Automation  
**Supervising Professor:** Prof. Dr.-Ing. Felix Georg Müller  

---

## 👥 Team Members & Responsibilities (4-Part Division)

The project follows the standard **CRISP-DM** (Cross-Industry Standard Process for Data Mining) methodology, divided into four dedicated, modular packages:

1. **`01_Neeraj_Bhuma_Data_Preparation_and_Synchronization`**
   - **Lead:** Neeraj Bhuma
   - **CRISP-DM Phase:** Data Understanding & Data Preparation
   - **Key Deliverables:** Ingestion of heterogeneous Beckhoff (TwinCAT, ~20 ms, event-driven) and Siemens (S7, ~100 ms cyclic) logs, clock offset correction (1.52 ms mean lag), backward `merge_asof` synchronization to an audited 85 ms uniform grid (5,945 rows × 202 columns, 0% missing values, 0 future leakage).

2. **`02_Lakshmi_Narayana_Reddy_Mandi_Hypothesis_Testing`**
   - **Lead:** Lakshmi Narayana Reddy Mandi
   - **CRISP-DM Phase:** Business Understanding & Exploratory Data Analysis
   - **Key Deliverables:** Formulation and quantitative validation of 4 domain-driven performance hypotheses:
     - **H1 (Slow Step Durations):** Supported (13 of 487 steps flagged at >= 25% delay and >= 2.0 s extra duration).
     - **H2 (Robot Transfer Latency):** Not confirmed (transfers strictly follow cycle progression without systematic waiting).
     - **H3 (Inter-part Bottleneck Gaps):** Not confirmed (pushing intervals consistent, 3 long pauses identified).
     - **H4 (Station Attribution):** High Bay Warehouse carries the largest flagged excess duration (~29.8 s).

3. **`03_Bala_Sai_Kiran_Feature_Selection_and_Isolation_Forest`**
   - **Lead:** Bala Sai Kiran
   - **CRISP-DM Phase:** Data Preparation & Modeling
   - **Key Deliverables:** 
     - **Feature Selection:** Audited screening of 195 candidate signals; exclusion of 40 zero-variance constants and 3 redundant communication mirror pairs (A200.4 -> A0.2, A200.5 -> A0.3, A200.6 -> A0.4); one-hot encoding of 7 categorical state codes into binary indicators; Min-Max normalization of 5 numeric counters/sensors; yielding a 172-dimensional feature matrix.
     - **Unsupervised Modeling:** Hyperparameter grid search and robust **40-forest seed-averaged Isolation Forest** (100 trees each, subsample 256, score threshold -0.50, 5.0 s maximum gap merging), identifying 13 candidate anomaly intervals.

4. **`04_Naga_Sai_Priyanka_Anomaly_Detection_and_Evidence_Integration`**
   - **Lead:** Naga Sai Priyanka Truvidhula
   - **CRISP-DM Phase:** Evaluation & Deployment
   - **Key Deliverables:** Evidence fusion and multi-source cross-validation across PLC operational states:
     - Filtering out initial ramp-up/homing artifacts (Event 1) and isolated non-impact sensor flickers.
     - Correlating machine stops (A0.0 running signal dropouts) and H1 step duration delays with anomaly intervals.
     - Consolidating the final **8 verified performance loss events** (totaling **28.560 seconds** of operational performance loss).

---

## 📁 Repository Structure

```text
B4-Project/
├── .gitignore
├── README.md
├── B4_Technical_Report.docx
├── B4_Presentation.pptx
├── 01_Neeraj_Bhuma_Data_Preparation_and_Synchronization/
│   ├── 01 data_prepration.ipynb
│   ├── merged_85ms.csv
│   ├── Raw_Data/
│   │   ├── Beispieldatensatz Beckhoff SPS.csv
│   │   ├── Beispieldatensatz Siemens S7 SPS.csv
│   │   └── Variablenerkl_rung.xlsx
│   └── README.md
├── 02_Lakshmi_Narayana_Reddy_Mandi_Hypothesis_Testing/
│   ├── 02 Hypothese_testing.ipynb
│   ├── b4_h3_gaps.csv
│   └── README.md
├── 03_Bala_Sai_Kiran_Feature_Selection_and_Isolation_Forest/
│   ├── 03 feature_selection.ipynb
│   ├── 04 Isolation forest.ipynb
│   ├── feature_exclusion_review.txt
│   ├── b4_if_tuning_results.csv
│   ├── b4_if_score_avg40.csv
│   ├── b4_if_intervals_avg.csv
│   ├── b4_if_events_gap5.csv
│   └── README.md
└── 04_Naga_Sai_Priyanka_Anomaly_Detection_and_Evidence_Integration/
    ├── 05 Hypothese on isolationm.ipynb
    ├── b4_event_evidence_table.csv
    ├── b4_h1_per_event_flagged.csv
    ├── b4_h1_per_event_summary.csv
    ├── b4_h4_station_per_event.csv
    ├── final_8_reported_performance_losses.csv
    └── README.md
```

---

## 📊 Summary of Final Verified Performance Losses (8 Events)

| Event | Start UTC | End UTC | Duration (s) | Station Attribution | Physical Evidence / Operational Impact |
| :---: | :---: | :---: | :---: | :--- | :--- |
| **#2** | 18:21:46.305 | 18:21:47.495 | 1.190 | Vacuum Gripper Robot | Robot positioning pause during workpiece pick-and-place. |
| **#3** | 18:22:57.450 | 18:22:58.895 | 1.445 | High Bay Warehouse | Carriage vertical axis hesitation / transfer delay. |
| **#4** | 18:23:18.190 | 18:23:24.055 | 5.865 | High Bay Warehouse | **Conveyor stop (5.015 s)**; A0.0 dropped; step delayed +3.570 s. |
| **#5** | 18:24:16.245 | 18:24:18.030 | 1.785 | Multi-Processing Station | Oven door / turntable index transition delay (+2.805 s). |
| **#6** | 18:25:22.715 | 18:25:25.180 | 2.465 | High Bay Warehouse | Storage shelf retrieval cycle extension (+3.485 s). |
| **#7** | 18:25:31.980 | 18:25:38.695 | 6.715 | High Bay Warehouse | **Line stoppage (6.630 s)**; full carrier stall; step delayed +3.655 s. |
| **#9** | 18:26:58.170 | 18:26:59.530 | 1.360 | High Bay Warehouse | Rack feeder axis repositioning pause. |
| **#10** | 18:27:20.100 | 18:27:25.965 | 5.865 | High Bay Warehouse | **Line stoppage (5.015 s)**; carrier stalled; step delayed +3.740 s. |
| **Total** | | | **28.560 s** | | **3 physical line stops, 5 micro-stops/slowdowns** |

---

## 🛠️ Reproduction & Requirements

All notebooks contain self-contained dynamic path resolution routines. They can be executed independently either from the repository root or from within their specific subfolder.

```bash
pip install numpy pandas scikit-learn openpyxl
```
