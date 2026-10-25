# waterprophet

Code from a team entry to the Open City Innovation Challenge of Founders Foundation,
Bielefeld. The notebooks predict soil moisture at 5 cm depth from weather and soil
sensor data.

## Data

Hourly measurements from a single station (Ochtrup), June 2021 to November 2023,
loaded from CSV files (semicolon-separated, decimal comma) on Google Drive. After
dropping rows with missing values, 11,243 hourly rows remain. Columns include air
temperature, pressure, humidity, precipitation, wind, soil temperature at several
depths and soil moisture at several depths. The data is not part of this repository.

## Notebooks

Both are Colab notebooks (they mount Google Drive) and predict
`Bodenfeuchte in 5cm Tiefe [vol%]` with an 80/20 random train/test split.

- `Model_ohne_Zeitkomponente.ipynb`: the raw hourly values as features. Compares a
  linear regression (OLS summary, R-squared 0.38), a decision tree (depth 5), a random
  forest (100 trees, depth 5) and an Explainable Boosting Machine. Test mean squared
  error: EBM 14.2, random forest 28.6, decision tree 32.1, linear regression 38.3. The
  random forest ranks soil temperature at 30 cm depth as the most important feature
  (importance 0.63). A Spearman correlation heatmap is included.
- `Model_mit_Zeitkomponente.ipynb`: adds a time component by replacing every column
  with its 24-hour rolling mean (plus a 24-hour lag of the soil moisture mean), then
  fits an EBM and a depth-3 decision tree. The decision tree reaches a test MSE of 3.5.
  The target is itself a rolling mean of the same series, so this number is not
  comparable to the first notebook's results.

Deep-soil moisture, neutron count and matrix potential columns are dropped before
modelling in both notebooks.

## Status

Exploratory work from the challenge; no packaged model, no requirements file.
Dependencies used in the notebooks: pandas, scikit-learn, interpret, statsmodels,
seaborn, matplotlib.
