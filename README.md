# B4 Project - Performance Loss in a PLC Production Line

University mini-project by Neeraj Bhuma, Lakshmi Narayana Reddy Mandi and Naga Sai Priyanka. We analyse two PLC logs (a Beckhoff and a Siemens S7) recorded on a small Fischertechnik training line, about 8.5 minutes of production, and look for performance losses: slow machine steps, waiting between stations and short stops. We follow the CRISP-DM workflow and combine rule-based timing hypotheses with an Isolation Forest anomaly detection.

The work is split into three folders, one per person:

- **01_Neeraj_Bhuma_Data_Preparation_and_Synchronization** - Neeraj's part. The raw PLC logs and the signal dictionary (Raw_Data/), the data preparation notebook (01 data_prepration.ipynb), and the merged 85 ms dataset (merged_85ms.csv) that both analyses build on.
- **02_Lakshmi_Narayana_Reddy_Mandi_Hypothesis_Testing_and_Feature_Selection** - Lakshmi's part. Tests four timing hypotheses on the merged data (slow steps, robot transfer waiting, pauses between finished parts, and which station delays the most) and prepares the feature matrix for the anomaly detection. The README in that folder explains the files and the results.
- **03_Naga_Sai_Priyanka_Anomaly_Detection_and_Evidence_Integration** - Priyanka's part. Isolation Forest anomaly detection on the prepared features and matching the detected events against the hypothesis evidence. Files are still being added.
