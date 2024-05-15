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




