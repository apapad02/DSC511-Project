# DSC511-Project
To provide a comprehensive overview of your project as described in the Jupyter notebook you uploaded, I'll start by examining the notebook's content. I'll analyze the dataset description, preprocessing steps, analytical approaches, and summarize any significant findings. Let's dive into the notebook to get the details.

### Project Overview:
**Title:** Steam Reviews Project

We selected a dataset about Steam reviews in 2021 from Kaggle. You can see the link below:
https://www.kaggle.com/datasets/najzeko/steam-reviews-2021 The dataset about Steam reviews is fascinating because it encapsulates a wealth of information on consumer preferences, behaviours, and sentiments within the vast PC gaming community. It serves as a rich corpus for sentiment analysis, enabling the development of advanced recommendation systems and the assessment of game features that resonate with or disappoint players. Additionally, this dataset can provide valuable feedback for developers and publishers, offering a direct line to consumer opinions on game performance, features, and potential improvements. We found the dataset interesting because, as students with programming backgrounds we are intrigued by video games both from a programming aspect but also from a “gamer” perspective. Part of our challenge will also be to view how the sentiment analysis will work in an environment where people use irony, jokes, and use of slang especially since we expect the average steam user to be of a younger age.
Our main subject of interest is the text analysis of the review combined with the actual review (positive or negative).  Using machine learning algorithms, we will try to predict the recommended feature.
Also, we will perform graph analysis on our dataset.

### Dataset Description:
Our data set now (with only English as language) has 7300865 rows and 23 columns. A small description for each column:


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
1. **Data Mounting:** The dataset is stored and accessed from Google Drive.
2. **Data Loading:** The data is loaded into a Spark DataFrame using the `spark.read.csv` method with options to recognize headers and infer data types automatically.
Files eksigisi etc
selected only the english
remove missing values and duplicates
parquet file

we created a sample, stratified sampling
convert data types to numeric

(anafernoume kapou kai to exploratory data analysis??????)
Findings from eda
Imbalanced data set

### Analytical Approaches:
feature selection
We decided to fit our model using these 3 features, because they were the top 3 features according to the feature selection methods we tried before.
The important features are : 'weighted_vote_score', 'votes_helpful', 
 'author_playtime_forever'.
binary classification
Here, we use 4 algorithms for the binary classification. The 4 algorithms are: Logistic Regression, RandomForestClassifier, GBTClassifier and DecisionTreeClassifer.
F1 score to compare

class weights!!
As we mentioned before, our response variable is very imbalanced, so here we created class weights to solve this problem and try to improve the metric's scores. The F1 scores which is more reliable when we deal with imbalanced response are very low. It is important for us to predict better the minority class which is the 'not recommended'. 

results
We perform Grid Search Cross Validation for our 2 best models according to the F1 Score. We decide based on the F1 score because it works well also on imbalanced data. The 2 best models are Random forest classifier and Decision Tree Classifiers. We will use K=5 in Cross Validation.

best model and hyperparameters na grapsoume poia einai

text analysis
sentiment analysis, recommendation
graph analysis
1. **Sentiment Analysis:** The project involves analyzing the textual content of the reviews to categorize them as positive or negative. This analysis aims to predict the 'recommended' feature in the reviews.
2. **Graph Analysis:** Beyond traditional analysis, the project explores graph analysis to understand the relationships and network effects within the data.

### Tools and Libraries Used:
- **PySpark:** Utilized for data handling and processing, including DataFrame operations and machine learning.
- **TextBlob and VaderSentiment:** These libraries are used for conducting sentiment analysis, extracting sentiments from the review texts.
- **GraphFrames and NetworkX:** Employed for conducting graph-based analysis on the dataset.

### Significant Findings:
conclusion/insights
