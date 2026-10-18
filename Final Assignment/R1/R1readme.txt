R1: WHOLE-RUN WORKOUT TYPE LABEL

R1 is a rule-based label that gives each run a whole-run workout type:
Z2/Steady State, Z2/Steady State with Strides, Tempo Run, Structured Intervals,
Structured Intervals with Strides.

The two notebooks use the same rules and are run separately.

REQUIREMENTS
- Python 3 with pandas, numpy, matplotlib and garmin-fit-sdk
- Running .fit files named DDMMYYYY__id.fit, in one folder


FILE: Garmin_R1_SegmentBased_Review.ipynb

What it does:
Labels a random sample of runs and plots a few from each label class, to check the
rules give sensible labels. Does not write any files.

Input:
.fit files in RUN_DIR. The sample is drawn from runs on or after MIN_DATE.

Output:
Label counts for the sample, and one plot per reviewed run shown in the notebook.

How to run:
1. Set RUN_DIR in the setup cell to the folder of .fit files.
   Currently: C:\dev\UoW\DATAX504\Final Assignment\garmin_data\DI_CONNECT\Sorted\Run
2. Optionally change these settings in the setup cell:
   POOL_SIZE    Number of runs picked at random from RUN_DIR and labelled (default 80)
   N_PER_CLASS  Number of runs plotted for each label (default 2)
   RANDOM_SEED  Fixed number so the same runs are picked each time. Change it to get a different sample (default 9307035)
   MIN_DATE     Only runs on or after this date can be picked, because recent runs have cleaner device data (default 2024-10-04)
3. Run all cells from top to bottom.


FILE: Garmin_R1_WholeRun_FullArchive.ipynb

What it does:
Labels every run in the archive.

Input:
.fit files in RUN_DIR.

Output (two CSV files in OUTPUT_DIR):
- r1_whole_run_labels.csv: one row per run with file_path, date, r1_whole_run_label
- r1_skipped_files.csv: files not labelled, with file_path, reason

How to run:
1. Set RUN_DIR and OUTPUT_DIR in the setup cell.
   Currently:
   RUN_DIR    C:\dev\UoW\DATAX504\Final Assignment\garmin_data\DI_CONNECT\Sorted\Run
   OUTPUT_DIR C:\dev\UoW\DATAX504\Final Assignment\garmin_data\Processed
2. Run all cells from top to bottom. Processing the full archive takes a while.


NOTES
- The thresholds are defined in the setup cell of each notebook. If you change one,
  change it in both.
- Join the labels to other files on file_path.