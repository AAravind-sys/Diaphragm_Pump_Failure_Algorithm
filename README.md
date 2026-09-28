# AODD Pump Condition Monitoring: FFT and Entropy Analysis

Analysis notebooks for an air-operated double diaphragm (AODD) pump test setup. The pump is a pulsating machine, so the raw pressure signals are converted into frequency spectra and the energy in the failure-related frequency band is followed over running hours.

The data came from bench tests with diaphragm cracks introduced on purpose, logged on two pumps (P1 and P2).

## What the project does

1. Reads the DAQ log files (`.tdms`) and builds a proper timestamp for every sample.
2. Removes duplicate timestamps and infinite values, then stores raw and cleaned data in MySQL.
3. Keeps only the samples where the pump is actually running (air supply pressure above 93).
4. Cuts the signal into time windows (5 minutes or 1 hour), removes the mean and runs an FFT on each window.
5. Saves the spectrum plots and a frequency/amplitude table for each run.
6. Calculates Shannon entropy of the air supply pressure for each window as a second health indicator.

Block diagrams and flow charts are in [docs/architecture.md](docs/architecture.md).


## Signals used

Air supply pressure, water suction pressure, water discharge pressure, water suction and discharge flow rate, air supply flow rate, and cycles per minute (CPM). The FFT notebooks work on air supply pressure.

## Analysis settings

- Sampling interval: 30 ms (about 33.3 Hz), so the highest frequency that can be seen is about 16.7 Hz.
- Frequency resolution is 1 divided by the window length: about 0.0033 Hz for a 5-minute window and about 0.00028 Hz for a 1-hour window.
- Amplitude in the plots and CSV files is the raw FFT magnitude, not divided by the number of samples. Compare windows of the same length only.
- Shannon entropy is calculated on the distribution of pressure values inside each window, base 2.

## Requirements

Python 3.10

```
pip install numpy pandas scipy matplotlib seaborn statsmodels scikit-learn nptdms sqlalchemy mysql-connector-python
```

MySQL is only needed for notebook 01.

## Running it

The notebooks read from local folders. Change the path variables in the first cells to your own folders before running:

- `tdms_path` in notebook 01 for the TDMS files
- the CSV folder and output folder in notebooks 03 and 04
- the database user, password and database names in notebook 01

Run 01 first, export the cleaned table to CSV, then run 03 and 04 on those CSV files.

## Data

No measurement data is included in this repository. The TDMS logs and CSV exports are not published.

## Author

[Your name]
