![license
status](https://img.shields.io/github/license/johnwslee/fine_dust_analysis)

# Study on the Relationship between Weather Parameters and Fine Dust Concentration in Seoul, Korea

**Author:** John W.S. Lee

## 1. Introduction

In Seoul, there are certain seasons when the air quality gets really bad due to the high concentration of fine dust. The concentration of fine dust is measured in terms of the density of fine particles. Based on the size of the particle, there are two metrics: (1) PM10 - density of particulate matter less than 10um, and (2) PM2.5 - density of particulate matter less than 2.5um (Please note that the density of PM2.5 was written as PM25 in the notebooks, as the dot can cause unwanted results).

It was a winter day in 2023 when the air quality was particularly poor. I became curious about the weather parameters that affect the concentration of fine dust, and that was the sole motivation for this study. The detailed procedure and codes can be found in the [notebook folder](https://github.com/johnwslee/fine_dust_analysis/tree/main/notebooks), while this README provides an overview of the approaches and some interesting results that I discovered.

Before I move on to the next section, I want to clarify that the purpose of using machine learning in this study was not to predict the concentration of fine dust. Rather, it was to examine the features that the models deemed important, satisfying my curiosity.

## 2. Data

### 2.1. Fine Dust Dataset

The fine dust data was downloaded from [Public Data Portal](https://www.data.go.kr/data/15089266/fileData.do). The dataset contains hourly measurements of fine dust in various regions of Seoul, spanning from 2008 to 2021. For this study, I extracted the average values of fine dust for each time period.

### 2.2. Weather Dataset

Weather data was downloaded from [Open MET Data Portal](https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36&tabNo=1) by KMA (Korea Meteorological Administration) Weather Data Service. The original dataset had 27 columns, but only 11 columns that seemed relevant were used in this study. The chosen weather parameters were `date`, `temp(°C)`, `precipitation(mm)`, `wind_speed(m/s)`, `wind_direction`, `humidity(%)`, `vapor_P(hPa)`, `dew_point_temp(°C)`, `local_P(hPa)`, `cloud_cover`, and `lowest_ceiling(100m)`.

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

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/predictions_by_MLs_monthly.png" style="width:800px;height:1200px;background-color:white">

### 3.2. Deep Learning

In addition to the machine learning models, neural network was also used to check its performance. The following figure shows the prediction by neural network. The performance of neural network seemed similar to the machine learning models.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/prediction_by_DL_monthly.png" style="width:800px;height:250px;background-color:white">


## 4. Feature Importance

As mentioned in the introduction, the purpose of this study was to determine which weather parameters affect the concentration of fine dust in Seoul. By examining the features identified as important by the machine learning model, I aimed to gain insights into the correlation between weather parameters and fine dust concentration. To analyze this, I utilized the `shap` library to assess the feature importances of the `LightGBM` model.

First, the follwing figure shows the rank of the weather parameters listed in descending order. The result was close to that of intuition and the results of EDA. `wind_direction` was the most important parameter in determining the fine dust concentration.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/feature_rank.png" style="width:600px;height:700px;background-color:white">

The next figure shows how each weather parameter affect the fine dust concentration. Here, the red and blue indicates the high and low value of the parameter, respectively. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/feature_effect.png" style="width:600px;height:700px;background-color:white">

Among the weather parameters, three of them appeared to be the most important, and further analysis was conducted on them. 

The first parameter was `wind_direction`, as shown in the figure below. The values for `wind_direction` ranged from 0 to 360. Since the values were scaled, -1.9/1.7, -1.0, 0, and 0.8 in the figure corresponded to the wind direction of North, East, South, and West, respectively. This graph indicated that the particle count was lowest when the wind blew from the East, and highest when the wind blew from the West. These findings aligned well with the actual situation in Korea.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/wind_direction_effect.png" style="width:600px;height:400px;background-color:white">

The next parameter was `humidity(%)`. As shown in the figure below, the fine dust concentration got small when the `humidity(%)` was really high. This may had something to do with rain, because rain tends to remove the find dust particles in the air. The graph for precipitation shown below backs up this explanation.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/humidity_effect.png" style="width:600px;height:400px;background-color:white">

The following figure shows the effect of `precipitation(mm)` on the fine dust concentration. As mentioned earlier, the figure clearly shows that, if there were any amount of precipitation, the partcle counts tended to be low.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/precipitation_effect.png" style="width:600px;height:400px;background-color:white">

## 5. Effect of Train Data on the Prediction Performance

The figure below shows the prediction by the `LightGBM` model. According to the figure, there are two interesting observations to note. First, the predicted particle concentration was significantly higher than the actual `PM10_Counts` in 2020. This could be attributed to the outbreak of Covid-19, which led to lockdowns worldwide in 2020. Second, it appears that the prediction error worsened as the predictions were made for later years. This issue will be further investigated in the next section.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/prediction_by_lgbm.png" style="width:800px;height:250px;background-color:white">

To assess whether the prediction accuracy indeed deteriorated, the Mean Absolute Percentage Error (MAPE) was calculated for each year's prediction, as shown in the figure below. The figure clearly indicates that the error increased for the later years.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_years.png" style="width:600px;height:400px;background-color:white">

To conduct further analysis, the `LightGBM` model was trained using different ranges of data, and its performance was investigated. For this analysis, the duration of the training data was fixed at 8 years, while varying ranges of data (such as 2008-2015, 2009-2016, etc.). The resulting figure illustrates that the prediction performance of the model improved when the training data included more recent observations.

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_train_data_1.png" style="width:800px;height:400px;background-color:white">

Next, an analysis was performed with a modified approach, where the duration of the training data was not limited to 8 years. Instead, the model was trained using data that covered a wider range (such as 2008-2015, 2008-2016, etc.).

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/MAPE_different_train_data_2.png" style="width:800px;height:400px;background-color:white">

The graph above exhibits a similar pattern to the previous figure. However, unexpectedly, the MAPE slightly increased when the model was trained with data encompassing a wider range. This could be attributed to the fact that including excessively old data in the training process affected the model's prediction for the future by making it too "old-fashioned".

## How to Run the Notebooks Locally

To download the contents of this GitHub page on to your local machine, follow these steps:

1. Copy and paste the following link: `git clone https://github.com/johnwslee/fine_dust_analysis.git` to your Terminal.

2. On your terminal, type: `cd fine_dust_analysis`.

3. Create a virtualenv by typing: `conda env create -f env.yml`

4. Activate the virtualenv by typing: `conda activate finedust_env`

5. Run the notebooks in notebook folder in order.
