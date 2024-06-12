# US Power Outage Analysis
By Marija Vukic and Sadrac Santacruz Ibarra

# Introduction
Throughtout this project, we explore major power outage events in the US from January 2000 to July 2016, using a data set curated by [*Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure*](https://engineering.purdue.edu/LASCI/research-data/outages). The dataset tracks significant power outage events across the US in the span of 16 years, along with geographical, climatic, electricity consumption trends, and economic attributes of the states impacted by these outages. Throughout this project we clean our data, conduct exploratory analysis to grasp an understanding of the trends present in our data, perform hypothesis testing, and create a prediction model. Our project aims to explore the question, ***what are the most frequent causes of power outages in different climate regions of the United States?***

Our initial raw data frame consists of 1535 rows and 57 columns. Of these columns from our initial data frame, we kept 31 columns for the sake of our analysis as described below. 

|Column                |Description|
|---                |---        |
|`'year'`                |Year an outage occurred|
|`'month'`                |Month an outage occurred|
|`'us_state'`                |US state the outage occurred in|
|`'nerc_region'`                |Region defined by the North American Electric Reliability Corporation (NERC) in which the outage occured|
|`'climate_region'`                |One of 9 U.S. Climate regions as defined by National Centers for Environmental Information (NCEI)|
|`'outage_start_date'`                |Day of the year when the outage event started|
|`'outage_start_time'`                |Time of the day when the outage event started|
|`'outage_restoration_date'`                |Day of the year when power was restored to all the customers|
|`'outage_restoration_time'`                |Time of the day when power was restored to all the customers|
|`'cause_category'`                |One of 7 categories that decribe the causes for major power outages|
|`'cause_category_detail'`                |Detailed description of the event categories causing the major power outages|
|`'outage_duration'`                |Duration of outage events (minutes)|
|`'customers_affected'`                |Number of customers affected by the power outage event|
|`'res_price'`                |Monthly electricity price in the residential sector (cents/kilowatt-hour)|
|`'com_price'`                |Monthly electricity price in the commercial sector (cents/kilowatt-hour)|
|`'ind_price'`                |Monthly electricity price in the industrial sector (cents/kilowatt-hour)|
|`'total_price'`                |Average monthly electricity price in the U.S. state (cents/kilowatt-hour)|
|`'res_sales'`                |Electricity consumption in the residential sector (megawatt-hour)|
|`'com_sales'`                |Electricity consumption in the commercial sector (megawatt-hour)|
|`'ind_sales'`                |Electricity consumption in the industrial sector (megawatt-hour)|
|`'total_sales'`                |Total electricity consumption in the U.S. state (megawatt-hour)|
|`'res_customers'`                |Annual number of customers served in the residential electricity sector of the U.S. state|
|`'com_customers'`                |Annual number of customers served in the commercial electricity sector of the U.S. state|
|`'ind_customers'`                |Annual number of customers served in the industrial electricity sector of the U.S. state|
|`'total_customers'`                |Annual number of total customers served in the U.S. state|
|`'pc_realgsp_state'`                |Per capita real gross state product (GSP) in the U.S. state (measured in 2009 chained U.S. dollars)|
|`'pc_realgsp_usa'`                |	Per capita real GSP in the U.S. (measured in 2009 chained U.S. dollars)|
|`'pc_realgsp_rel'`                |Relative per capita real GSP as compared to the total per capita real GDP of the U.S. (expressed as fraction of per capita State real GDP & per capita US real GDP)|
|`'pc_realgsp_change'`                |Percentage change of per capita real GSP from the previous year (in %)|
|`'total_realgsp'`                |Real GSP contributed by all industries (total) (measured in 2009 chained U.S. dollars)|
|`'population'`                |Population in the U.S. state in a year|

# Data Cleaning & Exploratory Data Analysis

## Data Cleaning
In order to investigate our question, we begin by cleaning our data.

1. After importing our data, we found there were multiple columns and rows we had to drop as a result of the way the data was originally formated. As a result we dropped the first row that contained metrics symbols and the `'variables'` column. 
2. Next, we droped 25 columns that were irrelevant to our goals of data analysis. From there, we had 31 remaining columns, which included `'YEAR'`, `'MONTH'`, `'U.S._STATE'`, `'NERC.REGION'`, `'CLIMATE.REGION'`, `'OUTAGE.START.DATE'`, `'OUTAGE.START.TIME'`, `'OUTAGE.RESTORATION.DATE'`, `'OUTAGE.RESTORATION.TIME'`, `'CAUSE.CATEGORY'`, `'CAUSE.CATEGORY.DETAIL'`, `'OUTAGE.DURATION'`, `'CUSTOMERS.AFFECTED'`, `'RES.PRICE'`, `'COM.PRICE'`, `'IND.PRICE'`, `'TOTAL.PRICE'`, `'RES.SALES'`, `'COM.SALES'`, `'IND.SALES'`, `'TOTAL.SALES'`, `'RES.CUSTOMERS'`, `'COM.CUSTOMERS'`, `'IND.CUSTOMERS'`, `'TOTAL.CUSTOMERS'`, `'PC.REALGSP.STATE'`, `'PC.REALGSP.USA'`, `'PC.REALGSP.REL'`, `'PC.REALGSP.CHANGE'`, `'TOTAL.REALGSP'`, and `'POPULATION'`.
3. We then reformatted our columns for ease of useability when calling them throughout. We lowered the case, then replaced any periods with underscores. An example of this reformatting is as follows: `'U.S._STATE'` &#8594; `'us_state'`.
4. From there, we created additional columns to further aid us in our analysis. 

First we concactenated the data for both outage start and restoration dates and times. The new columns we created are `'outage_start'`, which combines `'outage_start_date'` and `'outage_start_time'` into a single column, and `'outage_restoration'`, which combines `'outage_restoration_date'` and `'outage_restoration_time'` into a single column. Next, we also created columns that store data on the revenue by industry (residential, commercial, industrial, and total). We aggregated the data for these columns by taking each industry's specific price and sales data and multiplying them together to calculate revenue. We decided to create these revenue columns to explore their distributions throughout our EDA process.

|Column                |Description|
|---                |---        |
|`'outage_start'`                |Day of the year and time of day when the outage event started|
|`'outage_restoration'`                |Day of the year and time of day when power was restored to all the customers|
|`'res_revenue'`                |Average monthly electricity revenue in the residential sector (price in dollars per megawatt-hour)|
|`'com_revenue'`                |Average monthly electricity revenue in the commercial sector (price in dollars per megawatt-hour)|
|`'ind_revenue'`                |Average monthly electricity revenue in the industrial sector (price in dollars per megawatt-hour)|
|`'total_revenue'`                |Average monthly electricity revenue in the U.S. state (price in dollars per megawatt-hour)|

## Exploratory Data Analysis
Next, we want to grap a better understanding of our data throughout visualizations that give us insights into different data distributions and relationships between different variables in our data set.

### Univariate Analysis

### Bivariate Analysis

### Interesting Aggregates

# Assessment of Missingness

## NMAR Analysis

## Missingness Dependency

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis