# Offer Acceptance Capstone Project

[(Click here for the actual website used for the project)](https://radumanea23.github.io/UCSDFlockFreightCapstone/)

## Table of Contents
- [Background](https://benduong2001.github.io/capstoneproject.html#Background)
- [Personal-Contribution](https://benduong2001.github.io/capstoneproject.html#Personal-Contribution)
- [Challenges](https://benduong2001.github.io/capstoneproject.html#Challenges)

## Background
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
* After 20 weeks of intense collaboration and learning from Flock-Freight, my team and I constructed a final prediction model for determining if a given offer was worthy of selection. The new model was able to reduce Flock Freight's costs by 9.8%. We also authored a research paper documenting our findings, and attended a project showcase where we presented our work to peers, college faculty, and industry professionals.

## Personal-Contribution

**So what did I do?**

  * **Predictive Modelling**
    * Our team's final prediction model was actually a conglomerate of 3 prediction sub-models, and I was responsible for 2 of them. I had to work with building hem, selecting the best data for it, feature engineering said data, training it, and improving the test accuracy with even more feature engineering like a cycle. These 2 models were:
      * A prediction model for the average of the offer rates for a given order. This would be used as a threshold to label any incoming offer as "cheap" if it is below-average for that order.
          - The final model used Linear Regression. 
          - It's final test accuracy or correlation coefficient was 87%.
      * A prediction model for the standard deviation of the offer rates for a given order. This extends the last average model, labelling any incoming offer as "REALLY cheap" if below-average for that order by a large difference (namely that of the standard deviation). 
          - The initial baseline model had a poor test set accuracy of 57%. A big portion of my time went to improving this model's accuracy. Several weeks would be spent (see the next "Challenges" section) in a cycle of data-cleaning, feature engineering, modeling, hyperparameter fine-tuning, peeking at EDA visuals (such as correlation matrix heatmaps to pinpoint correlated features).
          - The final model used random forest, and its final (and improved) test accuracy was 67%. 
  * **Geo-data**
    * I was the person who managed to integrate external, geographic data sources into our project. 
    * The original data already provided by Flock Freight only had zipcode columns as the geographic data.
    * I wrote ETL python scripts to extract GIS data from online government census data sources, and integrated them into the pre-existing dataset through geographic data preprocessing with Geopandas. 
    * These supplementary geographic features turned out to be very helpful for the feature engineering, and even strongly influential for improving the prediction models' accuracies, compared to if we only limited ourselves to the pre-existing, non-geographic feature.
    * These included employing K-means clustering unsupervised ML for segmenting the clients into metropolitan US regionsin terms of order data
    * By including this newfound geo-data, I was also able to let our team incorporate maps into our data visualizations.

## Challenges

**Challenges Faced & Overcoming Them**

This real-world project came with real-world **messy data** -full of nuances and imperfections that served as challenges. Not all of them had singular solutions, and needed a combination of several solutions.
* **Handling imbalanced data**: the target column for the standard deviation sub-model was highly imbalanced; it was a severely right-skewed distribution lower-bounded at 0 inclusive. This aspect inhibited the regression accuracy to have scores below even 50%.
  * **Redefining the target column**: in other projects, zero-bounded right-skewed distributions like these can be re-shaped into a bell-curve by log-transformation (Log(x+1)). But in this case, the column was still severely right-skewed after log-normalization. Several different combinations of transformation attempts led to dead-ends, and so I decided to ordinalize the column into 2 binary classes with a median threshold.
  * **Alternative Metrics to Accuracy**: Because of class imbalance, a high test set accuracy would be potentially deceiving since the model would just label everything as the majority class. For this reason, I had to use other metrics to assess the model, such as visualizing the confusion matrix or looking at ROC AUC Scores.
  * **Resampling of the imbalanced classes**. Under-sampling some of the over-represented classes (or vice versa) allowed for more equal representations during training.
  * **Sample weighting**: a reason as to why the target column was distributed like so, was because in many cases, Flock Freight closed many orders by picking offers too early; this meant the target column couldn't truly be representative of real-life data, and that for the observations that were "low" (i.e. on the left side of the distribution close to 0), it was impossible to tell if the value was truly low in real life... or if it could have been a potentially high value that was cut off too prematurely. For that reason, orders that were closed very early were given less trust or weight during the training and error.
* **Improving model accuracy**:
I employed various actions and improved the baseline model's accuracy from being 58% to 67%. These included adding further features, or doing further data-cleaning.
  * **Geographic Feature Engineering**: as mentioned previously, adding geographic features helped the accuracy. 
      * These included: 
          * The metropolitan region that the origin and destination zipcodes of the order resided in
          * The population density and weather conditions on the route between the 2 locations 
      * I would perform T-Tests in python, which showed that these features indeed had a statistical significance with respect to the offer rate standard deviation per order.  
