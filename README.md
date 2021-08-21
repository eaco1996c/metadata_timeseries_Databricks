# metadata_timeseries_Databricks
This is a metadata project with **~2.65 Million** records and 17 attributes. The dataset is from Kaggle by 
[**Danilo Lessa Bernardineli**](https://danlessa.github.io/), accessed from https://www.kaggle.com/danlessa/brazilian-bird-observation-metadata-from-wikiaves#.

This project is processed through Spark and written in both SQL and Python. The platform used is DataBricks. The **DataBricks Community Edition** is able to accomplish this project. And the cluster is configured with **version 8.3** (includes Apache Spark 3.1.1, Scala 2.12) and 15 GB memory and 2 cores.

![image](https://user-images.githubusercontent.com/38795845/130290658-6bf1c5ea-e53f-4123-b062-ad131a38513e.png)

Following are extra libraries installed to the Cluster:

![image](https://user-images.githubusercontent.com/38795845/130290711-8b0f634a-3a8e-4114-a5d5-708e0d5279da.png)

## Dataset Description

This dataset is captured from [Wikiaves](https://www.wikiaves.com.br/#), a Brazil bird dicovery record website. 
![image](https://user-images.githubusercontent.com/38795845/130290824-941cf6d9-a339-4c5b-90aa-a7a68db6f7db.png)

![image](https://user-images.githubusercontent.com/38795845/130290903-02d0a738-00aa-4f9f-b12a-0e9dfd91c20a.png)

Every record has 17 attributes, around the bird discovered and author of the record and discovery information like time, date and location. The same species could be discovered multiple times. Birds discovered are not unique values. Neither are the authors. One author could write many posts. 

Following is what the dataset looks like:
![image](https://user-images.githubusercontent.com/38795845/130291732-4ee90cc5-505a-4001-a370-d7747266922a.png)
![image](https://user-images.githubusercontent.com/38795845/130291795-00edb1ca-0e3a-436a-82f6-2c340028bc6e.png)

From the dataset, there are 1942 unique species recorded in this dataset. And the attributes are:
- integer index(index)
- author_id(unique id for authors)
- registry_id(unique id for every registry
- comment_count(Registry comment count)
- registry_date(Author given registry date)
- is_large(Large file identifier (0 or 1))
- location_id(Location identifier (IBGE code))
- is_flagged(Registry was reported (0 or 1))
- like_count(Registry likes count (int))
- registry_link(Registry URI)
- location_name(Location name (Municipality / State))
- views_count(Registry views count (int))
- home_location_id(Author hometown location identifier (IBGE code))
- species_id(Species unique identifier)
- scientific_species_name(Species scientific name)
- popular_species_name(Species popular name)
- species_wiki_slug(Species slug)

## Methodology
Two parts of analysis are going to be involved: Exploratory Data Analysis, Time Series Analysis.

### EDA 
1. Monthly total records of authors and species(Both unique):

![image](https://user-images.githubusercontent.com/38795845/130304723-68a6d9f2-1881-46c5-827a-ad0fd4a79903.png)

The number of authors has been on an upward trend since 2008, and the upward trend is noticeable. Although the number of birds has also shown an upward trend since 2008, the upward trend is not apparent. Furthermore, the number of authors has dropped significantly since 2017, and the number of birds has no evident downward trend from 2017 to 2019. These two phenomena may mean that the number of birds has reached the top limit around 2017. In fact, authors' numbers have been on the rise, but the number of birds photographed has declined after 17 years, which could mean that environmental damage in South America is making birds harder to find.

2. The heat maps year 2008 - 2019 of the 20 locations with the largest number of bird species(Unique):

![image](https://user-images.githubusercontent.com/38795845/130304828-7f21e1d7-32da-4ef8-a6b7-cd097b65a320.png)

Next, we look at the distribution of the number of birds by geographical location. Because there are more than 5,000 locations in the dataset, we selected the top 20 locations with the most significant number of bird species found. Judging from these 20 locations, some places have reduced since 2014. For example, Ubatuba/SP found 392 species in 2013 and only 280 species in 2018. Note that the data for 2019 is not meaningful because it is less than a year. However, some locations found that the number of birds is on the rise. For example, Parauapebas/PA In general, it is estimated that the environment in many places is deteriorating. Then the birds are migrating to places where the living environment is good, but at the same time, the overall species of birds has decreased. Some of them may be extinct.


3. Top 20 largest species in Brazil and number before 2017 & after 2017:

![image](https://user-images.githubusercontent.com/38795845/130304907-6e8acdf0-50e3-4c92-a0c6-aba93568a7d0.png)

![image](https://user-images.githubusercontent.com/38795845/130304988-8fcb48a6-fdb1-41fb-b592-83b6f5d2ea20.png)

![image](https://user-images.githubusercontent.com/38795845/130304892-534cfa64-f903-4dad-91a0-b600a541230a.png)

Because we know that the living environment of birds is deteriorating with 2017 as the dividing line, let us look at the difference in the number of times each bird was found in 2016 and 2018. There are more than 1,500 species, so we choose the 20 species that have been recorded the most and the least. The number of records for the top 20 birds has also dropped significantly after 2017. Regrettably, the rare birds disappeared directly after 2017.

### Time Series Analysis
1. Model & Forecast

     To discuss the relationship between the count of total bird discoveries and time, a time series prediction model is included. The procedure used is the fbprophet created by Facebook in 2017. It is a powerful time series analysis tool. It normally uses an addictive linear fitting theorem to fit the data, adjusting weights of participating parameters such as “which day in the week”, “if it is a holiday”. It is designed to predict at a daily scale, but it could be extended to longer time period prediction by adjusting parameters. Through time series analysis, a general trend of the total perching bird in Brazil could be generated. 

    To utilize this method, we choose the “registry_date” as the “ds” value and the count of all “registry_id” of that “registry_date”as the “y” value which are recognized by the FBProphet model. To prepare the data, a query is called to generate a new table for “registry_date” and count of “registry_id”, then set the data ordered by the date in an increasing direction. The spark dataframe has to be transferred to a pandas dataframe and the count value “y” needs logarithm processing to reduce the influence of increasing scale of data. 

![image](https://user-images.githubusercontent.com/38795845/130305046-013be2e6-5b5b-40b1-a917-fd87a7500808.png) -->
![image](https://user-images.githubusercontent.com/38795845/130305052-8b5f5b0e-9316-4795-aac9-9064386d29e1.png)

- Forecast Plotting

![image](https://user-images.githubusercontent.com/38795845/130305071-6601833b-4259-476b-9344-87c6c6095f83.png)

The x axis is the date, and the y axis is the log(total records of that date). The black spots are actual data points. The blue line is the prediction line. Vertical red lines are dates which are change points of the total trend. The red curve is the general prediction trend curve. 

From the plot, the right section with no black points is the prediction section from 2019-8-11 to 2021-8-10. The trend is clearly shown. The bird discoveries increased rapidly from 2008 to 2011, then increased slower between 2011 to mid 2016, and turned to decreasing from mid 2016 to the end. Through the change points, which refers to points where the curvature of the trend curve changes, the increasing speed is reducing in the whole growth progress, and the downward velocity is accelerating in 2017. The growth until mid 2016 should be resulted from an increasing number of participating authors. The reduction from mid 2016 may indicate a decline of total perching birds. The 2017 change point with an accelerating reduction speed corroborated this guess.


2. Cyclical Trends

![image](https://user-images.githubusercontent.com/38795845/130305087-6ca8bfee-7187-45c3-b928-54548dc20131.png)

   For time series analysis, periodic trends are always an essential topic. The cyclical parameters are important attributes of a date. From the weekly trend plot, Mondays have the least new registries with a weight in prediction model below -0.05. Saturdays and Sundays have distinct weights both beyond 0.075, the weight of Saturday even approaches 0.1. It is obviously impacted by people's behavior. Because authors have more leisure time to search for birds and to register. March is the month with the least prediction weight of -0.06, that reason is probably because it is the summer time, the hottest month (with the highest average temperature) in Brazil, a southern hemisphere country. Birds and authors are both less active in extremely hot weather, causing the registry valley in March. July has the most comfortable average temperature of 21.8 celsius so that it gains a high weight of prediction. The January high weight may be caused by dense holidays in that month, which means more spare time for bird discovery. From the daily trend, an interesting phenomenon is revealed, that time spots with the highest weights are the time just before sunrise and just after sunset. That could inspire further analysis on the habitat of birds.  

3. Cross-validation this model

   A cross validation analysis is introduced to check how well the time series model fits the data. The metric is about to pick an interval of dates in existing data and predict the counts of the dates. Then we compare the predictions and the actual counts by the mean absolute percent error (MAPE). The MAPE should be a low score when the model fits the data well. 

![image](https://user-images.githubusercontent.com/38795845/130305167-e284520a-bfd4-47d3-a2a9-df04056bd9bd.png)Cross-validation processing

![image](https://user-images.githubusercontent.com/38795845/130305170-e0b2ba2a-e822-4302-a421-85dc511a5f79.png)Prediction on chosen interval

![image](https://user-images.githubusercontent.com/38795845/130305173-c7b79314-c161-48d9-bf6f-2208e19e3405.png)Metric result generating


   The chosen interval for cross validation is between 2010-03-26 and 2018-08-11. The figure is showing the mean absolute percent error from predictions to actuals by spots. The blue line is the computed regression of these MAPE. From the blue line, the MAPE is around 0.03, the mean absolute difference from y predictions to y actuals equals ~0.03% of y actuals, which is a relatively low error for prediction models. It indicates this time series model fits the data well.

![image](https://user-images.githubusercontent.com/38795845/130305147-a533be0d-a339-4f76-a7b8-237b666c316b.png)

