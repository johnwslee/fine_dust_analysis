![license
status](https://img.shields.io/github/license/johnwslee/fine_dust_analysis)

# Study on the Relationship between Weather Parameters and Fine Dust Concentration in Seoul, South Korea

**Author:** John W.S. Lee

## 1. Introduction

In Seoul, South Korea, there are certain seasons when the air quality gets really bad because of the high concentration of fine dusts. The concentration of fine dusts are measured in terms of the density of fine particles, and based on the size of the particle, there are two metrics: (1) PM10 - density of particulate matter less than 10$\mu$m, and (2) PM2.5 -  density of particulate matter less than 2.5$\mu$m (Please note that the density of PM2.5 was written as PM25 in the notebooks since the dot can cause unwanted results). It was one day during winter in 2023 when the air quality was really  bad. I just got curious what kind of weather parameters affect the concentration of fine dust, and that was the sole motivation of this study. The detailed procedure and codes can be found in the [notebook folder](https://github.com/johnwslee/fine_dust_analysis/tree/main/notebooks), whereas this README shows the overall approaches and some of the interesting results that I found.
Before I move on to the next section, I just want to say that the purpose of using the machine learning in this study was not to predict the concentration of the fine dust, it was rather to take a look at the features that the models thought important, thereby fulfilling my curiosity.

## 2. Data

### 2.1. Fine Dust Dataset

The fine dust data was downloaded from [Public Data Portal](https://www.data.go.kr/data/15089266/fileData.do). The data contains hourly data for fine dust measured in many different regions in Seoul, for the period of 2008 to 2021. For this study, I extracted the average values of the fine dust for each time.

### 2.2. Weather Dataset

Weather data was downloaded from [Open MET Data Portal](https://data.kma.go.kr/data/grnd/selectAsosRltmList.do?pgmNo=36&tabNo=1) by KMA (Korea Meteorological Administration) Weather Data Service. The original dataset had 27 columns, but only 11 columns that seem relevant were used in this study. The chosen weather parameters were "date", "temp(°C)", "precipitation(mm)", "wind_speed(m/s)", "wind_direction", "humidity(%)", "vapor_P(hPa)", "dew_point_temp(°C)", "local_P(hPa)", "cloud_cover", and "lowest_ceiling(100m)".

### 2.3. Exploratory Data Analysis

#### 2.3.1. Fine Dust Concentratio over Years

The following figure shows how the fine dust concentration has been fluctuating since the year of 2008. The figure showed both particle counts had seasonality to a certain degree, and they seemed to be correlated to each other. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/fine_dust_over_years.png" style="width:800px;height:250px;background-color:white">

#### 2.3.1. Weather Parameters vs Fine Dust Concentration

The following series of figures show line charts that fine dust concentrations are plotted with each weather parameter. "wind_direction, "wind_speed", and "local_P(hPa)" showed a positive correlation with the particle density, whereas "temp(°C)", "precipitation(mm)", "humidity(%)", "vapor_P(hPa)", "dew_point_temp(°C)", and "cloud_cover" showed a negative correlation.  Among the parameters, the "wind_direction" showed the strongest correlation with the particle density. 

<img src="https://github.com/johnwslee/fine_dust_analysis/blob/main/img/fine_dust_all_history.png" style="width:800px;height:3000px;background-color:white">

## 3. Modeling

### 3.1. Machine Learning


### 3.2. Deep Learning


## 4. Feature Importance


## 5. Further Analysis



## How to Run the Notebooks Locally

To download the contents of this GitHub page on to your local machine, follow these steps:

1. Copy and paste the following link: `git clone https://github.com/johnwslee/fine_dust_analysis.git` to your Terminal.

2. On your terminal, type: `cd fine_dust_analysis`.

3. Create a virtualenv by typing: `conda env create -f env.yml`

4. Activate the virtualenv by typing: `conda activate finedust_env`

5. Run the notebooks in notebook folder in order.
