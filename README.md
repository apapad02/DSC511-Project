# DSC511 - Steam Reviews Project 

### Project Overview: 

We selected a dataset about Steam reviews in 2021 from Kaggle. You can see the link below: 

https://www.kaggle.com/datasets/najzeko/steam-reviews-2021  

The dataset about Steam reviews is fascinating because it encapsulates a wealth of information on consumer preferences, behaviours, and sentiments within the vast PC gaming community. It serves as a rich corpus for sentiment analysis, enabling the development of advanced recommendation systems and the assessment of game features that resonate with or disappoint players. Additionally, this dataset can provide valuable feedback for developers and publishers, offering a direct line to consumer opinions on game performance, features, and potential improvements. We found the dataset interesting because, as students with programming backgrounds we are intrigued by video games both from a programming aspect but also from a “gamer” perspective. Part of our challenge was also to view how the sentiment analysis will work in an environment where people use irony, jokes, and use of slang especially since we expect the average steam user to be of a younger age. 

Our main subject of interest is the text analysis of the review combined with the actual review (positive or negative).  Using machine learning algorithms, we try to predict the recommended feature. Also, we create a small recommendation system and perform graph analysis on our dataset. 

### Dataset Description: 

After the selection of only English as language the data set has 7300865 rows and 23 columns. A brief description for each column: 

- **_c0**: Represents a unique identifier or sequential index for each row in the dataset. 

- **app_id**: Contains the unique identifier for each application or game reviewed. 

- **app_name**: The name of the application or game that has been reviewed. 

- **review_id**: A unique identifier for each individual review. 

- **language**: The language in which the review has been written. 

- **review**: Contains the actual text of the review left by the user. 

- **timestamp_created**: Unix timestamp indicating when the review was originally created. 

- **timestamp_updated**: Unix timestamp showing when the review was last updated. 

- **recommended**: Boolean value indicating whether the reviewer recommends the game or application. 

- **votes_helpful**: Number of times other users have found this review helpful. 

- **votes_funny**: Number of times other users have found this review funny. 

- **weighted_vote_score**: A score that represents a weighted measure of the review's helpfulness or quality. 

- **comment_count**: Number of comments other users have left on this review. 

- **steam_purchase**: Boolean value indicating whether the reviewer purchased the game on Steam. 

- **received_for_free**: Boolean value indicating whether the reviewer received the product for free. 

- **written_during_early_access**: Boolean value showing whether the review was written while the game was still in early access. 

- **author_steamid**: Unique Steam ID of the reviewer. 

- **author_num_games_owned**: Number of games owned by the reviewer on Steam. 

- **author_num_reviews**: Number of reviews the user has written on Steam. 

- **author_playtime_forever**: Total playtime in minutes the reviewer has spent on the game being reviewed, across all time. 

- **author_playtime_last_two_weeks**: Total playtime in minutes the reviewer has spent on the game in the last two weeks. 

- **author_playtime_at_review**: Playtime in minutes that the reviewer had at the time of writing the review. 

- **author_last_played**: Unix timestamp of the last time the reviewer played the game. 

 

### Preprocessing Steps: 

- **Data Mounting:** The dataset is stored and accessed from Google Drive. 

- **Data Loading:** The data is loaded into a Spark DataFrame using the `spark.read.csv` method with options to recognize headers and infer data types automatically. 

- **English language only:** Our data set contains reviews from multiple languages, so we decide to investigate only the English reviews which we are more familiar. 

-  **Missing values and Duplicates:** We removed any missing values and duplicates. 

- **Data Files:** We converted the csv file to parquet file to reduce the execution time. If you want to do the process, you have to use the csv file from the link. The commands for transforming the file to parquet are inside the ipynb file.

- **Stratified sampling:** We ensured that each subset of the dataset (each game) maintains the same percentage of samples of each class as the original dataset. In other words, it helps us preserve the proportion of classes across different subsets. Stratifying is particularly important in situations where the target variable (and or another feature) in a dataset is imbalanced, meaning some classes are underrepresented compared to others. 

- **Convert data types to numeric:** All the columns had a string data type and we converted them to numeric format. 

### Analytical Approaches: 

- **Feature selection:** We fit our model using Random Forest, Decision Tree and Gradient Boosted Trees Classifiers and we calculate the feature importances. We find that the 3 most  important features are 'weighted_vote_score', 'votes_helpful' and 'author_playtime_forever'. 

- **Binary Classification:** 
We use 3 algorithms for the binary classification. The 3 algorithms are Logistic Regression, RandomForestClassifier and DecisionTreeClassifer. We compare the machine learning algorithm’s based on the F1 scores because F1 score is more robust when we deal with imbalanced data. It is important for us to make a good prediction also for the minority class which is the 'not recommended'. At our first attempt we had a very low F1 scores.   
To address the issue of the imbalanced response variable we create class weights and try to improve the metric's scores.  Using class weights helped to improve the F1 scores. 
Then, we perform Grid Search Cross Validation with 5 folds (K=5) for our 2 best models according to the F1 Score. The 2 best models are Random Forest and Decision Tree Classifiers. The 2 best models are Random forest classifier and Decision Tree Classifiers. 
The best model for the binary classification problem to predict if a user recommended or not the app is using the Random Forest Classifier with these hyperparameters: Number of trees to train: 20 and Maximum depth of the decision trees: 5

- **Text Analysis:** We begin the text analysis part by cleaning the review column and creating the pipeline. Firstly, we split the sentences in set of words using the tokenizer and we remove the stopwords using the remover. Also, HashingTF is used to map a set of words to fixed-length feature vectors, where each word is represented as a feature and the value of the feature corresponds to the frequency of the word in the sentence. The IDF is then computed to determine the inverse document frequency, which forms part of the TF-IDF measure, emphasizing the importance of a word based on its rarity across documents. Then the Logistic Regression is applied as a classification algorithm to predict categorical outcomes. Finally, our text is pushed through the pipeline, and we evaluate our metrics before transforming our original DataFrame to consist of the new columns. 

- **Sentiment Analysis:** The project involves analyzing the textual content of the reviews to categorize them as positive or negative. This way we quantify the sentiment of each review based on its context. 

- **Recommendation:** We take the nouns of each review using a POS tagging method, this way we can perform TF-IDF on the nouns. This way each game is characterized by a set of nouns. We used the ALS algorithm to recommend to each author some apps that may he likes. 

- **Graph Analysis:** Beyond traditional analysis, the project explores graph analysis to understand the relationships and network effects within the data. We build our graph using the video games and the review authors as Vertices, and the reviews themselves as Edges, and using our target variable as the weight. However, due to the datasets great sparsity it is difficult to build well-connected graphs. So, we continued our graph analysis attempts for clearly research purposes.  

 

### Tools and Libraries Used: 

- **PySpark:** Utilized for data handling and processing, including DataFrame operations and machine learning. 

- **TextBlob and VaderSentiment:** These libraries are used for conducting sentiment analysis, extracting sentiments from the review texts. 

- **GraphFrames and NetworkX:** Employed for conducting graph-based analysis on the dataset. 

 

### Significant Findings: 

From the Exploratory Data Analysis (EDA) we observe that our target variable is really imbalanced because the majority of the ‘recommended’ points is ‘TRUE’.  The most reviews have zero votes helpful, zero votes funny and zero comments count. Also, the feature votes helpful and the comment count are highly correlated and the features author playtime at review and the author playtime forever are also highly correlated. 

The feature selection showed the 3 most important variables are  'weighted_vote_score', 'votes_helpful' and 'author_playtime_forever'.
With class weights our F1 score improved a lot.
The best model of the binary classification is the Random Forest Classifier with these hyperparameters: Number of trees to train: 20 and Maximum depth of the decision trees: 5 

The majority reviews have positive sentiment score which is consistent with our previous findings that most reviews recommend the apps.
The word 'good' has the most occurences in the reviews with positive sentiment score. However the word 'no' has the most occurences in the reviews with negative sentiment score.


We would recommend Steam to reward consistent and insightful reviewers. This would solve many of the connectivity issues which were seen during our graph analysis. Steam can heavily benefit from this because they can get much more information from their users which can translate to improvements and profits. In general, we believe that an increase in reviews (especially meaningful and insightful ones) can help generate more information which yields better results and understanding from an analysis point of view.  

 
