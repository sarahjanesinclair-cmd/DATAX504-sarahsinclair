C1: NEXT-WINDOW EFFICIENCY LABEL

C1 predicts how efficiency will change in the next 30-second window of a run
(Base, Declining, Improving or Recovery) from the current window and the one
before it, using a GRU. The notebooks run in order. The test set is used only in
C1_09_Test_Evaluation, once.

REQUIREMENTS
- Python 3 with pandas, numpy, scikit-learn, matplotlib and TensorFlow (Keras)
- X_flat_c1_v2.npy and y_labels_c1_v2.csv, built by data_prep\Garmin_C1_DataBuild.ipynb
- r1_whole_run_labels.csv from R1\Garmin_R1_WholeRun_FullArchive.ipynb (C1_06 only)

HOW TO RUN
1. Set the data paths in the setup cell of each notebook (DATA_DIR, or X_PATH, Y_PATH and OUT_DIR in C1_08)
   Currently: C:\dev\UoW\DATAX504\Final Assignment\garmin_data\Processed
2. Run all cells from top to bottom.**Some notebooks take a long time to run
Every notebook splits by whole run with seed 42 (80% train and validation, 20% test; then 85/15
within the 80%), so TRAIN, VAL and TEST are the same in all of them.

FILE: C1_01_Baselines.ipynb
What it does: naive baseline, Random Forest and a Simple DNN (16-8).
Input: X_flat_c1_v2.npy and y_labels_c1_v2.csv (in DATA_DIR).
Output: none saved. Validation results are printed in the notebook.
Time: minutes.

FILE: C1_02_Overfit_Regularise.ipynb
What it does: a small, an oversized and a regularised dense network, to show overfitting.
Input: X_flat_c1_v2.npy and y_labels_c1_v2.csv (in DATA_DIR).
Output: none saved. Validation results, a loss-curve figure and a classification report are printed in the notebook.
Time: about 1 hour.

FILE: C1_03_Architecture_Ablation.ipynb
What it does: seven recurrent networks (GRU and LSTM, different widths and depths) on the current window.
Input: X_flat_c1_v2.npy and y_labels_c1_v2.csv (in DATA_DIR).
Output: c1_architecture_ablation_2026-10-04\ with results.csv (one row per run), and for each run a pred_<run>.npy (validation predictions) and a hist_<run>.json (loss history). The loss curves, the confusion matrix and the classification report are printed in the notebook.
Time: 1.5 to 3.5 hours per model, about 17.5 hours in all. Finished runs are listed in results.csv and skipped on re-running.

FILE: C1_04_Input_Ablation.ipynb
What it does: Random Forest and Simple DNN on four input sets (current window only, plus the previous window, plus two, plus the window two back only).
Input: X_flat_c1_v2.npy and y_labels_c1_v2.csv (in DATA_DIR).
Output: c1_context_test_results_2026-10-04.csv (in DATA_DIR), one row per input set. The bar chart is printed in the notebook, not saved.
Time: about 15 minutes.

FILE: C1_05_Dense_vs_GRU.ipynb
What it does: four dense networks (3 seeds each) and the GRU 64x3, with the previous window as input.
Input: X_flat_c1_v2.npy, y_labels_c1_v2.csv.
Output: c1_context_final_2026-10-04\ (results.csv, val_idx.npy, pred_*.npy, hist_*.json).
Time: dense networks minutes; the GRU about 7 hours. Finished runs are skipped.

FILE: C1_06_Workout_Type_Input.ipynb
What it does: adds the R1 workout type as an input to the Random Forest, Simple DNN and GRU.
Input: X_flat_c1_v2.npy, y_labels_c1_v2.csv, r1_whole_run_labels.csv.
Output: none saved; results are printed in the notebook.
Time: about 2 hours (the GRU).

FILE: C1_07_Class_Weighting.ipynb
What it does: dense 32-16 with balanced class weights, against the unweighted runs from C1_05.
Input: X_flat_c1_v2.npy, y_labels_c1_v2.csv, and val_idx.npy, results.csv and pred_dense_32-16_seed*.npy from c1_context_final_2026-10-04\.
Output: c1_class_weighting\ (pred_weighted_seed*.npy, comparison.csv).
Time: about 5 minutes.

FILE: C1_08_Final_Model.ipynb
What it does: retrains the chosen model (GRU 64x3 on the previous and current windows) and saves it.
Input: X_flat_c1_v2.npy, y_labels_c1_v2.csv.
Output: c1_final_model\ (gru_prev1_final.keras, scaler_and_classes.npz, val_idx.npy, val_pred.npy, history.json, run_record.json, gru_prev1_best.keras, a backup folder).
Time: about 7 hours. An interrupted run resumes from the backup.

FILE: C1_09_Test_Evaluation.ipynb
What it does: runs the saved model once on the held-out test runs, with the confusion matrix, per-class scores and a check of how well it predicts changes.
Input: X_flat_c1_v2.npy, y_labels_c1_v2.csv, and from c1_final_model\: gru_prev1_final.keras, scaler_and_classes.npz, val_idx.npy, run_record.json.
Output: c1_final_model\test_evaluation\ (test_pred.npy, test_idx.npy).
Time: minutes (saved predictions are loaded if they exist).

FILE: C1_11_Time_Trend.ipynb
What it does: the share of windows in each class by year, the model's predictions next to the labels (on the test runs), and the model's accuracy by year.
Input: y_labels_c1_v2.csv, c1_final_model\scaler_and_classes.npz, and test_idx.npy and test_pred.npy from c1_final_model\test_evaluation\.
Output: c1_time_trend\ (model_share_by_year.csv, actual_share_by_year.csv, accuracy_by_year.csv, share_by_year.png).
Time: seconds.


NOTES
- Data folders keep their dates in their names (for example c1_context_final_2026-10-04).
  The notebooks read them by name, so do not rename them.
- C1_06 joins the R1 labels to the windows on file_path, not run_idx.
- C1_03, C1_05 and C1_08 take hours. Their results are saved, and finished runs are skipped
  when re-running, so later notebooks do not need them repeated.
- C1_09 needs the saved model and files from C1_08. C1_11 needs the saved files from C1_08 and C1_09.