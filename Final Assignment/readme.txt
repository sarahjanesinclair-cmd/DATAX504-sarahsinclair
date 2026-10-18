DATAX504 FINAL PROJECT: PREDICTING RUNNING EFFICIENCY FROM GARMIN DATA

Sarah Sinclair (9307035)

This project predicts how efficiency (speed divided by heart rate) will
change in the next 30-second window of a run (Base, Declining, Improving
or Recovery) from my own Garmin sensor data, using a GRU.

FOLDERS
- data_prep: turns the sorted running .fit files into the model input (X)
  and the labels (Y). (see dataprep_readme.txt for full instructions)
- R1: a rule-based whole-run workout type label (five types). Only used in
  one rejected experiment, C1_06. (see R1readme.txt for full instructions)
- C1: the models. Baselines, ablations, final model, test evaluation and
  year trends. (see C1readme.txt for full instructions)

RUN ORDER
1. data_prep: builds X and Y.
2. R1: builds the workout type labels. Only needed for C1_06.
3. C1: run the notebooks in number order. The test set is used once, in C1_09.
See the readme in each folder for details.

DATA
The data is my personal Garmin Connect export and is not in this repo.
To run the C1 notebooks without rebuilding the data, download the
Processed folder (X, Y and the saved final model) from: [add link]

SETUP
- Python 3 with pandas, numpy, scikit-learn, matplotlib, TensorFlow (Keras)
  and garmin-fit-sdk
- File paths are set in the setup cell of each notebook. They currently
  point to C:\dev\UoW\DATAX504\Final Assignment\garmin_data\..., so change
  them to your own.