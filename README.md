<h1 align="center">
Solar Irradiance Prediction Project</h1>

<div align="center">
  <a href="https://www.linkedin.com/in/eliatorre/">Elia Torre</a>,
  <a> Cyrill Albrecht</a>,
  <a> Martha Rösler</a>,
  <a href="https://www.linkedin.com/in/mario-baumgartner-101ba2168/"> Mario Baumgartner</a>
  <p><a href="https://www.ics.uzh.ch/en/">Institute for Computational Science </a>, University of Zurich, Zurich, Switzerland</p>
</div>

>**<p align="justify"> Abstract:** *This repository presents the project developed in the Introduction to Data Science course of University of Zurich. The purpose of the project was that of predicting the _solar radiation_ in the location of Zurich by regression of the climate factors. In addition, prediction of the same variable was investigated as the "quadrangulation" of climate data from neighboring cities. The project was based on five different data sets extracted from the NASA platform associated with the NASA POWER project. Leveraging classical Machine Learning, Deep Learning and Ensemble methods we were able to obtain accurate estimates of the _solar radiation_ in the city of Zurich as the rest of this repository illustrates.*

<hr/>

## Dataset
We created five different data sets from [NASA Platform](https://power.larc.nasa.gov/data-access-viewer/). The NASA POWER project combines data from different NASA research projects with the goal of an easily accessible data to study the climate. The five data sets refer to five different cities:
- Zurich [Latitude: 47.35, Longitude: 8.55]
- Milan [Latitude: 45.47, Longitude: 9.18]
- Dijon [Latitude: 47.32, Longitude: 5.06]
- Innsbruck [Latitude: 47.27, Longitude: 11.43]
- Karlsruhe [Latitude: 48.99, Longitude: 8.43]

Each data set consists of approximately 80k entries with 15 variables of which 9 hourly-measured features on a time-span of 10 years ranging from 01.01.2013 until 31.12.2022:
1. The radiance [W/m2]: The total solar irradiance incident (direct plus diffuse) on a horizontal plane at the surface of the earth under all sky conditions.
2. Temperature [C]: The average air (dry bulb) temperature at 2 meters above the surface of the earth in Fahrenheit.
3. Humidity [g/kg]: The ratio of the mass of water vapor to the total mass of air at 2 meters.
4. Precipitation [mm/h]: The total atmospheric water vapor contained in a vertical column of the atmosphere.
5. Wind speed [m/s]: The average wind speed at 10 meters above the surface of the earth.
6. Wind direction [◦]: The average wind direction at 10 meters above the surface of the earth.
7. Frost point [C]: The dew/frost point temperature at 2 meters above the surface of the earth.
8. Wet bulb temperature [C]: The adiabatic saturation temperature which can be measured by a thermometer covered in a water-soaked cloth over which air is passed at 2 meters above the surface of the earth.
9. Surface Pressure [kPa]: The average surface pressure at the surface of the earth.

And additional variables such as:

10. Timestamps: year, month, day, hour.
11. Time of sunrise.
12. Time of sunset.

<hr/>

## Research Questions
- What relations exist in our data set?
- Can we develop an accurate machine learning model to predict solar radiation using our data?
- Is it possible to estimate solar radiation in Zurich by averaging over four neighboring locations?

<hr/>

## Data Cleaning & Augmentation
- Limit data set to items till 31. December 2021
- Add sunrise and sunset time
- Make a binary variable light
- Check for NaN values

<hr/>

## Prediction Pipeline
1. Dataset splitting with 5-Kfold Cross-Validation
2. Initial Models Evaluation
3. Hyperparameter Tuning of Models
4. Training of Tuned Models
5. Ensemble of Best Performing Models
6. Neural Networks Architectures Training & Hyperparameters Optimization
7. Tuned Models Evaluation

<hr/>

## Models
- Linear Regression
- K-Nearest Neighbor Regression (KNN)
- Decision Tree Regression
- Random Forest Regression (RF)
- Extreme Gradient Boosting (XGB)
- AdaBoost (ADB)
- CatBoost
- LightGBM (LGBM)
- Voting Regressor (rf, xgb, adb)
- Stacking Regressor (Ridge) (rf, xgb, adb)
- Stacking Regressor (ElasticNet) (rf, xgb, adb)
- Feed-Forward Neural Network (FFNN)
- Long Short-Term Memory Neural Network (LSTM)

<hr/>

## Hyperparameter Tuning
- Machine Learning Models:
  - RandomizedSearchCV (5-Folds)
  - 20 Iterations (approx. 3h per model)
  - Negative Mean Squared Error
- Deep Learning Models:
  - Feed-Forward Neural Network (FFNN):
    - Min-Max Scaling
    - 6 FC Layers
    - ELU Activation
    - ADAM Optimizer
    - 32 Batch Size
    - 100 Epochs
    - Approx. 40 mins on NVIDIA T4
  - Long Short-Term Memory Neural Network (LSTM):
    - 6 LSTM Layers
    - 1 Dense Layer
    - ReLU Activation
    - ADAM Optimizer
    - 32 Batch Size
    - 40 Epochs
    - Approx. 30 mins on NVIDIA T4
   
<hr/>

## Results
<div align="center">
<img src="img/results_hist.png" alt="Results Histogram" width="40%">
</div>

<div align="center">
<img src="img/results_table.png" alt="Results Table" width="40%">
</div>

<hr/>

## "Quadrangulation" of Zürich Location
We aim to predict the ```IRRADIANCE``` in Zurich as an ensemble of 4 locations data. 

<div align="center">
<img src="img/locations.png" alt="Quadrangulation" width="40%">
</div>

- 3 Different Techniques:
  - **Averaging Measured Solar Radiation of the 4 Cities**: $\text{MSE}(\frac{\text{Dijon} + \text{Milan} + \text{Karlsruhe} + \text{Innsbruck}}{4}) = 1979.2$
  - **Weighted Averaging Measured Solar Radiation of the 4 Cities**: $\text{MSE}(\frac{\alpha * \text{Dijon} + \beta * \text{Milan} + \delta * \text{Karlsruhe} + \gamma * \text{Innsbruck}}{4}) = 1888.75$

    Where the coefficients (i.e., $\alpha, \beta, \delta, \gamma)$ are defined as the normalized mean correlations over the data sets features between each city and Zürich.

  - **Custom Data Set**
    - We create a custom data set by concatenating the features from the 4 cities.
    - Features: ```[TEMPERATURE]```, ```[HUMIDITY]```, ```[PRECIPITATION]```, ```[PRESSURE]```, ```[WIND SPEED]```, ```[WIND DIRECTION]```, ```[DEW]```, ```[WET BULB TEMPERATURE]```, ```[LIGHT]```, ```[IRRADIANCE]```.
    - Data set shape: 78888 entries x 45 features.
    - 3-Fold Cross Validation with the same models used before (no retuning).
    - Results:

<div align="center">
<img src="img/custom.png" alt="Custom Data Set Results" width="40%">
</div>

<hr/>

## Contact
Feel free to e-mail etorre@student.ethz.ch.
