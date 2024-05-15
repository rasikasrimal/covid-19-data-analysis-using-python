# COVID19 Data Analysis using Python

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











