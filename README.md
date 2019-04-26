# Subreddit Classification: r/CasualConversation vs. r/SeriousConversation

### Problem Statement

There is a subreddit for everything. Given a post from two different subreddits, can the subreddit to which the post belongs to be identified? I am specifically wondering how well can posts about serious conversation topics (r/SeriousConversation) be identified when compared to posts about casual conversation topics (r/CasualConversations)? To solve this problem I will analysis the different language used between the two posts and find a model that categorizes posts with the highest accuracy. While determining the feasibility of this classification I will determine what the unique identifies are and what the highest correlated words are to the individual threads.

### Data Collection

The first step toward answering this problem was collecting the data. This required me to use the Reddit API to collect information such as title text, body text, author name from each individual posts. After cleaning this resulted in 797 observations of r/CasualConversation and 897 observations of r/SeriousConversation. In addition to this information, I also collected up to 30 comments per post from the first and second reply level, when available. I collected 17,476 comments from both r/CasualConversation and r/SeriousConversation, an average of 10 comments per post.

### Executive Summary
With the data now collected, it was time to clean it. After removing rows I felt would not add to my model - such as posts made by the moderator - and filling nulls with empty strings I began to clean up the text. The cleaning process involved removing punctuation and special characters, converting all letters to lowercase, removing trailing and leading whitespace and lemmatization. This process was applied to all text columns, including a newly created text column that was a string containing the text from the title, body, and comments.

To determine whether or not I would be able to differentiate between posts from these two subreddits I had to begin exploring and analyzing the data. I began by visualizing the character count, word count, and average word length as well as the correlation between these counts and the target (serious). I found that serious posts tend to have longer character counts (and word counts). As a result of this trend, character count and word count are both positively correlated with the target.

Analyzing the text for positive and negative sentiments I discovered that serious posts tend to be negative and casual posts tend to be positive. This did not come as a surprise. The correlation between negative sentiment and the target is 0.37. After the text containing all parts of the post, the body text has the strongest sentiment correlation to the target.

When looking at the correlation between common words it was no surprise that words such as "don't know". "feel like", "people think", and "don't think" had a positive correlation as I would expect those words to appear together. In regards to words with high correlation to the target, the word list differed by the text area (title, body, comments), but some of the top words include:
- Casual: today, favorite, excited, song, haha, congrats
- Serious: feel, people, think, death, depressed

The language from the casual posts is very light-hearted, while the serious posts clearly discuss very deep and heavy life topics.

In order to classify these posts I used both a Naive Bayes and a Logistic Regression Model, with both models performing very similarly (accuraccy around 85%). To determine the models I would use I looked at a variety of classification models and their accuracy, sensitiviyy and specificity metrics. I also compared scores between text that had been cross vectorized and TF-IDF text. I then picked the highest performing models and used a grid search to fine tune the models to produce the most accurate results. Lastly, for the Logistic Regression model, I used SVD which really helped me to remove some of the high variances my model was seeing (train and test scores were quite different)

In conclusion, the model I developed can classify a given post between r/CasualConversationa and r/SeriousConversation with 85% accuracy. Both sensitivity and specificity are around 80% as well. The main identifiers between the two threads are text length, sentiment and word choice. One applitcation of this model is for it to used to flag serious posts for additional review to determine if the author mentions potential harmful behavior toward themselves or others.

## Data Dictionary

|Feature|Type|Description|
|---|---|---|
|title|string|title of the post|
|selftext|string|body of the post|
|first_com|string|comment directly on the post itself|
|second_com|string|first reply to any comment directly on the post itself|
|subreddit|string|either SeriousConversation or CasualConversation|
|is_serious|int|target: 1 = serious, 0 = casual|
|num_comments|int|number of total comments on a given post|
|permalink|string|link attachment that redirects to the comments page|
