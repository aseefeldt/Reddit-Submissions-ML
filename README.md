## OVERVIEW 
There is money to be made on Costa Rica's Tourists.  The economy is getting better (ish), COVID restrictions are up, and people still have cabin fever.  

The problem is finding tourists.  In the before-times (pre-Covid), word of mouth and passive advertising worked well.  Tourists expected to find attractions through their hotel, or host, or just waited to see what was available when they got there.  However, now everyone (even grandparents) knows how to use the internet, and with everyone working at home, they have time to plan their trip (daydream about Costa Rica when they should be working).   

Life changed with COVID, but the way most Costa Rica tourist businesses advertise has not.  To find greater success in this sector, companies must go to the tourist while they are daydreaming at their desks.  We all know the best way to do this is through Social Media.  

Additionally, Costa Rica has more and more perpetual tourists and digital nomads that spend extended amounts of time in Costa Rica.  This population can be difficult to distinguish from tourists, but this is a vital distinction to make as we can leverage this population to become marketing partners.  By distinguishing these two populations we can better target our advertising.  

## PURPOSE
The point of this project is to use social media posts to find tourists that we can digitally market to with social media while identifying digital nomads that can become marketing partners.  I seek to distinguish between the two populations with an 80% accuracy using text vectorization with K-Nearist_Neighbors, Logarithmic Regression, Naive Bayes, Random Forest, and SVC.  I will split the data into test and train components and take the accuracy score of the test data to evaluate my models.  

## DATA DICTIONARY
| Feature | Type | Description |
|---|---|---|
|title|object|Title of the Reddit post| 
|self_text|object|The body or text of the Reddit post| 
|subreddit|object|The name of the subreddit that the post originated on| 

## Executive Summary
Data was gathered from two subreddits.  The Costa Rica subreddit is an English-speaking sub for tourists, locals, and ex-pats.  It is periodically Expats and long-term tourists looking for information about living in a new culture rather than traditional tourist questions around finding leisure activities.  The second subreddit is Costa Rica Travel.  As the name implies this is mostly for tourists looking for travel information.  

Posts were imported by using the PRAW API.  As time data has not been working and there is currently a Reddit Civil War, I pulled the top 1,000 posts from new, hot, top, and controversial categories.  I pulled the post title, the post, and the subreddit it came from.  I removed duplicates based on the post text, removed NA values that appeared from converting the information into a CSV file (those NA values seem to be Reddit advertising), and combined these posts into one data file.  I converted all letters to lowercase and combined common compounds words with an underscore (San_Jose, La_Fortuna, and Costa_Rica)

I explored the length of the posts and the top words used.  There appear to be some patterns, but nothing that jumped right out at me as an obvious way to distinguish the two data sets.  I experimented with adding various stop words (mainly Spanish words) but no obvious pattern emerged.  

The KNN model was grid-searched with stopwords (none, English, custom, and English + custom), preprocessing (none, Lemmatize, and Stem), Ngrams (1, 1:2, 1:3, 2:2), K-Neighbors (3, 5, 7, 10).  The best performance was achieved with Custom stopwords, Stemming, Ngram of 1:1, and 7 K-neighbors.  The accuracy score was 56.69% compared to a baseline of 57%.  

The Logarithmic Regression model was grid-searched with stopwords (none, English, custom, and English + custom), preprocessing (none, Lemmatize, and Stem), Ngrams (1, 1:2, 1:3, 2:2), Max Iteration (100, 500) and Regression (Lasso, Ridge, and Elastic Net).  The best performance was achieved with Custom and English stopwords, Lemmatize, Ngram of 1:3, Max Iteration of 100, and by using Ridge preprocessing.  The accuracy score was 68.42% compared to a baseline of 57%. 

The Naive Bayes model was grid-searched with stopwords (none, English, custom, and English + custom), preprocessing (none, Lemmatize, and Stem), Ngrams (1, 1:2, 1:3, 2:2), and Alpha (0.0, .075, 1).  The best performance was achieved with English stopwords, no preprocessing, an Ngram of 1:1, and an Alpha of .75.    The accuracy score was 68.42% compared to a baseline of 70.77%. 

The Random Trees model was grid-searched with stopwords (none, English, and custom), preprocessing (none, Lemmatize, and Stem), Ngrams (1, 1:2, 1:3, 2:2), Max Depth (4, 7, 10), and  Bootstrap(True, False).  The best performance was achieved with no stopwords, no preprocessing, Ngram of 1:1, Max depth of 10, and without bootstrapping.  The accuracy score was 62.08% compared to a baseline of 57%. 

The SVC model was grid-searched with stopwords (none, English, and custom), preprocessing (none, Lemmatize, and Stem), Ngrams (1, 1:2, 1:3, 2:2), and C (25, 50, 100).  The best performance was achieved with Custom stopwords, Ngram of 1:2, Lemmatize, and C of 50.  The accuracy score was 68.42% compared to a baseline of 57%. 

| Model               | Best Parameters                                                                        | Accuracy (%) |
| ------------------- | -------------------------------------------------------------------------------------- | ------------ |
| Base                |                                                                                        | 57           |
| KNN                 | Stopwords: Custom, Preprocessing: Stemming, Ngram: 1:1, K-neighbors: 7                 | 56.69        |
| Logistic Regression | Stopwords: Custom+English, Preprocessing: Lemmatize, Ngram: 1:2, Ridge                 | 68.30        |
| Random Forest       | Stopwords: None, Preprocessing: None, Ngram: 1:1, Max Depth: 10, Bootstrapping: False, | 62.08        |
| SVC                 | Stopwords: Custom, Preprocessing: Lemmatize, Ngram: 1:2, C: 50,                        | 65.61        |
| Naive Bayes         | Stopwords: English, Preprocessing: None, Ngram: 1:1, Alpha: .5,                        | 70.77        |


## RECOMMENDATIONS 
I recommend that we start using the cross-validate NB model to find Tico/Expat posts on social media.  This model has the highest accuracy while maintaining a low false positive rate.  In other words, it allows us to find tourists in the sea of Expats.  

By using this model we will be able to start creating a network of Expats that can assist us in advertising for tourist businesses.  Additionally, this will also reveal, to a greater extent, who the tourists are.  This will allow us to direct more marketing toward our target audience.  I recommend that we increase our social marketing campaign to greater increase our impact.  

Additionally, Reedit is currently going through a period of adjustment that is making using an API difficult.  Once this period of adjustment is over, I recommend we pull more posts and use them to refine the model for greater accuracy.  