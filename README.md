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

- EDA 

