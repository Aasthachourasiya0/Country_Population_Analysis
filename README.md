# Country Population Analysis and Visualization

**Project Overview**
This project provides a comprehensive analysis and visualization of country population data. It includes data cleaning, analysis, and visualization components designed to offer insights into global population trends. The dataset used in this project is sourced from Kaggle and has been processed to facilitate accurate and meaningful analysis.

# Repository Contents
**Country_population.ipynb**: Jupyter Notebook containing the main analysis and visualization scripts.
**Dashboard_country_population.pbit**: Power BI template file for interactive data visualization.
**cleaned_data_Task1.csv**: Cleaned dataset used for analysis.

# Getting Started
**Prerequisites**
To run the Jupyter Notebooks and interact with the Power BI dashboard, ensure you have the following software installed:

* Python 3.x
* Jupyter Notebook
* Power BI Desktop

# Data Description
The dataset cleaned_data_Task1.csv contains the following columns:

* Country: Name of the country.
* Year: Year of the population data.
* Population: Population count for the specified year.

# Data Source
The data used in this project is sourced from Kaggle. You can find the original dataset here.

# Analysis Workflow
Jupyter Notebooks
* Country_population.ipynb:

    * Data loading and preprocessing.
    * Exploratory Data Analysis (EDA).
    * Visualization of population trends.
    * Statistical analysis and insights.
* Untitled.ipynb:

   * Additional exploratory analysis.
   * Supplementary visualizations and insights.
Power BI Dashboard
* Dashboard_country_population.pbit:
   * Interactive dashboard for dynamic data exploration.
   * Visual representations of population trends by country and year.
   * Filters for customized data views.
# Python Analysis
Data Cleaning
The data cleaning process involves removing rows with missing values and converting the year columns to numeric types.

python
Copy code

import pandas as pd

import matplotlib.pyplot as plt

import plotly.express as px

# Load the data
df = pd.read_csv('cleaned_data_Task1.csv')

# Drop rows with missing values
df.dropna(inplace=True)

# Convert year columns to numeric type
for col in df.columns[1:]:

    df[col] = pd.to_numeric(df[col], errors='coerce')

# Save the cleaned data
df.to_csv('cleaned_data_Task1_cleaned.csv', index=False) 

# Time Series Analysis
Convert the data from wide to long format for time series analysis and group by country and year.

python
Copy code

# Convert the data from wide to long format
df2 = df.melt(id_vars=["Country Name"], var_name="Year", value_name="Population")

df2['Year'] = df2['Year'].astype(int)

# Group by country and year, summing the populations
df2 = df2.groupby(['Country Name', 'Year']).sum().reset_index()

# Check the first few rows
df2.head()

# Visualizations
Total Population Over Years
python
Copy code

years = df.columns[1:]
population = [df[year].sum() for year in years]

plt.figure(figsize=(12, 4))
plt.plot(years, population, color="blue")
plt.xticks(rotation=90)
plt.xlabel('Year')
plt.ylabel('Total Population')
plt.title('Total Population Over Years')
plt.grid(True)
plt.show()

Population Trends by Country
python
Copy code

selected_countries = ['India', 'China', 'European Union']
plt.figure(figsize=(12, 8))
for country in selected_countries:
    country_data = df2[df2['Country Name'] == country]
    plt.plot(country_data['Year'], country_data['Population'], label=country)
plt.xlabel('Year')
plt.ylabel('Population')
plt.title('Population Trends by Country (1960-2022)')
plt.legend()
plt.grid(True)
plt.show()

**Population Variation Over Years**

python
Copy code

df2['Population Variation'] = df2.groupby('Country Name')['Population'].diff().fillna(0)
fig = px.line(df2.groupby('Year')['Population Variation'].sum().reset_index(),
              x='Year', y='Population Variation', title='Population Variation Over Years')
fig.show()

**World Map Showing Population Distribution**
python
Copy code

fig = px.choropleth(df2[df2['Year'] == 2022].groupby('Country Name')['Population'].sum().reset_index(),locations='Country Name', locationmode='country names', color='Population' hover_name='Country Name', color_continuous_scale='Rainbow')

fig.update_geos(projection_type="natural earth", showcoastlines=True)

fig.update_layout(title_text='World Map 2022 - Population')

fig.show()

# Power BI Analysis
**Data Transformation**
Load the cleaned data (cleaned_data_Task1_cleaned.csv) into Power BI.

Transform the data by unpivoting the year columns to create a Year and Population column.

Creating Visualizations

Total Population Over Years

Use a Line Chart, drag Year to the X-axis and Population to the Y-axis.

Population Variation Over Years

Create a new calculated column using DAX:

DAX
Copy code

Population Variation = [Population] - CALCULATE(SUM([Population]), DATEADD(DATE(VALUE('Year'), 1, 1), -1, YEAR))

Use a Line Chart, drag Year to the X-axis and Population Variation to the Y-axis.

**Population Growth by Country**

Use a Line Chart or Stacked Area Chart, add Country Name to the Legend, Year to the X-axis, and Population to the Y-axis.

Population Trends for Selected Countries

Filter data to include India, China, and European Union and use a Line Chart.

World Map Showing Population Distribution

Use a Map visualization, add Country Name to the Location field and Population to the Size field.

**Connecting Slicers**
To connect slicers in Power BI and have them interact with each other:

* Add slicers to your report.
    Select one slicer, go to the Format tab, and click on Edit Interactions.
    Configure the interactions between slicers by selecting the filter icon above the other slicers.

# Conclusion
This project successfully demonstrates the process of analyzing population data using Python and Power BI. By following the provided scripts and steps, you can replicate the analysis and create insightful visualizations and interactive dashboards.

# Acknowledgements
The dataset is sourced from Kaggle.
Special thanks to the contributors and the open-source community.











