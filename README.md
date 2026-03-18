# A Data Analysis of Power Outages

# Introduction

In this Project I will explore data on major power outage events in the continental U.S. The data is from major power outages witnessed by various states during January 2000-July 2016. The outages dataset contains outages that impacted at least 50,000 customers or caused an unplanned firm load loss of at least 300 MW. The dataset also contains various factors such as geographical locations, electricity consumption, economic characteristics, causes, and regional climatic info.

The dataset was accessed from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure at https://engineering.purdue.edu/LASCI/research-data/outages. Throughout this project, we will explore the following question: **Which factors are most strongly associated with the duration of major power outages in the United States?** We can answer this question through data cleaning and exploratory data analysis. After we can build a predictive model to predict the duration of a major power outage as a measure of its severity  

The insights we gain from this data is important to everyone, including communities, businesses, and essential services. The question I am exploring may prevent future outages and provide insights into why they occur. Understanding this relationship can help utility companies focus on upgrades or repairs to existing power grids or on how they may be able to prevent the next outage. Along with this, we can identify high-risk areas and the causes to prevent future outages.

## The Dataset
The original data frame had 1534 rows for each outage and 57 columns. However, I will only focus on 20 columns that are relevant to my analysis as shown below. 

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

The first few rows of the cleaned DataFrame are shown below with some columns shown. 

| U.S._STATE | NERC.REGION | CAUSE.CATEGORY    | OUTAGE.DURATION | OUTAGE.START        | URBAN |
|------------|-------------|-------------------|-----------------|---------------------|-------|
| Minnesota  | MRO         | severe weather    | 3060            | 2011-07-01 17:00:00 | -0.51 |
| Minnesota  | MRO         | intentional attack| 1               | 2014-05-11 18:38:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 3000            | 2010-10-26 20:00:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 2550            | 2012-06-19 04:30:00 | -0.51 |
| Minnesota  | MRO         | severe weather    | 1740            | 2015-07-18 02:00:00 | -0.51 |

## Exploratory Data Analysis 
### Univariate Analysis
To begin my exploratory data analysis, I first examined the distribution of major power outages acroess cause categories to understand the cause of most power outages in the U.S. As shown below the leading cause was severe weather with intentional attacks following as second. 

<iframe
  src="assets/outage_per_cause.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

Next, I examined how major power outages are distributed across the U.S. climate regions to understand whether geography plays a role in outage frequency. The bar chart shows how Northeast and South regions experience the highest number of outages while the West North Central and Southwest experience the least. 

<iframe
  src="assets/outage_per_region.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Bivariate Analysis
Moving into bivariate analysis, I tested how different features affected each other but the ones I found most interesting are found below. 

The first plot examines the relationship between outage duration and the number of customers affected, colored by category. The lack of strong linear relationship suggests that outage does not necessarily determine how long it takes to restore power. Hovering over individual points also reveals how certain states have more extreme values. 

<iframe
  src="assets/duration_vs_customer.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The second plot uses a box plot to compare the number of customers affected across each cause category. Severe weather and system operability disruptions show the greatest spread with the highest outliers. The graph also reveals how intentional attacks are frequent though they are typically smaller in scale. 

<iframe
  src="assets/num_customer_category.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Interesting Grouping
To further understand the relationship between cause category and outage impact, I aggregated the number of customers affected by cause category using the mean, median, and count. One thing I found interesting is how system operability has the largest mean yet the median is only 69,000 showing how large outliers pull the mean up. 

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
From the data set I believe that the `CUSTOMERS.AFFECTED` column is MNAR or Missing Not At Random. This is due to how utilities may be less likely to report customer counts for smaller outages. Along with this many factors go into why the customers affected may not be counted as it all likely comes down to the company reporting the data. 

## Missingness Dependency
In order to test missingness dependency I will focus on the column `OUTAGE.DURATION` and tested whether missingness depends on `YEAR` and `CAUSE.CATEGORY`

### Test 1: OUTAGE.DURATION Missingness vs. YEAR

### Null Hypothesis:
The missingness of `OUTAGE.DURATION` is independent of `YEAR`.

### Alternative Hypothesis:
The missingness of `OUTAGE.DURATION` depends on `YEAR`.

### Test Statistic:
Difference in mean `YEAR` between missing and non-missing groups.

Significance Level: 0.05

### Results: 
From the permutation test with 1000 repetitions I got a p-value above 0.05, so we fail to reject the null hypothesis. This suggests that the missingness of `OUTAGE.DURATION` does not depend on `YEAR` as duration data appears to be missing equally across all years. 

The plot below shows the empirical distribution of the permutation test statistic with the red line showing out observed statistic falling within the distribution, confirming no dependency on `YEAR`

<iframe
  src="assets/missing_year.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

### Test 2: OUTAGE.DURATION Missingness vs. CAUSE.CATEGORY

### Null Hypothesis:
The missingness of `OUTAGE.DURATION` is independent of `CAUSE.CATEGORY`.

### Alternative Hypothesis:
The missingness of `OUTAGE.DURATION` depends on `CAUSE.CATEGORY`.

### Test Statistic:
Total Variation Distance (TVD), since `CAUSE.CATEGORY` is categorical.

Significance Level: 0.05

### Results: 
From our permutation test with 10,000 repetitions we got a very small p-value, far below 0.05, so we reject the null hypothesis. This suggests that the missingness of `OUTAGE.DURATION` does depend on `CAUSE.CATEGORY` or that certain categories are more liekly to have missing duration data than other. 

The plot below shows the empirical distribution of TVD statistics from out permutation test. The red line marks our observed TVD, which falls far to the right of the distribution, meaning that the observed difference in `CAUSE.CATEGORY` distributions between missing and non-missing `OUTAGE.DURATION` groups is extremely unlikely to occur by chance. This plot also confirms why we reject the null hypothesis visually. 

<iframe
  src="assets/missing_cause_category.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Hypothesis Testing
In order to determine whether certain climate regions experience higher frequency of severe weather outages I performed a permutation test comparing the proportion of severe weather outages across all climate regions. This question is important since different regions across the U.S. have vastly different weather patterns, infrastructure, and geographic characteristics. If certain regions experience more severe outages this could help utility companies better handle resources and prepare for these events. 

### Null Hypothesis:
The proportion of severe weather outages is the same across all climate regions and any observed differences are due to chance.

### Alternative Hypothesis:
At least one climate region has a significantly different  proportion of severe weather outages.

### Test Statistic:
The range (max - min) of severe weather outage proportions across climate regions. This is a good choice because we are looking for any difference across multiple groups, and the range captures the largest spread between groups.

Significance Level: 0.05

### Results: 
After performing the permutation test, the p-value was consistently below 0.05 so we reject the null hypothesis. This suggests that severe weather outage rates are not equally distributed across climate regions. This makes sense as regions with more extreme weather events may be more prone to severe weather outages versus a more stable environment. 

The plot below shows the empirical distribution of out permutation test statistic with the red line marking out observed test statistic far in the tail of the distribution

<iframe
  src="assets/hypothesis_severe.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

# Framing a Prediction Problem
The predictions problem I will explore is **Can we predict the duration of a major outage given information available at or near the time the outage begin?**

This problem is a regression problem since I am predicting `OUTAGE.DURATION`, a continuous value measured in minutes. I chose `OUTAGE.DURATION` as the response variable as it is the most direct and measurable indication of outage severity. Longer outages have greater impacts on customers, businesses, and essential services.

The metric I am using to evaluate my model is the RMSE (Root Mean Squared Error). RMSE is relevant here since it penalizes large prediction errors more heavily, which matters in this context as severely underestimating a long outage has consequences. 

At the time of prediction the features available include:`CAUSE.CATEGORY`, 
`ANOMALY.LEVEL`, `CLIMATE.REGION`, `MONTH`, `TOTAL.CUSTOMERS`, `URBAN`, and `TOTAL.PRICE`. All of these features describe conditions at or before the start of the outage making the valuable. `OUTAGE.DURATION` 
itself would not be known at prediction time, which is why it is our target.

# Baseline Model

My baseline model is a linear regression model that predicts `OUTAGE.DURATION` using three features: `CAUSE.CATEGORY` (nominal), `CLIMATE.REGION` (nominal), and `ANOMALY.LEVEL` (quantitative). All the steps for the model were implemented using a single sklearn pipeline with a ColumnTransformer for preprocessing. THe two categorical features were encoded using OneHotEncoding, which creates a binary column for each unique category value filled with 1 if the row belongs to that category and 0 otherwise. THe quantitative feature `ANOMALY.LEVEL` was passed through as is since it is already a numerical feature. 

I choose these three features because: 

- `CAUSE.CATEGORY`: The cause of an outage is one of the strongest indicators of how long an outage will last. For example, severe weather outages tend to involve widespread damage that takes much longer to repair compared to intentional attacks or equipment failure. Both of which may be more localized and quicker to resolve. 

- `CLIMATE.REGION`: Different climate regions across the U.S. have vastly different weather patterns, infrastructure, and geographic characteristics. Regions that experience more extreme weather such as the Northeast or Southeast may have longer outages due to more damage while more stable regions may have less damage. 

- `ANOMALY.LEVEL`: This feature describes the El Niño/La Niña climate index at the time of the outage. Higher anomaly levels reflect more extreme climate conditions which could lead to more severe weather events and therefore longer outages. 

| Metric     | Score           |
|------------|-----------------|
| Train RMSE | 5331.66 minutes |
| Test RMSE  | 6326.16 minutes |

The baseline model is not very strong as predictions are off by about 6,326 minutes on unseen data. This gap between train and test RMSE suggests that the data is over fitting. However, this is expected with simple linear regression models with only three features, as linear regression assumes a linear relationship between features and the target. THis may not hold for complex outage data. This leads me to conclude that more features will help our model.

# Final Model
For my final model I upgraded to use a Random Forest Regressor and added four new features. Unlike linear regression, Random Forest is much better at describing complex non-linear patterns in data making it better for predicting outage duration. 

- `MONTH` (ordinal): The month an outage occurred in describes seasonal weather patterns that may influence outage duration. For example, winter months may have longer outages due to ice storms and freezing temperatures that make repairs more difficult and dangerous.

- `TOTAL.CUSTOMERS` (quantitative): States that serve more customers tend to have larger and more complex power grids. When an outage occurres in a state that is more populated, the outage may take longer to repair and restore. 

- `URBAN` (quantitative): Similar to the last feature, highly urban areas have a more dense and interconnected infrastructure. This leads to longer outage duration due to the large space workers may need to repair. At the same time, urban areas may have more utility and crews available to repair power grids. 

All quantitative features were scaled using StandardScaler to normalize their ranges and all steps were implemented in a single sklearn Pipeline. Before I tuned the hyperparameters I decided to use the two hyperparameters below:

- `max_depth`: Controls how deep each decision tree grows.

- `n_estimators`: The number of trees in the forest. 

I used `GridSeachCV` with 5-fold cross validation to search over the following values:

| Hyperparameter | Values Tested   | Best Value |
|----------------|-----------------|------------|
| max_depth      | 5, 10, 15, None | 5          |
| n_estimators   | 50, 100, 200    | 200        |
 
The best hyperparameters I found were `max_depth = 5` and `n_estimators = 200`. The smaller depth of 5 shows that the relationship between out features and outage duration is best captured with simpler trees to avoid overfitting. 

| Metric     | Baseline        | Final Model     | Improvement     |
|------------|-----------------|-----------------|-----------------|
| Train RMSE | 5331.66 minutes | 3736.24 minutes | 1595.42 minutes |
| Test RMSE  | 6326.16 minutes | 6146.37 minutes | 179.79 minutes  |

The final model improved test RMSE by about 180 minutes compared to the baseline model. While this improvement seems small, it shows that the additional features and the Random Forest algorithm were able to better capture patterns in the data compared to linear regression. The train RMSE of 3736 minutes compared to the test RMSE of 6146 minutes suggests some overfitting which is common with Random Forests on smaller data sets. Though the improvement overall confirms that the final model generalizes better than the baseline model.  

# Fairness Analysis

For my fairness analysis I examined whether my model performs differently for severe weather outages compared to non-severe weather outages. This is an important comparison because severe weather is the most common cause of major outages in our dataset. In fact, they make up about half of all recorded causes. If the model performs worse for one group over the other, it could lead to inaccurate duration predictions that misguide utility companies in their resource allocation and/or preparedness. 

I sepreated the test data into two groups of severe weather outages (`CAUSE.CATEGORY == 'severe weather'`) and all other categories. 

## Null Hypothesis:
Our model is fair. Its RMSE for severe weather outages and non-severe weather outages are about the same and any differences is due to random chance.

## Alternative Hypothesis
Our model is unfair. Its RMSE for severe weather outages is significantly different from its RMSE for non-severe weather outages.

## Test Statistic
The absolute difference in RMSE between the two groups. We use RMSE as out evaluation metric since this is a regression model and absolute difference lets us capture the meaningful gap in performance between two group no matter what direction. 

## Results:

| Group                        | RMSE            |
|------------------------------|-----------------|
| Severe Weather               | 4845.21 minutes |
| Other Causes                 | 7238.29 minutes |
| Observed Absolute Difference | 2393.09 minutes |

I performed a permutation test with 1000 repetitions with a significance level of 0.05. I got a p-value of 0.381, which is well above out significance level, so we fail to reject the null hypothesis. This suggests that the difference in RMSE between severe weather and non-severe weather outages is not statistically significant and could be due to chance. Our model seems to appear fairly across both groups. Our model also performs better on severe weather outages likely due to more training examples. 

The plot below shows the empirical distribution of our permutation test statistic with the red line marking our observed absolute difference. The observed value falls within the distribution confirming our results.  

<iframe
  src="assets/fairness_permutation.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
