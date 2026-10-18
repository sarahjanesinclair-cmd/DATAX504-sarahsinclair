DATA_PREP: FROM THE SORTED RUNNING FILES TO THE C1 MODEL INPUTS

Garmin_C1_DataBuild.ipynb turns the sorted running files from the Garmin Connect export
into the two files every C1 model uses: X_flat_c1_v2.npy (the model input) and
y_labels_c1_v2.csv (the label of the next 30-second window).

REQUIREMENTS
- Python 3 with pandas, numpy and garmin-fit-sdk
- The sorted running .fit files, named DDMMYYYY__activityId.fit, in one folder
  (garmin_data\DI_CONNECT\Sorted\Run, 1,891 files)

HOW TO RUN
1. Set RUN_DIR and OUT_DIR in the setup cell.
   Currently:
   RUN_DIR  C:\dev\UoW\DATAX504\Final Assignment\garmin_data\DI_CONNECT\Sorted\Run
   OUT_DIR  C:\dev\UoW\DATAX504\Final Assignment\garmin_data\Processed
2. Run all cells from top to bottom.
3. The notebook writes X_flat_c1_v2_rebuild.npy and y_labels_c1_v2_rebuild.csv. Rename them to
   X_flat_c1_v2.npy and y_labels_c1_v2.csv (remove "_rebuild"), which are the names the C1
   notebooks read.

FILE: Garmin_C1_DataBuild.ipynb
What it does: reads each running file once and builds the model input (X) and the labels (Y).
Input: the running files in RUN_DIR.
Output (in OUT_DIR):
- X_flat_c1_v2_rebuild.npy: one row per 30-second window, 121 numbers
- y_labels_c1_v2_rebuild.csv: one row per window, in the same order: run_idx, date, file_path, start_s, end_s, c1_target
Time: [confirm].