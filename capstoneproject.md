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
* I worked with Nima Yazdani, Radu Manea, and Keagan Benson.
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
  * **Impactful Insights**
    * I managed to personally find several impactful insights that were useful to Flock Freight's business case. Some of these will be mentioned again in the rest of this page, but several are:
       * Performing data analysis with Pandas, and discovering the cheapest delivery offers frequently fell on Thursday, then verifying with statistical hypothesis testing that this pattern is likely existent and not out of random chance.
       * Producing data analysis on delivery data and finding out that:
          * Delivery routes with the cheapest shipments tend to be along the west-coast states or from in and out of Florida.
          * Atlanta was a region of high delivery activity
          * Information about the order's delivery route itself, i.e. the average population density or average weather road conditions (rainfall, temperature) of the counties encountered along the way, did have influence on the volume of delivery offers for that order

  * **Building and Training the Prediction Models and iteratively improving their accuracies**
    * Our team's final prediction model was actually a "conglomerate" of 3 prediction sub-models. I and Nima were responsible for **two** of them. 
    * We had to work with building the models, finding more data for it, cleaning the data, feature engineering the data, training the model with it, and improving the model's test accuracy by either feature engineering the data even more or finding even newer data. Like a cycle.
    * These 2 models were:
      * A prediction model for the average of the offer rates for a given order. This would be used as a threshold to label any incoming offer as "cheap" if it is below-average for that order.
          - Final model: Linear Regression. 
          - Final test accuracy: 87%.
      * A prediction model for the standard deviation of the offer rates for a given order. This extends the last average model, labelling any incoming offer as "REALLY cheap" if below-average for that order by a large difference (namely that of the standard deviation). 
          - The initial baseline model had a poor test set accuracy of 57%. A big portion of the time went to **improving** this model's accuracy. Several weeks would be spent (see the next ["Challenges"](https://benduong2001.github.io/capstoneproject.html#Challenges) section) in a cycle of **data-cleaning**, **feature engineering**, **modeling**, **hyperparameter fine-tuning**, **peeking at EDA visuals (such as correlation matrix heatmaps to pinpoint correlated features)**. All of these eventually improved the model's accuracy and ROC AUC score by ~10%.
          - Final model: Random Forest
          - Final (and **improved**) test accuracy: 67%. 
  * **Geo-Data**
    * A lot of this project was geographical in nature, since this company dealt with transportation shipping.
     * The original data already provided by Flock Freight only had **one** geographic feature - the zipcodes of the order's origins and destinations.
    * Since I was the main person in the team with at least prior familiarity with geo-data in my internship and past projects, I had to do 2 tasks:
        1. Procuring external, geographic data sources for our project. 
        2. Transforming it to be "usable" or "pandas-dataframe friendly" and then pass the work to my other teammates so they could do more analysis on it.
    * How I did these 2 tasks were:
        1. I wrote **ETL** python scripts to extract this geo-data from online government census data sources.
            * Retrival is done by **Socrata API** by Tyler Insights, OR **webscraping** with **BeautifulSoup**, as a sequence of **"ETL Failsafes"** with Python's **Try / Except**.
            * The zipcode column  mentioned earlier would be the **"join key"** to these new "dimension tables"
        2. Geographic data preprocessing / Feature Engineering.
            * **GeoPandas** is used for geo-data pre-processing. To make the geodata usable as structured data, the zipcodes are mapped to X/Y coordinate columns.
            * But X/Y coordinates weren't enough, so I applied **K-means Clustering** to these zipcode nodes, andd thus grouping them into "Regions" suitable for **One-hot-encoding**.
            * Also, I decided to symbolize the order's **"Delivery Route"** by connecting each order's origin / destination zipcode nodes. I brought in further geodata sources and used GeoPandas's Intersection and Buffer methods to aggregate different information about the counties that the delivery routes crossed through. These included average population density, rainfall, etc. encountered during the shipping route for that order.
    * **Benefits**: 
        * These supplementary geographic features turned out to be very helpful for the feature engineering, and even influential for improving the prediction models' accuracies, compared to if we only limited ourselves to the pre-existing, non-geographic feature.
            * T-Tests coded in python showed that these features indeed had a statistical significance with respect to the offer rate standard deviation per order.  
        * By including this newfound geo-data, I was also able to let our team incorporate **geographic maps** into our data visualizations
![](images/images_dsc180/maps.png)
* **Automating the data science tasks of the 2 models with Python**:
    *  As We had to improve the model's accuracy by routinely adding and cleaning new data or retransforming old data then re-training it, it was repetitive to re-run the tasks again and again through Jupyter Notebook to find which different configuration of factors boosted the model accuracy. And so I developed Python scripts to automate nearly all stages of my tasks into an end-to-end pipeline, that could be done in at least **6 to 8** commands on the terminal that can be put into a shell script, so that when those 6-8 commands were executed, the pipeline automatically did the following things:
        * Data retrieval from Socrata API and webscraping from online data sources using BeautifulSoup.
        * Data transformations using geopandas, feature engineering with pandas, and machine learning with sklearn pipelines to train and test multiple ML models.
        * Data integrity tests using custom Python functions to ensure data quality and accuracy.
        * Recorded EDA and model metrics using Python logging and generated visualizations with matplotlib.
        * Updating of the image assets on own project's website with those generated visualizations.
        * Reproducibility of the project was ensured by creating a Docker Image, and containerizing our group's work.
  
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
  * **Geographic Features**: as mentioned previously, adding geographic features helped the accuracy.
  * **Time-based Features.** 
     
