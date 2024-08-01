# LSTM_Forecast - Temperature Forecasting Long Short Term Model

![cover](https://github.com/GabrielAlvesDS/CarPrice_Pro/blob/main/images/carprice%20pro_cut.jpg)

<br>

## Objective
Predict the air temperature for the next hour based on the last 24 hours of historical data using an LSTM model.

## Data Set
The data used in this project were extracted from the complete historical records of INMET for the region of Seropédica, in the state of Rio de Janeiro, Brazil, where EMBRAPA is located. This data set includes various climate measurements recorded hourly since 2002. For this project, we exclusively used the variable 'TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C)', which provided a robust base for training the models. Below is a sample of the data used:

## Data Preprocessing
- **Standardizing Date and Time Format:** Ensured a consistent format for the date and time feature.
- **Handling Missing Values:** Identified intervals with missing temperature values and imputed them with the mean value. (Other approaches were considered but showed no significant impact on model performance, thus the simplest method with minimal processing load was chosen.)
- **Correcting Erroneous Values:** Detected instances where the temperature was incorrectly recorded as -9999. Applied the same imputation method used for missing values.
- **Feature Engineering:** Created new features based on the date: hour, dayofweek, quarter, month, year, dayofyear, dayofmonth, weekofyear, and season.
- **Data Analysis:** Conducted various analyses to better understand the data and identify key points for model development:
- **Temperature Distribution:** The air temperature distribution approximates a normal distribution (see image below).

  ![img1](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Distribution%20of%20Air%20Temperature.png)
  
<br>

- **Temperature Range:** 50% of recorded temperatures fall within a 5°C range, from 21°C to 26°C. Note that this analysis was impacted by the imputed mean values (see image below).

 ![img2](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Boxplot%20of%20Air%20Temperature.png)
 
<br>

- **Missing Data Impact:** Visualized the distribution by year to identify four significant gaps in 2002, 2004, 2010, and 2019 (see image below).

 ![img3](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Missing%20values.png)
 
<br>

- **Seasonal Influence:** Identified the impact of seasons on temperature variation. The highest median temperature was in autumn and the lowest in spring, while the highest and lowest recorded temperatures were in summer and winter, respectively (see image below).

 ![img4](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Season.png)
 
<br>

- **Daily Temperature Variation:** Analyzed temperature variation throughout the day (see image below).

![img5](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Hour.png)

<br>

- **Data Preparation:** Applied MinMaxScaler to the target variable.

- **Feature Selection:** To prepare the data for the LSTM model, we created input sequences and labels. The function create_sequences is responsible for generating these sequences, where each sequence consists of the temperatures from the last 24 hours (n_input = 24) and the label is the temperature of the next hour.

  
## Model training
The data was divided into training and testing sets, with the training set containing data from 2002 to 2018 and the testing set including 2019 to 2023.

The LSTM model was trained with the following steps:
- **Model Architecture:**
    - An LSTM layer with 50 units and ReLU activation was added.
    - A Dropout layer with a rate of 0.2 was included to prevent overfitting.
    - A Dense layer with a single unit was added to produce the final output.

<br>

- **Model Compilation:**
    - The model was compiled using the Adam optimizer and Mean Squared Error (MSE) loss function.

<br>

- **Early Stopping:**
    - Early stopping was implemented to monitor the validation loss with a patience of 10 epochs. This helps in preventing overfitting by stopping the training once the validation loss stops improving.

<br>

- **Model Training:**
    - The model was trained for up to 50 epochs with a batch size of 32.
    - The training data was split into training and validation sets with a validation split of 20%.
    - The early stopping callback was used to restore the best weights.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/LSTU-output.png)

## Model Evaluation

- **Overall Performance:** A plot comparing the true vs. predicted temperatures was created to visualize the model’s performance over the entire test period.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredictedComplete.png)

As evident in the graph, the model successfully tracked temperature variations even when they deviated significantly from the averages.

- **First Month of 2019:** A focused plot for the first month of 2019 was created to examine predictions in this specific period.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012019.png)

- **January 2020 and January 2023:** Scatter plots and line plots were used to compare predictions with true values for January 2020 and January 2023.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012023.png)

In both months, the model demonstrated efficiency in predicting the next hour's air temperature. This also indicates that predicting just the next hour is not as complex as expected.

- **Residual Analysis:** A boxplot of residuals was plotted to examine the spread and outliers.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/BoxplotResidual.png)

The above graph shows that very few predictions had an error greater than 5 degrees, either higher or lower.

<br>

 - **Performance Metrics:** The results below indicate that the model performed very well. Both the Mean Squared Error (MSE) and Root Mean Squared Error (RMSE) were below 1, specifically 0.80 and 0.89, respectively. This means that, on average, the model's air temperature predictions deviated by less than 1 degree Celsius from the actual values.

 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/FinalMetrics.png)

 
