# Food Recommendation Data Science Project

* This Data Science project explores food related datasets for tasks involving customer recommendation:
* 
* 

## Restaurant Rating Prediction given User Text Reviews
* This sentiment analysis prediction will predict whether a user's rating for a restaurant is negative, given the text data of their google review.
* For this task, deep learning, 
* In-depth Feature Engineering will be all that is needed to transform unstructured text data into explainable and structured, tabular data. 

#### **Data Preparation and Feature Engineering**

* 

#### **Modeling, with Class Imbalance Undersampling**
* Logistic Regression was used. However, the target data had class imbalance where good ratings were over-represented. There are probably some explanations behind this (maybe users tend to be polite or optimistic). To address this, 10 models were trained separately, where each model's training data had to under-sample the over-representative good ratings. These 10 models had each of their coefficients normalized for equal scaling, and each coefficient across the 10 models was averaged, (alongside the intercept).

#### **Results and Evaluation**
* The results on the test data showed that this prediction had a remarkable 79% accuracy in determining if a user would rate a restaurant as good or bad based on their google review; 
* The confusion matrix also showed that the undersampling procedure fixed the class imbalance; had this not been done, the prediction model would have just given almost every review a good rating.

![](images/images_food_recommendation/food_sentiment_confusion_matrix.png) 

* Finally, a peek at the 20 strongest coefficients of the prediction model, and their corresponding words.
* In other words, the presence of these following words in a google review are heavily associated with a user giving a restaurant a **bad rating**. 

![](images/images_food_recommendation/food_sentiment_coefficients.png) 

* This makes sense, as most of these words have negative connotations, and even more impressively, it manages to pick out words that aren't just negative "emotional" words (like worst, or bland)- it also includes words that are otherwise arguably neutral but deemed negative *in the context of food at a restaurant* (overpriced, burnt, dry, cold)
* This demonstrates our intricate feature engineering process was successful in extracting meaningfulness from the unstructured text data of the google reviews.

## User - Restaurant Interaction Prediction 

* This task determines how likely a given user would dine a given restaurant (regardless if their eventual opinion), based on the user's history of previosuly cosen restaurants (if available) and the restaurant's history, or [***collaborative filtering***](https://en.wikipedia.org/wiki/Collaborative_filtering).
* Unlike the previous task, this one will employ more advanced deep learning and pre-trained ML tools. 

* Using Jaccard Similarity in terms of users is not a good option, since it is very disconnected.

#### **Word2Vec**
* Gensim's Word2Vec model can make vector representations for a vocabulary of words.
* One of Word2Vec's fun features is the ability to do "arithmetic" with words. For the custom food dataset vocabulary that we trained Word2Vec on, we expect things like "barbeque - july 4th + thanksgiving" to equal "turkey" (i.e. July 4th is to barbeque, as Thanksgiving is to turkey). 

![](images/images_food_recommendation/food_word_arithmetic.png) 












