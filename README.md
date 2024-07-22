# COVID19 Data Analysis using Python

Kaggle Link - https://www.kaggle.com/code/rasikasrimal/covid-19-data-analysis

My other Covid-19 Data Visualization Project (advanced) - https://github.com/rasikasrimal/covid-19-data-visualization <br>
Kaggle Link - https://www.kaggle.com/code/rasikasrimal/covid-19-data-visualization

This project provides a hands-on exploration of COVID19 data analysis techniques using Python. It covers data preparation, analysis, and visualization, aiming to understand correlations with happiness metrics.

## Project Overview

- **Course Objectives:** Learn data preparation, exploration, and visualization techniques.
- **Datasets Used:** COVID19 dataset (John Hopkins University) and World Happiness Report dataset.
- **Project Structure:** Divided into tasks covering dataset import, measure calculation, and result visualization.

## Getting Started

1. Clone the repository: `git clone https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython.git`
2. Navigate to the project directory: `cd Covid19DataAnalysisUsingPython`
3. Explore the project files and tasks in the respective folders.

## Requirements

- Python 3
- Libraries: pandas, matplotlib, seaborn

## In Detail Overview Of The Project

1. Importing COVID-19 dataset
```python
import pandas as pd

corona_dataset_csv = pd.read_csv("Datasets/covid19_Confirmed_dataset.csv")
corona_dataset_csv.head(10)
```
![Importing Dataset](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/raw/main/Screenshots/importing%20dataset.png)

2.Aggregating the rows by the country
```python
corona_dataset_aggregated = corona_dataset_csv.groupby("Country/Region").sum()
corona_dataset_aggregated.head()
```
![Aggregating the rows by the country](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/Screenshot%202024-05-14%20214017.png)

3.Visualizing data related to a country for example China
```python
corona_dataset_aggregated.loc["China"][1:].plot(label='China')  
corona_dataset_aggregated.loc["Italy"][1:].plot(label='Italy')

plt.legend()
plt.title('COVID-19 Cases in China and Italy')
plt.xlabel('Date')
plt.ylabel('Total Cases')
plt.grid(True)

plt.show()
```
![Visualizing data related to a country](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/2.4.png)

4.Calculating a good measure
```python
corona_dataset_aggregated.loc['China'][1:].plot()
```
![Calculating a good measure](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/3.png)
```python
corona_dataset_aggregated.loc['Italy'][1:].plot()
```
![Calculating a good measure](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/3.2.png)

5.Calculating the first derivative of the curve
```python
corona_dataset_aggregated = corona_dataset_aggregated.apply(pd.to_numeric, errors='coerce')

corona_dataset_aggregated.fillna(method='ffill', inplace=True)
corona_dataset_aggregated.loc['China'].diff().plot()


plt.xlabel('Date')
plt.ylabel('Difference')
plt.title('Daily Change in COVID-19 Cases for China')

plt.grid(True)
plt.show()

```
![Calculating a good measure](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/Derivative.png)

6.Maximum infection rate for all of the countries. 

Chart Preview:

| Province/State               | Country/Region       | Lat       | Long      | 1/22/20 | 1/23/20 | ... | 4/29/20 | 4/30/20 |
|------------------------------|----------------------|-----------|-----------|---------|---------|-----|---------|---------|
| NaN                          | Afghanistan          | 33.0000   | 65.0000   | 0       | 0       | ... | 1828    | 1939    |
| NaN                          | Albania              | 41.1533   | 20.1683   | 0       | 0       | ... | 750     | 766     |
| NaN                          | Algeria              | 28.0339   | 1.6596    | 0       | 0       | ... | 3649    | 3848    |
| NaN                          | Andorra              | 42.5063   | 1.5218    | 0       | 0       | ... | 743     | 743     |
| NaN                          | Angola               | -11.2027  | 17.8739   | 0       | 0       | ... | 27      | 27      |
| NaN                          | Antigua and Barbuda | 17.0608   | -61.7964  | 0       | 0       | ... | 24      | 24      |
| NaN                          | Argentina            | -38.4161  | -63.6167  | 0       | 0       | ... | 4127    | 4285    |
| NaN                          | Armenia              | 40.0691   | 45.0382   | 0       | 0       | ... | 1867    | 1932    |
| Australian Capital Territory | Australia            | -35.4735  | 149.0124  | 0       | 0       | ... | 106     | 106     |
| New South Wales              | Australia            | -33.8688  | 151.2093  | 0       | 0       | ... | 3016    | 3016    |



```python
countries = list(corona_dataset_aggregated.index)
max_infection_rates = []

for c in countries:
    max_infection_rates.append(corona_dataset_aggregated.loc[c].diff().max())
corona_dataset_aggregated["max_infection_rate"] = max_infection_rates
corona_dataset_aggregated.head()
```
Code Execution Results:
| Province/State | 1/22/20 | 1/23/20 | 1/24/20 | 1/25/20 | 1/26/20 | 1/27/20 | ... | 4/29/20 | 4/30/20 | max_infection_rate |
|----------------|---------|---------|---------|---------|---------|---------|-----|---------|---------|--------------------|
| Afghanistan    | 0.0     | 0       | 0       | 0       | 0       | 0       | ... | 1939    | 2171    | 232.0              |
| Albania        | 0.0     | 0       | 0       | 0       | 0       | 0       | ... | 766     | 773     | 34.0               |
| Algeria        | 0.0     | 0       | 0       | 0       | 0       | 0       | ... | 3848    | 4006    | 199.0              |
| Andorra        | 0.0     | 0       | 0       | 0       | 0       | 0       | ... | 743     | 745     | 43.0               |
| Angola         | 0.0     | 0       | 0       | 0       | 0       | 0       | ... | 27      | 27      | 5.0                |

7.Create a new dataframe with only needed column 

```python
corona_data = pd.DataFrame(corona_dataset_aggregated["max_infection_rate"])
corona_data.head()
```
| Country/Region | max_infection_rate |
|----------------|--------------------|
| Afghanistan    | 232.0              |
| Albania        | 34.0               |
| Algeria        | 199.0              |
| Andorra        | 43.0               |
| Angola         | 5.0                |



### WorldHappinessReport.csv dataset: 
- Importing the WorldHappinessReport.csv dataset
- selecting needed columns for our analysis 
- join the datasets 
- calculate the correlations as the result of our analysis

### Importing the dataset

```python
happiness_report_csv = pd.read_csv("Datasets/worldwide_happiness_report.csv")
happiness_report_csv.head()
```
| Overall rank | Country or region | Score | GDP per capita | Social support | Healthy life expectancy | Freedom to make life choices | Generosity | Perceptions of corruption |
|--------------|-------------------|-------|----------------|----------------|-------------------------|------------------------------|------------|--------------------------|
| 1            | Finland           | 7.769 | 1.340          | 1.587          | 0.986                   | 0.596                        | 0.153      | 0.393                    |
| 2            | Denmark           | 7.600 | 1.383          | 1.573          | 0.996                   | 0.592                        | 0.252      | 0.410                    |
| 3            | Norway            | 7.554 | 1.488          | 1.582          | 1.028                   | 0.603                        | 0.271      | 0.341                    |
| 4            | Iceland           | 7.494 | 1.380          | 1.624          | 1.026                   | 0.591                        | 0.354      | 0.118                    |
| 5            | Netherlands       | 7.488 | 1.396          | 1.522          | 0.999                   | 0.557                        | 0.322      | 0.298                    |

Removing(drop) useless columns:
```python
drop_cols = ["Overall rank", "Score", "Generosity", "Perceptions of corruption"]
happiness_report_csv.drop(drop_cols, axis=1, inplace=True)
happiness_report_csv.head()
```
| Country or region | GDP per capita | Social support | Healthy life expectancy | Freedom to make life choices |
|-------------------|----------------|----------------|-------------------------|------------------------------|
| Finland           | 1.340          | 1.587          | 0.986                   | 0.596                        |
| Denmark           | 1.383          | 1.573          | 0.996                   | 0.592                        |
| Norway            | 1.488          | 1.582          | 1.028                   | 0.603                        |
| Iceland           | 1.380          | 1.624          | 1.026                   | 0.591                        |
| Netherlands       | 1.396          | 1.522          | 0.999                   | 0.557                        |

Changing the indices of the dataframe:
```python
happiness_report_csv.set_index("Country or region", inplace=True)
happiness_report_csv.head()
```

Corona dataset:

| Country/Region | max_infection_rate |
|----------------|--------------------|
| Afghanistan    | 232.0              |
| Albania        | 34.0               |
| Algeria        | 199.0              |
| Andorra        | 43.0               |
| Angola         | 5.0                |

Wolrd happiness report Dataset :
| Country or region | GDP per capita | Social support | Healthy life expectancy | Freedom to make life choices |
|-------------------|----------------|----------------|-------------------------|------------------------------|
| Finland           | 1.340          | 1.587          | 0.986                   | 0.596                        |
| Denmark           | 1.383          | 1.573          | 0.996                   | 0.592                        |
| Norway            | 1.488          | 1.582          | 1.028                   | 0.603                        |
| Iceland           | 1.380          | 1.624          | 1.026                   | 0.591                        |
| Netherlands       | 1.396          | 1.522          | 0.999                   | 0.557                        |

```python
data = corona_data.join(happiness_report_csv, how="inner")
data.head()
```

| Country        | max_infection_rate | GDP per capita | Social support | Healthy life expectancy | Freedom to make life choices |
|----------------|--------------------|----------------|----------------|-------------------------|------------------------------|
| Afghanistan   | 232.0              | 0.350          | 0.517          | 0.361                   | 0.000                        |
| Albania        | 34.0               | 0.947          | 0.848          | 0.874                   | 0.383                        |
| Algeria        | 199.0              | 1.002          | 1.160          | 0.785                   | 0.086                        |
| Argentina      | 291.0              | 1.092          | 1.432          | 0.881                   | 0.471                        |
| Armenia        | 134.0              | 0.850          | 1.055          | 0.815                   | 0.283                        |

Correlation matrix:
```python
data.corr()
```
|                   | max_infection_rate | GDP per capita | Social support | Healthy life expectancy | Freedom to make life choices |
|-------------------|--------------------|----------------|----------------|-------------------------|------------------------------|
| max_infection_rate | 1.000000          | 0.250118       | 0.191958       | 0.289263                | 0.078196                     |
| GDP per capita    | 0.250118           | 1.000000       | 0.759468       | 0.863062                | 0.394603                     |
| Social support    | 0.191958           | 0.759468       | 1.000000       | 0.765286                | 0.456246                     |
| Healthy life expectancy | 0.289263     | 0.863062       | 0.765286       | 1.000000                | 0.427892                     |
| Freedom to make life choices | 0.078196 | 0.394603      | 0.456246       | 0.427892                | 1.000000                     |



### Visualization of the results

Plotting GDP vs maximum Infection rate:
```python
x = data["GDP per capita"]
y = data["max_infection_rate"]
sns.scatterplot(x=x, y=y)
```
![Plotting GDP vs maximum Infection rate](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/GDP%20vs%20Max%20Infection%20rate.png)

In Log Scale:
```python
x = data["GDP per capita"]
y = data["max_infection_rate"]
sns.scatterplot(x=x, y=np.log(y))
```
![Plotting GDP vs maximum Infection rate](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/GDP%20vs%20Max%20Infection%20rate%20log%20scale.png)

```python
sns.regplot(x=x, y=np.log(y))
```
![Plotting GDP vs maximum Infection rate best fitting line](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/GDP%20vs%20Max%20Infection%20rate%20regplot.png)


Plotting Social support vs maximum Infection rate:
```python
x = data["Social support"]
y = data["max_infection_rate"]
sns.scatterplot(x=x, y=np.log(y))
```
![Plotting Social support vs maximum Infection rate](https://github.com/rasikasrimal/Covid19DataAnalysisUsingPython/blob/main/Screenshots/Plotting%20Social%20support%20vs%20maximum%20Infection%20rate.png)

