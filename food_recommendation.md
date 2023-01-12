# Food Recommendation Data Science Project

* This Data Science project explores food related datasets (unstructured text data) for prediction data analysis tasks involving customer recommendation. Two tasks or "sub-projects" were done:
    * Task 1 is about predicting whether a given user will rate a given restaurant negatively, with their text reviews as input data.
    * Task 2 is about predicting whether a given user likely would visit a given restaurant, using text and images as input data.
       * During the feature analysis, the unsupervised ML code also serendipitously uncovered indirect associations shared amongst users and restaurants (that would have been too subtle for a human analyst to notice), allowing a automated way to divide the users and restaurants demographic into indirect sub-groups, based on the types of restaurants they visit and the types of customers they get -respectively.

* This project will show:
   * Articulately-reasoned Feature Engineering using NLP techniques (Word2Vec and Tf-Idf), to transform unstructured text data into a structured tabular format, so that it can be compatibly trained with linear models as baseline (specifically logistic regression).
   * Data Analysis and results applicable to useful business questions on customer recommendation
   * Succinct and Intuitive Data visualizations
   * A balanced use of both basic machine learning (e.g. Sklearn logistic regression) vs deep learning (e.g. multi-layered neural networks, pretrained models)- nuanced understanding when either should be used appropriately, thereby seeing their pro's and con's.
   * Python code that includes object oriented programming, and Pyspark. It is also designed in a way to be updateable, and extensible for new incoming data

[(Link to Github Repo)](https://github.com/benduong2001/Food-Recommendation)

## **Sub-Project 1: Restaurant Rating Prediction given User Text Reviews**

* For this 1st task, a basic sentiment analysis model will be made and trained to predict whether a user's rating for a restaurant is negative or not, given the text data of their text review.
* In-depth Feature Engineering will be all that is needed to transform unstructured text data into explainable and structured, tabular data. 

### **Data Preparation and Feature Engineering**

1. Preprocessing was done on the review text column like lowercasing, and replacement of non alphanumeric characters with whitespace.
2. TF-IDF vectorization was done on the processed review text column, dropping  NLTK’s English stop-words list. This produced a matrix with column count as the
vocabulary size. 
3. The review rows of the new TF-IDF matrix were grouped separately by the corresponding binary rating category. 
4. Then within each group, the rows were collapsed with a column-wise mean (i. e. np.mean with axis = 0). This produced a new matrix that had a row count of 2 and a column count of vocabulary size.
5. Do a column-wise argmax (i. e. axis = 0) on the new array, and filter only for the columns where the argmax row index corresponds to the category for bad ratings. Use these column indices and the vocabulary mapping of the saved TF-IDF vectorizer object to retrieve the words.
6. The previous steps basically filtered for the words that have a stronger "belongingness" to bad reviews compared to good reviews, using TF-IDF. But now, it was decided that among these words, only 100 is needed. While all of these words have stronger relevance to bad reviews, some are  stronger than others, so these words are sorted by the largest absolute difference (between the 2 rows), and the topmost 100 got picked.
7. With these 100 words, and typical bag-of-words count vectorized matrix was produced upon the original reviews dataset, and serves as the input matrix to the model.

### **Modeling, with Class Imbalance Undersampling**

* Logistic Regression was used. However, the target data had class imbalance where good ratings were over-represented. There are probably some explanations behind this (maybe users tend to be polite or optimistic). To address this, 10 models were trained separately, where each model's training data had to under-sample the over-representative good ratings. These 10 models had each of their coefficients normalized for equal scaling, and each coefficient across the 10 models was averaged, (alongside the intercept).

### **Results and Evaluation**

* The results on the test data showed that this prediction had a remarkable 79% accuracy in determining if a user would rate a restaurant as good or bad based on their text review; 
* The confusion matrix also showed that the undersampling procedure fixed the class imbalance; had this not been done, the prediction model would have just given almost every review a good rating.

![](images/images_food_recommendation/food_sentiment_confusion_matrix.png) 

* Finally, a peek at the 20 strongest coefficients of the prediction model, and their corresponding words.
* In other words, the presence of these following words in a google review are heavily associated with a user giving a restaurant a **bad rating**. 

![](images/images_food_recommendation/food_sentiment_coefficients.png) 

* This makes sense, as most of these words have negative connotations, and even more impressively, it manages to pick out words that aren't just negative "emotional" words (like worst, or bland)- it also includes words that are otherwise arguably neutral but deemed negative *in the context of food at a restaurant* (overpriced, burnt, dry, cold)
* This demonstrates our intricate feature engineering process was successful in extracting meaningfulness from the unstructured text data of the google reviews.

## **Sub-Project 2: Predicting if a User will visit a Restaurant**

* This 2nd task on user-restaurant determines how likely a given user would dine at a given restaurant, based on the user's history of previosuly chosen restaurants (if available) and the restaurant's history of customers. In other words, [***collaborative filtering***](https://en.wikipedia.org/wiki/Collaborative_filtering).
* Unlike the previous task, this one will employ more advanced deep learning and pre-trained ML tools. 

### **Overview**

* The model should take a given user and a given restaurant as input, and output 1 or 0 if the user will visit and eat at that restaurant (or not).
* The user and restaurant are both represented by vectors of the same dimension. These vectors are in turn the compressed summaries of all the information about a given user / restaurant. 
   * The vector for a user is the TF-IDF weighted centroid (i.e. the average vector) of the Word2Vec vector representations of each food-related word that showed up in all of the reviews that the user has written.
   * The vector for a restaurant is the TF-IDF weighted centroid (i.e. the average vector) of the Word2Vec representations of each food-related word that showed up in all of the reviews about the restaurant.
 
### **Word2Vec**

* In order to approximately select only the food-related words, a completely separate dataset was used: a recipe dataset where one of the columns was a list of ingredients for each recipe; While it's impossible to get a universal list of every food-related word that exists, this is sufficient enough to build a bag-of-words. 
* A custom Word2vec model was then trained on this ingredients column. Because of the way the autocorrelation in the ingredients column works, the newfound embedding model should be able to capture relationships between words: words that are associated with one another, will have word2vec vectors that are close to one another. 
* One of Word2Vec's famous features is the ability to do "arithmetic" with words. For the custom food dataset vocabulary that we trained Word2Vec on, we expect things like "barbeque - july 4th + thanksgiving", which basically asks **"If July 4th is to barbeque, then Thanksgiving is to ...?)."**. The top leading words are related to **turkey**. 

![](images/images_food_recommendation/food_word_arithmetic.png) 

* For exploratory purposes, it's also nice to check the word that "represents" a restaurant - which is just the word whose vector representation is closest to that restaurant's vector.
* Sometimes, that representative word is very obvious, being a very frequently appearing word in the reviews (like pizza)

![](images/images_food_recommendation/restaurant_embedding_pizza.png) 

* Other times, it is a subtle word that might not even have appeared in the reviews at all. For this restaurant with only 1 review, it was deemed "mediterranean", even though the word "mediterranean" is nowhere to be found. Only 1 word may have been all that was needed to tip it to that label - "Gyro" (a Greek dish)

![](images/images_food_recommendation/restaurant_embedding_gyro.png) 

* From this, each review was preprocessed, filtered of any non-alphanumeric characters, tokenized, and filtered of any words that didn't exist in the bag-of-words. 
* Then for each restaurant, all of the remaining words for each review they are associated with are turned into a Word2Vec vector using the custom-trained Word2Vec model from earlier, then re-weighted by TF-IDF, and averaged column-wise. The exact same process is done for each user.

### Image Recognition with Pre-trained VGGNET16

* Some of the reviews comes with images of the food submitted by the users; this could be fed through a pre-trained VGGNET16 convolutional neural network, which then labels the object in the image, outputting the most likely possible words it might be. These words are attached to the text review as though it was part of the review.
* The python code accomodates this procedure, but was switched off for the baseline model since it's quite computationally costly.

### **Unsupervised ML: Grouping Apart the Restaurants and Users with K-Means Clustering**

* After doing this, each restaurant and each user should have its own vector representation; This can be mapped to 2D scatterplots (made with TSNE): each point in space on the left scatterplot is a restaurant, while on the right are users. 
* Furthermore, it is possible to group up the vector representations. 

![](images/images_food_recommendation/uncolored_embeddings_scatterplot_.png) 

* By using k-means clustering, I found a way to group the restaurants based on the textual content of the reviews written about it, and group the users by the textual content of the reviews they've written. 

![](images/images_food_recommendation/colored_embeddings_scatterplot_.png) 

* While this is not necessary, it would be insightful to actually see what kinds of groups these mean for restaurants and users. Using TF-IDF on the words that show up in each group's collection of reviews, the leading keywords are extracted. Examples are shown

![](images/images_food_recommendation/keywords_business.png) 

![](images/images_food_recommendation/keywords_users.png) 

* The overarching direction of this project is to connect user-types to restaurant-types. For example, restaurants of Group 4 tends to make desserts (sweets, confectionery, pastries), and user-group 0 tends to eat mostly desserts. 
* **In other words, we are grouping up restaurants by the food they serve, and grouping up the restaurant-goers by the food they review, and hoping that a one-to-one correspondence (if it even exists) becomes visible between restaurant-types and customer-types in terms of food.** This would allow us to recommend the dessert-lovers (as seen above in group 0) to the dessert restaurants (as seen above in group 4). ...And recommend Italian food lovers to Italian restaurants, and etc.
* To see more examples of these restaurant/user "groups" and their "most identifiable keywords", scroll to the [bottom half of the visualization notebook in the github repo](https://github.com/benduong2001/Food-Recommendation/blob/main/src/visualizations/visualization_notebook.ipynb)

### **Prediction Modeling: Binary Classification**

* Finally, the last part is to create a prediction model that answers whether or not a given user is likely to interact with a given restaurant... with the underlying data being based on the types of food that the given restaurant serves and the types of food the user has eaten at other restaurants.
* 2 different model versions will be created and trained towards the same goal of binary classification: a regular single-layered logistic regression with Sklearn; and a Multi-layered Deep Neural Network with TensorFlow
* For this interaction problem, it is necessary to manually create "artificial, unseen" pair; every user-restaurant pair in the dataset is real and observed, so there is no "negative" samples to work with. A sampling procedure was developed that tries to find "negative" (a.k.a. never-before-seen) combinations of users and restaurant. This of course brings into consideration an unspoken assumption that the given dataset is an extremely accurate representation of real life; in other words, we are assuming there is no cases where the artificial pairs we created actually do exist in real life but weren't in the dataset.
* The input matrix is basically concatenating the user and restaurant embeddings from earlier for each pair. Since the embedding sizes for the user and restaurant were both 100, the input matrix's shape is N rows by 200 columns, where N = the number of "real, observed" samples + "artificial, unseen" samples.

**Logistic Regression**

* The baseline model will use logistic regression: in terms of metrics, it's around 63-66% accurate in correctly determining whether or not a given user has visited a given restaurant. Its AUC score is 64%.
* For a baseline model, this score isn't awful, but clearly not good enough either; nevertheless, it is a good starting point open to more adjustments to the model; for example, the embedding sizes of the user and restaurants Word2Vec vector representations can be increased from 100 to 1000 dimensions (for a 2000 dimensional matrix)

![](images/images_food_recommendation/LogReg_Confusion_matrix.png) 

**Multi-Layered Neural Network**

* This model uses 3 layers (2 ReLU, 1 sigmoid), with Binary Cross Entropy Loss.
* For metrics of the Multi-Layered Neural Network as the prediction model, it is around 73% accurate in correctly determining whether or not a given user has visited a given restaurant.
* While this metric is clearly an improvement from the logistic regression, it is not far enough of a jump in improvement. At this stage, it is wise to see if using Deep Neural Networks over Logistic Regression run into diminishing returns.

![](images/images_food_recommendation/NN_Confusion_matrix.png) 


* Overall, this model does not yet have a procedure for never-before user
* This model also must acknowledge several considerations: 
   * The dataset used is only limited to the customers who willingly and manually wrote a review for a restaurant.
   * This dataset lacks the geo-location of the restaurants, which might be essential to this specific prediction task. 


