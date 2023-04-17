# Offer Acceptance Capstone Project

[(Click here for the actual website used for the project)](https://radumanea23.github.io/UCSDFlockFreightCapstone/)

## Table of Contents
- [Background](https://benduong2001.github.io/capstoneproject.html#Background)
- [What I did](https://benduong2001.github.io/capstoneproject.html#Personal-Contribution)
- [Challenges Faced](https://benduong2001.github.io/capstoneproject.html#Challenges)

## Background {#Background}
* In my university, all data science majors have to complete a 20 week capstone project before getting their degrees in Data Science B.S.
* You will get placed in small, close-knit teams with 4-6 people for industry projects with company mentors, or research projects with professors, on various topics from Biomedicine to Sports.
* I was part of a 6-month capstone project with 3 other students, under the mentorship of Flock Freight, a delivery logistics company.
* The background to Flock Freight's mission is as follows:
  * A company needs to deliver an **order**  from Point A to Point B, but lack their own trucking or delivery services to do so.
  * Trucking Service companies, or "**Carriers**", are willing to give **offers** to deliver any shipments at a given cost
  * Flock Freight is the intermediary, or "Freight Broker", between both parties. 
    * For a given **Company X** in need to deliver **Order X**, a number of **N** carriers - ("**Carrier i**") will give offers ("**Offer i**") to deliver it for a certain cost a.k.a. shipping rate ("**rate i**"), so "i" is 1,2, 3....N.
  * And Flock Freight's questions are: 
    * Knowing just Order X alone, predict N - the number of carriers that will make an offer.
    * Knowing just Order X alone, predict the cheapest offer rate the order might get.
* Flock Freight gave our team data about the orders themselves, which were just: the origin and destination zipcodes of the shipment order,the date of deadline, and physical characteristics about it (i.e. whether it's hazardous, or needs refridgeration).
* After 20 weeks of intense collaboration and learning from Flock-Freight, my team and I constructed a final prediction model for determining if a given offer was worthy of selection. The new model was able to reduce Flock Freight's costs by 9.8%. 
* We also authored a research paper documenting our findings, and attended a project showcase where we presented our work to peers, college faculty, and industry professionals.

## Personal-Contribution {#Personal-Contribution}

**So what did I do?**

  * **Predictive Modelling**
    * Our team's final prediction model was actually a "conglomerate" of 3 prediction sub-models, and I was responsible for **two** of them. 
    * I had to work with building the models, finding more data for it, cleaning the data, feature engineering the data, training the model with it, and improving the model's test accuracy by either feature engineering the data even more or finding even newer data. Like a cycle.
    * These 2 models were:
      * A prediction model for the average of the offer rates for a given order. This would be used as a threshold to label any incoming offer as "cheap" if it is below-average for that order.
          - Final model: Linear Regression. 
          - Final test accuracy: 87%.
      * A prediction model for the standard deviation of the offer rates for a given order. This extends the last average model, labelling any incoming offer as "REALLY cheap" if below-average for that order by a large difference (namely that of the standard deviation). 
          - The initial baseline model had a poor test set accuracy of 57%. A big portion of my time went to **improving** this model's accuracy. Several weeks would be spent (see the next ["Challenges"](https://benduong2001.github.io/capstoneproject.html#Challenges) section) in a cycle of **data-cleaning**, **feature engineering**, **modeling**, **hyperparameter fine-tuning**, **peeking at EDA visuals (such as correlation matrix heatmaps to pinpoint correlated features)**.
          - Final model: Random Forest
          - Final (and **improved**) test accuracy: 67%. 
  * **Geo-Data Enrichment**
    * A lot of this project was geographical in nature, since this company dealt with transportation shipping.
     * The original data already provided by Flock Freight only had **one** geographic feature - the zipcodes of the order's origins and destinations. This column would be the main foreign key to the dimension tables
    * Since I worked with geo-data in my internship and past projects, I was the person in our 4-person team put in charge with 2 roles:
        1. Integrating external, geographic data sources into our project. 
        2. Transforming it to be "usable" or "pandas-dataframe friendly" for my non-geospatial teammates 
    * How I did these 2 role were:
        1. I wrote **ETL** python scripts to extract this geo-data from online government census data sources.
            * Retrival is done by **Socrata API** by Tyler Insights, OR **webscraping** with **BeautifulSoup**, as a sequence of **"ETL Failsafes"** with Python's **Try / Except**.
            * The aforementioned zipcode column would be the **"foreign key"** to these new **"dimension tables"**
        2. Geographic data preprocessing / Feature Engineering.
            * **GeoPandas** is used for geo-data pre-processing. To make the geodata usable as structured data, the zipcodes are mapped to X/Y coordinate columns, with a left-outer-join.
            * But X/Y coordinates weren't enough, so I applied **K-means Clustering** to these zipcode nodes, subdividing into "Regions" suitable for **One-hot-encoding**.
            * Also, I decided to symbolize the order's **"Delivery Route"** by connecting each order's origin / destination zipcode nodes. Further geodata sources and use of GeoPandas's Intersection and Buffer methods, can provide aggregated information about the average population density, rainfall, etc. encountered during the shipping route for that order.
    * **Benefits**: 
        * These supplementary geographic features turned out to be very helpful for the feature engineering, and even influential for improving the prediction models' accuracies, compared to if we only limited ourselves to the pre-existing, non-geographic feature.
            * T-Tests coded in python showed that these features indeed had a statistical significance with respect to the offer rate standard deviation per order.  
        * By including this newfound geo-data, I was also able to let our team incorporate **geographic maps** into our data visualizations
![](images/images_dsc180/maps.png)
* Automating Tasks:
    *  I developed Python scripts to automate nearly all stages of my tasks into an end-to-end pipeline, that could be done in at least **6** commands on the terminal. The pipeline
        * data retrieval from a Socrata API and webscraping from an online data source using BeautifulSoup.
        * Implemented data transformations using geopandas, feature engineering with pandas, and machine learning with sklearn pipelines to train and test multiple ML models.
        * Conducted data integrity tests using custom Python functions to ensure data quality and accuracy.
        * Recorded EDA and model metrics using Python logging and generated visualizations with matplotlib.
        * Automated updating the visuals on own project's website with said generated visualizations and model metrics, while pickling the model
        * Ensured reproducibility and portability of the project by running everything inside a Docker container.
  
![](images/images_dsc180/flowchart.png)

## Challenges {#Challenges}

**Challenges Faced & Overcoming Them**

This real-world project came with real-world **messy data** -full of nuances and imperfections that served as challenges. Not all of them had singular solutions, and needed a combination of several solutions.
* **Handling imbalanced data**: the target column for the standard deviation sub-model was highly imbalanced; it was a severely right-skewed distribution lower-bounded at 0 inclusive. This aspect inhibited the regression accuracy to have scores below even 50%.
  * **Redefining the target column**: in other projects, zero-bounded right-skewed distributions like these can be re-shaped into a bell-curve by log-transformation (Log(x+1)). But in this case, the column was still severely right-skewed after log-normalization. Several different combinations of transformation attempts led to dead-ends, and so I decided to ordinalize the column into 2 binary classes with a median threshold.
  * **Alternative Metrics to Accuracy**: Because of class imbalance, a high test set accuracy would be potentially deceiving since the model would just label everything as the majority class. For this reason, I had to use other metrics to assess the model, such as visualizing the confusion matrix or looking at ROC AUC Scores.
  * **Resampling of the imbalanced classes**. Under-sampling some of the over-represented classes (or vice versa) allowed for more equal representations during training.
  * **Sample weighting**: a reason as to why the target column was distributed like so, was because in many cases, Flock Freight closed many orders by picking offers too early; this meant the target column couldn't truly be representative of real-life data, and that for the observations that were "low" (i.e. on the left side of the distribution close to 0), it was impossible to tell if the value was truly low in real life... or if it could have been a potentially high value that was cut off too prematurely. For that reason, orders that were closed very early were given less trust or weight during the training and error.
* **Improving model accuracy**:
I employed various actions and improved the baseline model's accuracy from being 58% to 67%. These included adding further features, or doing further data-cleaning.
  * Geographic Features: as mentioned previously, adding geographic features helped the accuracy.
  * Time-based Features. 
     
