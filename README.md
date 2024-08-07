# LSTM_Forecast - Temperature Forecasting Using Long Short Term Memory Models

![cover](https://github.com/GabrielAlvesDS/CarPrice_Pro/blob/main/images/carprice%20pro_cut.jpg)

<br>

## Objective
Predict the air temperature for the next hour using different LSTM models, each leveraging different sets of historical data and features.

## Data Set
The data used in this project were extracted from the complete historical records of INMET for the region of Seropédica, in the state of Rio de Janeiro, Brazil, where EMBRAPA is located. This data set includes various climate measurements recorded hourly since 2002. The primary variable used for this project is 'TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C)'.

## Models Overview

### 1st Model: Basic LSTM

**Objective:** Predict the air temperature for the next hour using the temperatures from the last 24 hours.
- **Feature Used:** TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C)
- **Training Data:** 2002 to 2018
- **Testing Data:** 2019 to 2023

### 2nd Model: Day-Lagged LSTM

**Objective:** Predict the air temperature for the next hour using the temperatures from the last 24 hours.
- **Feature Used:** TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C)
- **Training Data:** 2002 to 2022
- **Testing Data:** 2023

### 3rd Model: Enhanced Feature LSTM

**Objective:** Predict the air temperature for the next hour using the temperatures from the last 24 hours.
- **Feature Used:**
  - TEMPERATURA DO AR - BULBO SECO, HORÁRIA (°C);
  - UMIDADE RELATIVA DO AR, HORÁRIA;
  - TEMPERATURA DO PONTO DE ORVALHO;
  - PRESSÃO ATMOSFÉRICA AO NÍVEL DA ESTAÇÃO, HORÁRIA;
  - RADIACÃO GLOBAL;
  - dayofyear;
  - VENTO, DIREÇÃO HORÁRIA;
  - VENTO, RAJADA MÁXIMA;
  - hour;
  - VENTO, VELOCIDADE HORÁRIA.
- **Training Data:** 2002 to 2022
- **Testing Data:** 2023

## Data Preprocessing
- **Standardizing Date and Time Format:** Ensured a consistent format for the date and time feature.
- **Handling Missing Values:** Identified intervals with missing temperature values and imputed them with the mean value.
- **Correcting Erroneous Values:** Detected instances where the temperature was incorrectly recorded as -9999. Applied the same imputation method used for missing values.
- **Feature Engineering:** Created new features based on the date: hour, dayofweek, quarter, month, year, dayofyear, dayofmonth, weekofyear, and season.

## Data Analysis
Conducted various analyses to better understand the data and identify key points for model development:

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

## Data Preparation:
- **Rescaling:** Applied MinMaxScaler to the target variable.
- **Feature Selection:** To prepare the data for the LSTM model, we created input sequences and labels. The function 'create_sequences' is responsible for generating these sequences, where each sequence consists of the temperatures from the last 24 hours (n_input = 24) and the label is the temperature of the next hour.

  
## Model training
The LSTM model was trained with the following steps:

- **Model Architecture:**
  - **LSTM Layer:** 50 units with ReLU activation.
  - **Dropout Layer:** Dropout rate of 0.2 to prevent overfitting.
  - **Dense Layer:** Single unit for the final output.

<br>

- **Model Compilation:**
  - **Optimizer:** Adam
  - **Loss Function:** Mean Squared Error (MSE)

<br>

- **Early Stopping:**
  - **Patience:** 10 epochs
  - **Validation Split:** 20%
  - **Best Weights:** The early stopping callback was used to restore the best weights.

<br>

- **Training Process:**
  - **Epochs:** Up to 50
  - **Batch Size:** 32
  - **Training and Validation Loss:**
    BasicLSTM
    
     ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/BasicLSTM - Output.png)
 
<br>

    Day-Lagged LSTM
    
     ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Day-Lagged LSTM - Output.png)    
 
<br>

    Enhanced Feature LSTM
    
     ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/Enhanced Feature LSTM - Output.png)

<br>

## Comparative Analysis
### Performance Metrics Comparison
 ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/FinalMetrics.png)

### Insights
  - **Basic LSTM:** Teve a melhor performance
  - **Day-Lagged LSTM:** Teve uma performance inferior devido a maior dificuldade em prever com base em dados mais afastados da previsão
  - **Enhanced Feature LSTM:** Showed improvements by incorporating additional meteorological features.

### Visualization

**OVERALL PERFORMANCE:** A plot comparing the true vs. predicted temperatures was created to visualize the model’s performance over the entire test period.
  - **Basic LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredictedComplete_BasicLSTM.png)
  As evident in the graph, the model successfully tracked temperature variations even when they deviated significantly from the averages.
  - **Day-Lagged LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredictedComplete_Day-Lagged LSTM.png)
  - **Enhanced Feature LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredictedComplete_Enhanced Feature LSTM.png)

<br>

**MONTHLY PERFORMANCE:** Focused plots for January 2019, January 2020, and January 2023.
  - **Basic LSTM**
    - **First Month of 2019:** A focused plot for the first month of 2019 was created to examine predictions in this specific period.
    ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012019_BasicLSTM.png)
    - **January 2020 and January 2023:** Scatter plots and line plots were used to compare predictions with true values for January 2020 and January 2023.
    ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012023_BasicLSTM.png)
    In both months, the model demonstrated efficiency in predicting the next hour's air temperature. This also indicates that predicting just the next hour is not as complex as expected.
  - **Day-Lagged LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012023_Day-Lagged LSTM.png)

  - **Enhanced Feature LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/TruevsPredicted012023_Enhanced Feature LSTM.png)

<br>

**RESIDUAL ANALYSIS:** A boxplot of residuals was plotted to examine the spread and outliers, for each model.
  - **Basic LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/BoxplotResidual_BasicLSTM.png)
  The above graph shows that very few predictions had an error greater than 5 degrees, either higher or lower.

  - **Day-Lagged LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/BoxplotResidual_Day-Lagged LSTM.png)

  - **Enhanced Feature LSTM**
  ![img6](https://github.com/GabrielAlvesDS/LSTM-TempForecast/blob/main/img/BoxplotResidual_Enhanced Feature LSTM.png)
  
## Conclusion
Os resultados indicam que o modelo **"Basic LSTM"** teve o melhor desempenho, com apenas 0.80 de RMSE, o que está diretamente ralacionado ao objetivo do modelo o qual utiliza as últimos 24 horas anteriores para prever a próxima hora, uma tarefa muito mais simples do que a dos demais modelos que utiliza as informações das temperaturas com um atraso de 1 dia. Também identificamos que comparando os modelos **"Day-Lagged LSTM	"** e **"Enhanced Feature LSTM"** houve uma melhora na performance ao acrescentar novas features, reduzindo o RMSE em x%.
 
