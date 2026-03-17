# A Data Analysis of Power Outages

# Introduction

In this Project I will explore data on major power outage events in the continental U.S. The data is from major power outages witnessed by various states during January 2000-July 2016. The outages dataset contains outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW. The dataset also contains various factors such as geographical locations, electricity consumption, economic characteristics, causes, and regional climatic info.

The dataset was accessed from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure at https://engineering.purdue.edu/LASCI/research-data/outages. Throughout this project, we will explore the following question: Which factors are most strongly associated with the duration of major power outages in the United States? We can answer this question through data cleaning and exploratory data analysis. After we can build a predictive model to predict the duration of a major power outage as a measure of its severity  

The insights we gain from this data is important to everyone, including communities, businesses, and essential services. The question I am exploring may prevent future outages and provide insights into why they occur. Understanding this relationship can help utility companies focus on upgrades or repairs to existing power grids or on how they may be able to prevent the next outage. Along with this, we can identify high-risk areas and the causes to prevent future outages.

## The Dataset
The original data frame had 1534 rows for each outage and 57 columns. However, I will only focus on 20 columns that are relevant to my analysis as shown bellow. 

| **Feature** | **Description** |
|-------------|----------------|
| `YEAR` | Year the outage occurred |
| `MONTH` | Month the outage occurred |
| `U.S_STATE` | U.S. state the outage occurred in |
| `NERC.REGION` | North American Electric Reliability Corporation (NERC) region involved in the outage event |
| `CLIMATE.REGION` | U.S. Climate region the outage occurred in |
| `ANOMALY.LEVEL` | Numeric value showing oceanic El Niño/La Niña (ONI) index |
| `OUTAGE.START.DATE` | Date the outage began |
| `OUTAGE.START.TIME` | Time the outage began |
| `OUTAGE.RESTORATION.DATE` | Date the power was restored |
| `OUTAGE.RESTORATION.TIME` | Time the power was restored |
| `CAUSE.CATEGORY` | Categorical cause of the outage |
| `OUTAGE.DURATION` | Duration of the outage |
| `DEMAND.LOSS.MW` | Amount of peak demand lost during the outage |
| `CUSTOMERS.AFFECTED` | Number of customers affected by the outage |
| `TOTAL.PRICE` | Average monthly electricity price in the U.S. state |
| `TOTAL.SALES` | Total electricity consumption in the U.S. state |
| `TOTAL.CUSTOMERS` | Annual number of total customers served in the U.S. state |
| `POPPCT_URBAN` | Percentage of the total population of the U.S. in urban areas |
| `POPDEN_URBAN` | Population density of the urban areas |
| `AREAPCT_URBAN` | Percentage of the land area of the classified as urban areas |

# Data Cleaning and Exploratory Data Analysis
## Cleaning
1. I started by selecting out only the columns needed for the Data Analysis and dropping others that were not necessary. These are:`YEAR`,`MONTH`,`U.S._STATE`,`NERC.REGION`, `CLIMATE.REGION`,`ANOMALY.LEVEL`,`OUTAGE.START.DATE`,`OUTAGE.START.TIME`, `OUTAGE.RESTORATION.DATE`,`OUTAGE.RESTORATION.TIME`,`CAUSE.CATEGORY`,`OUTAGE.DURATION`,`DEMAND.LOSS.MW`,`CUSTOMERS.AFFECTED` `TOTAL.PRICE`,`TOTAL.SALES`,`TOTAL.CUSTOMERS`,`POPPCT_URBAN`,`POPDEN_URBAN`, and `AREAPCT_URBAN`.

2. Next, I converted the `OUTAGE.START.DATE` and `OUTAGE.RESTORATION.DATE` columns into datetime. Then I combined `OUTAGE.START.DATE` and `OUTAGE.START.TIME` into the column `OUTAGE.START`. Along with this I combined `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` into the column `OUTAGE.RESTORATION`. Lastly, I dropped the old columns which are no longer relevant since they have now been combined. 

3. Next, I replaced any 0's with NaN for columns where 0 could mean missing data. These include `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`, `OUTAGE.DURATION`. When these values have 0 it could indicate that the outage information is missing. 

4. Next, I standardized and combined `POPPCT_URBAN`, `POPDEN_URBAN` and, `AREAPCT_URBAN` into the column `URBAN` which takes in the population living in urban areas, urban density, and the percentage of land area classified as urban. Lastly, I dropped those columns are they are no longer relevant. 

The first few rows of the cleaned DataFrame are shown bellow with some columns shown. 

| U.S._STATE | NERC.REGION | CAUSE.CATEGORY    | OUTAGE.DURATION | OUTAGE.START        | URBAN |
|------------|-------------|-------------------|-----------------|---------------------|-------|
| Minnesota  | MRO         | severe weather    | 3060            | 2011-07-01 17:00:00 | -0.51 |
| Minnesota  | MRO         | intentional attack| 1               | 2014-05-11 18:38:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 3000            | 2010-10-26 20:00:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 2550            | 2012-06-19 04:30:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 1740            | 2015-07-18 02:00:00 | -0.51 |

## Exploratory Data Analysis 
### Univariate Analysis
In my exploratory data analysis I first looked at a univariate analysis to see the frequency of outages based on the category they are under. 
<iframe
  src="outage_per_cause.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
I also looked at how the number of outages changed in each climate region.
<iframe
  src="outage_per_region.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis
For the bivariate analysis I tested how different features affected each other but the ones I found most interesting are found bellow. 
For the first graph I looked at the relationship between outage duration and number of customers affected to examine their relationship. 
<iframe
  src="duration_vs_customer.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Another relationship I found interesting was the number of customers affected and the cause category. 
<iframe
  src="num_customer_category.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
### Interesting Grouping
A group that I found interesting was a table showing which causes lead to the most people affected aggregated by mean, median, and count. 
| CAUSE.CATEGORY                | mean       | median   | count|
|-------------------------------|------------|----------|------|
| system operability disruption | 211066.02  | 69000.0  | 83   |
| severe weather                | 190971.94  | 111599.5 | 708  |
| equipment failure             | 105450.59  | 47000.0  | 29   |
| intentional attack            | 18753.42   | 2500.0   | 19   |
| public appeal                 | 15999.40   | 15800.0  | 10   |
| islanding                     | 7232.72    | 4300.0   | 29   |
| fuel supply emergency         | 1.00       | 1.0      | 1    |

# Assessment of Missingness
## MNAR Analysis
From the data set the `CUSTOMERS.AFFECTED` column is likely MNAR since utilities may be less likely to report customer counts for smaller outages. Along with this many factors go into why the customers affected may not be counted as it all likely comes down to the company reporting the data. 

## Missingness Dependency
In order to test missingness dependency I will focus on the column `OUTAGE.DURATION` against the columns 