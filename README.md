# An investigation on the Trend of Cooking Time Over Years
**Authors**: Cici Xu 

## Project Overview 
This data science project investigates the relationship between cooking time and year by conducting permutation testings.  This is a course project for DSC80- Practice and Application of Data Science at UCSD. 

## Introduction 
While human beings are stepping into more efficient and fast-paced lifestyle where people seem to spend less time on cooking, there is in fact a  trend of self-care, where people enjoy spending time on cooking themselves to give themselves some space and enjoy time of their own. And the increasing workout rate also brings more people to start spending ime cooking for a healthier diet or making themselves a delicate and cost-effective dishe. **Thus, based on this observation, this project aims to investigate whether people are spenidng more time on cooking throughout the years using dataset from a recipe website.** 

## Datasets in the study 
The main dataset used (Recipes) contains information on 83,782 recipes from 2008 to 2018 on food.com, with 10 columns as shown in the table below.

| Column          | Description                                     |
| --------------- | ----------------------------------------------- |
| name            | Recipe name                                     |
| id              | Recipe ID                                       |
| minutes         | Minutes to prepare recipe                       |
| contributor_id  | User ID who submitted this recipe               |
| submitted       | Date recipe was submitted                       |
| tags            | Food.com tags for recipe                        |
| nutrition       | Nutrition information in the form [calories, total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for "percentage of daily value" |
| n_steps         | Number of steps in recipe                       |
| steps           | Text for recipe steps, in order                 |
| description     | User-provided description                        |


Another dataset (Ratings) which contains people's ratings and comments on recipes is also used. This dataset has a total of 731,927 reviews, with 5 columns as listed below:

| Column      | Description             |
| ----------- | ----------------------- |
| user_id     | User ID                 |
| recipe_id   | Recipe ID               |
| date        | Date of interaction     |
| rating      | Rating given            |
| review      | Review text             |


# Cleaning Data and Exploratory Data Analysis 
## Data Cleaning 

1. Left merge Recipes and Ratings datasets together: This is for the purpose of pairing each recipe with its ratings and have a clearer observation. 

2. Fill all zeros in "rating" column with np.nan: Ratings with zeros probably means that people didn't fill in a rating, and the website hence automatically gives a zero in rating. Zeros are therefore replaced with np.nan to indicate missingess of data. 

3. Get the series of average rating per recipe: This step creates a series of average rating per recipe by using groupby on the 'id' column. 

4. Add the series of average_ratig per recipe back to dataframe using transform method.

   After the steps above, I combined the two datasets and get a general overview of the merged data. Since I'm investigating into possible relationship between cooking time and years, columns needed are the 'date' and 'time' columns in the Recipes dataset. Thus, the following steps focus on cleaning the Recipes Dataset and extracting necessary information. 

5. Convert columns in Recipes dataset into correct types: 'id' column is converted from integer to string, and 'date' column is converted from string to datetime object. 

4. Create a "year" and a "month" column.

5. retrieve needed columns "id", year","minutes","date", and put them into a smaller dataframe df.

6. Sort data by year. 

   After data cleaning, dataframe df contains information on recipe ID, year and date each recipe is posted, and the cooking time of each recipe. 
   First five rows of the dataframe df after cleaning are displayed below. 
print(merged[['name','id','minutes','contributor_id','submitted', 'tags', 'nutrition','n_steps','steps','description', 'ingredients','n_ingredients', 'user_id', 'recipe_id	date', 'rating', 'review','rating_per_recipe']].head().to_markdown(index=False))

| Column | Description |
| ----------- | ----------- |
| id	| minutes | year| month | date|
| 0 | 333281 | 40 |	2008 |	10|	2008-10-27|
| 42898 | 317877 | 15 | 2008 |	8 |	2008-08-06|
| 42901 | 313434 | 20 | 2008 |	7 |	2008-07-13|
| 42903 | 300597 | 45 | 2008 |	4 |	2008-04-24|
| 42906 | 327285 | 200 | 2008 |	9 |2008-09-25|


## Univariate Analysis
With cleaned data, we proceed to look at the distribution of the recipes' cooking time and the distribution of years in which recipes were posted. 

1. The Statistics below shows a cooking mean time of around 115 minutes, which is way much higher than the cooking median time of 35 minutes. This combined with the maximum cooking time indicates that there seem to be a lot of huge outliers of cooking time that largely affects the mean value. 

| Column | Description |
| ----------- | ----------- |
| count | 8.378200e+04|
| mean | 1.150309e+02|
| std | 3.990871e+03|
| min | 0.000000e+00|
| 25% | 2.000000e+01|
| 50% | 3.500000e+01|
| 75% | 6.500000e+01|
| max | 1.051200e+06|

2. The Box plot below also shows distribution of cooking minutes with some huge outliers. 
<iframe src="assets/cooking_time_boxplot.html" width=800 height=600 frameBorder=0></iframe>

3. Outliers are then handled by filtering the dataset with a threshold of the 97th percentile of the cooking minutes. 

4. The histogram below shows the  Distribution of number of recipe posted in each Year: From the graph, it can be observed that number of recipes posted have been derecreasing since 2009. It's important to note that the huge difference in number of recipoe posted each year can affect the result we get by looking at the relationship between cooking minutes and year using this dataset. 
<iframe src="assets/recipe_each_year.html" width=800 height=600 frameBorder=0></iframe>

## Bivariate Analysis
1. Histogram of Cooking time by year (with unfiltered data) 
<iframe src="assets/cooking_time_by_year.html" width=800 height=600 frameBorder=0></iframe>

2. Boxplot of Cooking time by year (with filtered data): From the boxplot, median of cooking time seem to remain at 35 minutes from 2008 and 2013, and the median cooking time flucuate to some longer minutes during 2014-2018. 
<iframe src="assets/cooking_time_by_year_box.html" width=800 height=600 frameBorder=0></iframe>

3. Scatterplot of Mean Cooking time by year (with filtered data): This scatter plot shows a very obvious increasing trend of cooking time throughout years from 2013-2014. 
<iframe src="assets/mean_cooking_time_by_year_scat.html" width=800 height=600 frameBorder=0></iframe>


## Aggregate Statistics using a pivot table: 
This pivot table shows the statistics including mean, median, min, max standard deviation, and count of cooking time in each year using a filtered dataset. First five rows of the pivot table are diplayed below. 

print(agg_stats_filtered[['mean', 'median', 'min', 'max', 'std', 'count']].head().to_markdown(index=False))

# Assessment of Missingness
## NMAR Analysis
In the merged datast, the missingess of "review" columns is probably NMAR, that is the missingess of review might due to the variable itself, 
and not related to other columns in the dataset. This is because, people may choose not to give any reivew because they probably don't have anything to comment on. However, this could also be related to other factors not in the dataset, such as people may not leave a review becuase they feel lazy, or because they don't enjoy posting things on social media at all. 


## MAR analysis 
In the merged dataset, there are many missing values in the 'rating' columns, and the missingness of this column data might depend on other columns. I focused on assessing dependency between missingess of rating with two possible column data: minutes and calories by permutation testing. 

### 1. Rating and Minutes (MCAR)
Null Hypothesis: The missingness of rating does not depend on minutes
Alternative Hypothesis: The missingness of rating depend on minutes

To perform permutation testing: I created a new colummn 'rating_miss' to indicate the missingness status of the rating, and shuffled the 'minutes' column for our permutation. Since minutes is numeric, we used the absolute mean difference of minutes of missing rating data and non-missing missting rating data as the test staistics. 

Below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.
<iframe src="assets/test_stats_1.html" width=800 height=600 frameBorder=0></iframe>


As shown in the graph, the p-value we get from the permutation tesing is 0, much greater than our significance level of 5%, so we fail to reject the null hypothesis.Therefore, we conclude that it is highly possible that the missingness of rating does not depend on minutes column.

### 2. Rating and Calories (MAR)
Null Hypothesis: The missingness of rating does not depend on calories (#)
Alternative Hypothesis: The missingness of rating depend on calories (#)

Similarly to perform permutation testing:, I created a new column indicating the missingness of rating, and shuffled this column for permutation. We use the absolute mean difference of calories of minutes of missing rating data and non-missing. 

Below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.
<iframe src="assets/test_stats_2.html" width=800 height=600 frameBorder=0></iframe>


The plot below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics.
As shown in the graph, the p-value we get from the permutation tesing is 0,  significantly less than our significance level of 5%, so we reject the null hypothesis. Hence, we conclude that the missingness of rating depends on the calories column

# Hypothesis Testing
## Permutation Test
Going back to our investigation topic, we are investigating if there is an increasing trend of cooking minutes in recent years. So far, we have  seen in fact seen a genearlly increasing trend of Mean cooking minutes and Median cooking minutes from 2008 to 2018, but observation alone cannot be a good indicator as to whether this trend shows actual change in people’s preference or is the trend merely coincidental. 

### 1. Permutation test on cooking time of year 2008 and 2018. 
Since 2008 and 2018 are most far away from each other, a testing on them can probably show a more obvious result. 
Thus, a permutation test is conducted by shuffling the 'year' column. 
Null Hypothesis: The cooking time of year 2008 is from the same distribution as cooking time of year 2018. 
Alternative Hypothesis: The cooking time of year 2018 is longer than the cooking time of year 2008. 
Significance Level: 5% 
Test Statistics used here is Mean cooking time of year 2018 - Mean cooking time of year 2008


The plot below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics. 
<iframe src="assets/test_stats_3.html" width=800 height=600 frameBorder=0></iframe>


From the graph, p-value = 0 < 0.05, so we can reject the null hypothesis, which mean it's highly possible that the cooking time of 2018 is higher than cooking time 0f 2008. 

### 2. Permutation test on cooking time of years 2008-2013 and 2014-2018.
Since the Permutation test on year 2008 and year 2018 aligns with my hypothesis, I continued to investigate the relationship by categorizing the years from 2008-2018 to two bins and performing Permutation test on distributions of these two bins. I set Bin1: 2008-2013 , and Bin2: 2014-2018. In this way, a more general trend can be investigated, by comparing cooking time of first five years and later five years. 
Similarly, a permutation test is conducted by shuffling the 'bin' column. 
Null Hypothesis: The cooking time of Bin1 is from the same distribution as cooking time of Bin2. 
Alternative Hypothesis: The cooking time of year Bin1 is longer than the cooking time of Bin2. 
Significance Level: 5% 
Test Statistics used here is Mean cooking time of year Bin2 - Mean cooking time of year Bin1. 

The plot below shows the empirical distribution of our test statistics in 1000 permutations, the red line indicates the observed test statistics. 
<iframe src="assets/test_stats_4.html" width=800 height=600 frameBorder=0></iframe>


From the graph, p-value = 0 < 0.05, so we can reject the null hypothesis, which mean it's highly possible that the cooking time of year 2014-2018 is higher than cooking time 0f 2008 -2013. 

# Conclusion:
From the two tests conducted above, I conclude that the observed test statistics in the dataset shows strong evidence against the null hypothesis that cooking time of year 2008 and year 2018/cooking time of year 2008-2013 and year 2014-2018 are from the same distribution. Hence, we reject that there isn’t an increase in cooking time over the decade from 2008 to 2018. Although more evidence are probably needed to get futher conclusions or explanations, this trend is likely due to people's increasing interest in home cook big dishes or increased investment in self-care by cooking. 