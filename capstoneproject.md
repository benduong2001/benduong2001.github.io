# Offer Acceptance Capstone Project

[(Click here for the actual website used for the project)](https://radumanea23.github.io/UCSDFlockFreightCapstone/)

## Overview
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

## What I did
My contributions to the project:
* Our team's final prediction model was a conglomerate of 3 prediction sub-models, and I was responsible for building and training 2 of them.
  * A prediction model for the average of the offer rates for a given order. This would be used as a threshold to label any incoming offer as "cheap" if it is below-average for that order. With just linear regression, I got it to a 87% accuracy.
  * A prediction model for the standard deviation of the offer rates for a given order. This extends the last model, labelling any incoming offer as "really cheap" if below-average for that order by a large difference (of the standard deviation). With random forest classification, the accuracy was 67%.
* I was the person who managed to integrate external, geographic data sources into our project. 
  * The original data already provided by Flock Freight only had zipcode columns as the geographic data.
  * I wrote ETL scripts to extract GIS data from online government census data sources, and integrated them into the pre-existing dataset through geographic data preprocessing with Geopandas. 
  * These supplementary geographic features turned out to be very helpful for the feature engineering, and even strongly influential for the in improving the prediction models' accuracies, compared to if we only limited ourselves to the pre-existing, non-geographic feature.
  * By doing this, I was also able to let our team incorporate maps into our data visualizations.

## Challenges Faced & Overcoming.

This real-world project came with real-world nuances and imperfections that served as challenges. Not all of them had singular solutions, and needed a combination of several solutions.
* **Handling imbalanced data**: the target column for the standard deviation sub-model was highly imbalanced; it was a severely right-skewed distribution lower-bounded at 0 inclusive. This aspect inhibited the regression accuracy to have scores below even 50%.
  * **Redefining the target column**: in other projects, zero-bounded right-skewed distributions like these can be re-shaped into a bell-curve by log-transformation (Log(x+1)). But in this case, the column was still severely right-skewed after log-normalization. Several different combinations of transformation attempts led to dead-ends, and so I decided to ordinalize the column into 2 binary classes with a median threshold.
  * **Alternative Metrics to Accuracy**: Because of class imbalance, a high test set accuracy would be potentially deceiving since the model would just label everything as the majority class. For this reason, I had to use other metrics to assess the model, such as visualizing the confusion matrix or looking at ROC AUC Scores.
  * **Over-sampling of the minority classes**.
  * **Sample weighting**: a reason as to why the target column was distributed like so, was because in many cases, Flock Freight closed many orders by picking offers too early; this meant the target column couldn't truly be representative of real-life data, and that for the observations that were "low" (i.e. on the left side of the distribution close to 0), it was impossible to tell if the value was truly low in real life... or if it could have been a potentially high value that was cut off too prematurely. For that reason, orders that were closed very early were given less trust or weight during the training and error.
* **Model Choices**:
  * As mentioned, the target column had to be re-defined as binary classification. The downside of this is that the model is basically simplified as just predicting "high" vs "low" standard deviation. The baseline model used Logistic Regression, and the final model used Random Forest.
* **Improving model accuracy**:
  * **Geographic Feature Engineering**: as mentioned previously, adding geographic features brought up the accuracy from 59% to 67%.
  
