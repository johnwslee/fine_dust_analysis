![license
status](https://img.shields.io/github/license/johnwslee/fine_dust_analysis)

# Study on the Relationship between Weather Parameters and Fine Dust Concentration in Seoul, South Korea

**Author:** John W.S. Lee

## 1. Introduction

In Seoul, South Korea, there are certain seasons when the air quality gets really bad because of the high concentration of fine dusts. The concentration of fine dusts are measured in terms of the density of fine particles, and based on the size of the particle, there are two metrics: (1) PM10 - density of particulate matter less than 10um, and (2) PM2.5 -  density of particulate matter less than 2.5um (Please note that the density of PM2.5 was written as PM25 in the notebooks since the dot can cause unwanted results). It was one day during winter in 2023 when the air quality was really  bad. I just got curious what kind of weather parameters affect the concentration of fine dust, and that was the sole motivation of this study. The detailed procedure and codes can be found in the [notebook folder](https://github.com/johnwslee/fine_dust_analysis/tree/main/notebooks), whereas this README shows the overall approaches and some of the interesting results that I found.
Before I move on to the next section, I just want to say that the purpose of using the machine learning in this study was not to predict the concentration of the fine dust, it was rather to take a look at the features that the models thought important, thereby fulfilling my curiosity.

## 2. Data

### 2.1. Fine Dust Dataset

The fine dust data was downloaded from [Public Data Portal](https://www.data.go.kr/data/15089266/fileData.do). The data contains hourly data for fine dust measured in many different regions in Seoul, for the period of 2008 to 2021. For this study, I extracted the average values of the fine dust for each time.

### 2.2. Weather Dataset

Weather data was downloaded from [Open MET Data Portal](https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36&tabNo=1) by KMA (Korea Meteorological Administration) Weather Data Service. The original dataset had 27 columns, but only 11 columns that seem relevant were used in this study. The chosen weather parameters were `date`, `temp(°C)`, `precipitation(mm)`, `wind_speed(m/s)`, `wind_direction`, `humidity(%)`, `vapor_P(hPa)`, `dew_point_temp(°C)`, `local_P(hPa)`, `cloud_cover`, and `lowest_ceiling(100m)`.

### 2.3. Exploratory Data Analysis

#### 2.3.1. Fine Dust Concentratio over Years

The following figure shows how the fine dust concentration has been fluctuating since the year of 2008. The figure showed both particle counts had seasonality to a certain degree, and they seemed to be correlated to each other. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/fine_dust_over_years.png" style="width:800px;height:250px;background-color:white">

#### 2.3.1. Weather Parameters vs Fine Dust Concentration

The following series of figures show line charts that fine dust concentrations are plotted with each weather parameter. `wind_direction`, `wind_speed`, and `local_P(hPa)` showed a positive correlation with the particle density, whereas `temp(°C)`, `precipitation(mm)`, `humidity(%)`, `vapor_P(hPa)`, `dew_point_temp(°C)`, and `cloud_cover` showed a negative correlation.  Among the parameters, the `wind_direction` showed the strongest correlation with the particle density. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/fine_dust_all_history.png" style="width:800px;height:2000px;background-color:white">

## 3. Modeling

### 3.1. Machine Learning

6 different models, `DummyRegressr`(as a baseline), `Ridge`, `RandomForest`, `XGBoost`, `LightGBM`, and `CatBoost`, were utilized, and their performace was checked by cross-validation. According to the cross-validation, `LightGBM` and `CatBoost` performed best among the models tested. Considering the computation speed, `LightGBM` was mainly used for analysis in the subsequent analysis. The following figures show the prediction by each machine learning model.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/predictions_by_MLs_monthly.png" style="width:800px;height:1500px;background-color:white">

### 3.2. Deep Learning

In addition to the machine learning models, neural network was also used to check its performance. The following figure shows the prediction by neural network. The performance of neural network seemed similar to the machine learning models.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/prediction_by_DL_monthly.png" style="width:800px;height:250px;background-color:white">


## 4. Feature Importance

As mentioned in Introduction, the purpose of this study was to find out what kind of weather parameters affect the fine dust concentration in Seoul. By looking at the features that the machine learning model thought important, I thought we could get some idea about the correlation between the weather parameters and the fine dust concentration. For this analysis, `shap` library was used to check on the feature importances of `LightGBM` model.

First, the follwing figure shows the rank of the weather parameters listed in descending order. The result was close to that of intuition and the results of EDA. `wind_direction` was the most important parameter in determining the fine dust concentration.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/feature_rank.png" style="width:600px;height:700px;background-color:white">

The next figure shows how each weather parameter affect the fine dust concentration. Here, the red and blue indicates the high and low value of the parameter, respectively. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/feature_effect.png" style="width:600px;height:700px;background-color:white">

Among the weather parameters, 3 of them that looked most important were looked into more deeply. The first parameter was `wind_direction`, whose figure is shown below. The value for `wind_direction` ranged from 0 to 360. Since the values were scaled, -1.9/1.7, -1.0, 0, and 0.8 in the figure correspond to wind direction of North, East, South, and West. This graph tells us that the particle count would be lowest when the wind blows from East, and it would be highest when the wind blows from West, which matches well with the actual situation in Korea.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/wind_direction_effect.png" style="width:600px;height:400px;background-color:white">

The next parameter was `humidity(%)`. As shown in the figure below, the fine dust concentration got small when the `humidity(%)` was really high. This may had something to do with rain, because rain tends to remove the find dust particles in the air. The graph for precipitation shown below backs up this explanation.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/humidity_effect.png" style="width:600px;height:400px;background-color:white">

The following figure shows the effect of `precipitation(mm)` on the fine dust concentration. As mentioned earlier, the figure clearly shows that, if there were any amount of precipitation, the partcle counts tended to be low.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/precipitation_effect.png" style="width:600px;height:400px;background-color:white">

## 5. Further Analysis

The following figure shows the prediction by `LightGBM` model. According to the figure, it seemed that the prediction error got worse as the prediction was done on the later years.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/prediction_by_lgbm.png" style="width:800px;height:250px;background-color:white">

In order to check if the prediction accuracy really get deteriorated, the MAPE was calculated for the prediction of each year, and the result is shown in the figure below. The figure clearly shows that the error got bigger for later years.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_years.png" style="width:600px;height:400px;background-color:white">

In order to further analyze, the LightGBM model was trained with different range of data and its performance was investigated. For the analysis, the duration of the train data was kept at 8 years, while different range of data (such as 2008 ~ 2015, 2009 ~ 2016, etc.) was used for training. The following figure is the result, clearly showing that the prediction performance of the model got improved as the train data contained more recent data.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_train_data_1.png" style="width:600px;height:400px;background-color:white">

The next analysis was carried out such that the duration of the train data was not kept at 8 years any more. Instead, the model was trained using data that covered wider range of data(such as 2008 ~ 2015, 2008 ~ 2016, etc.).

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_train_data_2.png" style="width:600px;height:400px;background-color:white">

The above graph shows similar pattern as the previous figure. However, unexpectedly, the MAPE slightly increased when the model was trained with data containing wider range. This might be due to the fact that, as the model was trained for a wider range of data, too old data adversely affected the model's prediction for the future by making the model too "old fashioned".

## How to Run the Notebooks Locally

To download the contents of this GitHub page on to your local machine, follow these steps:

1. Copy and paste the following link: `git clone https://github.com/johnwslee/fine_dust_analysis.git` to your Terminal.

2. On your terminal, type: `cd fine_dust_analysis`.

3. Create a virtualenv by typing: `conda env create -f env.yml`

4. Activate the virtualenv by typing: `conda activate finedust_env`

5. Run the notebooks in notebook folder in order.
