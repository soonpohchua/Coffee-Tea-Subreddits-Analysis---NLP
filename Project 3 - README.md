<img src="http://imgur.com/1ZcRyrc.png" style="float: left; margin: 20px; height: 55px">

# Project 3: Coffee & Tea Subreddits Analysis - NLP
<br>

## 1. Problem Statement

This project is meant to provide market research for owners planning to start their new cafe. The strategy is to analyze data from the 'Coffee' and 'Tea' subreddits so as to influence the business decisions of the cafe set-up.

In summary, the project shall :-

1. extract data from the 'Coffee' and 'Tea' subreddits;
2. perform data cleaning on the data;
3. identify useful and popular topics related to coffee and tea; 
4. produce a classification model to classify texts according to either coffee or tea topic; and
5. provide sentiment analysis on each subreddit in general and specific topics of interest under each subreddit.

## 2. Background

Based on [market research](https://backlinko.com/reddit-users), Reddit is a fast growing community platform categorized around different interests. With more than 430 million monthly active users and over 100,000 active communities (as of Oct 21), it is ranked among the most popular social networks worldwide. With its growing online presence, this project shall leverage on Reddit's potential to conduct market research.

The market research will mainly help the cafe in two ways:

1. Identifying useful and popular topics related to coffee and tea that can provide business ideas such as choice of products, presentation of products, coffee/tea-making equipments etc. The usefulness of these topics will be further validated using sentiment analysis.

2. Providing a useful classification model to identify posts related to coffee and tea. This can enhance the feedback channel for customers providing feedback to the cafe knowing which kind of products (coffee or tea) that they are commenting on. The classification model can be further enchanced to include other range of products offered such as cake etc. Additionally, the classification model can also be utilised when the cafe tap on other channels like facebook, instalgram to gather more customer feedbacks for analysis.

## 3. Dataset

15000 posts were scraped from the Coffee and Tea subreddits respectively using [Pushshift Reddit API](https://github.com/pushshift/api). The timeframe is before 1 Oct 2022, UTC.

Below is the overview of each dataset before Exploratory Data Analysis (EDA).

| Column | Description |
|:----|:----|
| subreddit | Indicated 'tea' or 'coffee' depending on the dataset |
| author | Usernames that posted in the respective subreddit |
| date_posted | Date of posts |
| text | Uncleaned text in the original form for sentiment analysis |
| text_cleaned | Cleaned text for EDA and classification model |

## 4. Notebooks 

There are 4 notebooks in total. 

| Notebook | Description |
|:----|:----|
| Notebook 1 - Exploratory Data Analysis | Covering Data Cleaning & EDA |
| Notebook 2 - Classification Modeling | Covering Base & Hyperparameter Tuning Models |
| Notebook 3 - Sentiment Analysis | Covering general analysis and popular topics of both subreddits |
| Side Notebook - Data Scraping | Covering Data Extraction using Pushshift Reddit API |

The summary of Notebook 1-3 is listed below.

### 4.1 Summary of Notebook 1 - Exploratory Data Analysis

**1. Data Cleaning Process**. The imported datasets began with 14979 rows and 82 columns, and 14971 rows and 85 columns for Coffee and Tea respectively. The following adjustments were made to each dataset as part of data cleaning :-
   - Removed all the irrevelant columns;
   - Removed posts that were removed from each subreddit; 
   - Removed recurring and duplicated posts; 
   - Created a new column 'text' by combining the title and body of posts;
   - Created and edited a new column 'text_cleaned' for EDA purpose and further analysis under notebook 2; 
   - Result: Coffee dataset is left with 13457 rows and 5 columns, while tea data has 11950 rows and 5 columns.  
   
**2. Key Points from EDA.**
   - Post from users: More than 70% users of each subreddit only posted one post. Coffee subreddit has more unique users but less posts per users as compared to tea.
   - Date of posts: Coffee subreddit receives more posts on a daily basis. Despite scraping the same amount of data (15000 posts), the earliest post of Tea subreddit was back in Aug 21 which is four months before the earliest post of Coffee subreddit. 
   - Unigram/ bigram/ trigram analysis:
        - Promising topics can be classified into types of coffee/ tea, types of coffee/ tea making technique and specific brands for coffe/tea products
        - Promising topics from Coffee dataset (21 in total): Espresso, Speciality Coffee, Filter Coffee, Dark Roast, Baratza Encore, Burr Grinder, Drip Coffee, Ground Coffee, Moka Pot, French Press, Cold Brew, Timemore Chestnut C2, Breville Barista Pro, Nitro Cold Brew, Fellow Stagg Ekg, 1Zpresso Jx Pro, Aeropress French Press, Breville Smart Grinder, Breville Precision Brewer, Gaggia Classic Pro, Breville Barista Express
        - Promising topics from Tea dataset (16 in total): Matcha, English Breakfast, Cold Brew, Harney Sons, Oolong Tea, Green Tea, Black Tea, Gong Fu, White Tea, Earl Grey, Da Hong Pao, Tie Guan Yin, Jin Jun Mei, Bi Luo Chun, Jasmine Green Tea, Japanese Green Tea
        - These topics will undergo sentiment analysis under notebook 3. 

### 4.2 Summary of Notebook 2 - Classification Modeling

**1. Baseline Models**. 
   - Naives Bayes model was used as a base line model achieving a train score of 0.962 and test score of 0.931, which refers to the accurancy in prediction.
   - In order to make the classification task more realistic, obvious words are removed based on the top 30 best performing words in predicting coffee and tea posts respectively.
   - A further three models (K-Nearest Neighbor, Random Forest & Logistic Regression) were introduced as base models. As a reference point, the training and test scores of the Naives Bayes model decreased to 0.912 and 0.855 respectively.
   - K-Nearest Neighbor is the worse performing model, while the remaining models are comparable in their test accuracy score. 
   - All models have evidence of overfitting with Random Forest being the most obvious with a near perfect training accuracy score.  
   
**2. HyperParameter Models.**
   - Most models, apart from Random Forest, showed improvement on test accuracy score after hyperparamter tuning.
   - All models were able to minimise the impact of overfitting by achieving a training accuracy score that is closer to the test accuracy score.
   - K-Nearest Neighbor is still the worse performing model, while Logistic Regression is the best performing model (with the results indicated above).
   - Logistic Regression is able to acheive the best test accurancy score with the training accuracy differing by just 0.02.
   - Logistic Regression is able to do a comparable job in classifying Coffee and Tea posts correctly with a score of 0.871 (recall score) and 0.872 (specificity score) respectively for each category.

### 4.3 Summary of Notebook 3 - Sentiment Analysis

**1. General Sentiment/ Emotion Analysis**
   - Textblob Sentiment Analysis:
       - Posts under each subreddit fare well on positivity more than 0.8 out of 1
       - Posts under each subreddit leans towards being objective than subjective
   - Pysentimiento Sentiment Analysis:
       - Most posts belong to the neutral category and score higher in being neutral
       - Posts from tea subreddit scored better in being positive and scored worse in being negative as compared to posts from coffee subreddit.
   - Pysentimiento Emotion Analysis:
       - Most posts from both subreddit belong to the Others category, which is devoid of the other emotions indicated (much like being neutral).
       - Posts from both subreddits fare low in emotions such as fear, surprise, disgust, sadness and anger.
       - Posts from tea subreddit fare better in terms of joy.

**2. Analysis on Popular Topics**
   - Promising topics were listed back in notebook 1. There are 21 topics and 16 topics for Coffee and Tea subreddits respectively.  
   - Of interest are indicators which are positive (under sentiment) and joy (under emotion).
   - Topic would need to meet the requirement of scoring higher in either positivity or joy as compared to the other posts without the topic.
   - Of the 21 promising topics from the Coffee subreddit, only 9 passed the criteria of having higher mean score in either positivity or joy. All of the 9 topics were only able to fulfil a higher positivity score than posts without the topic. The topics are: nitro cold brew, espresso, filter coffee, dark roast, burr grinder, timemore chestnut c2, breville barista pro, aeropress french press and gaggia classic pro.
   - Of the 16 promising topics from the Tea subreddit, 11 passed the criteria. All were able to fulfil a higher positivity score than posts without the topic. The topics 'White Tea' and 'Da Hong Pao' performed the best as they were also able to fulfill having a higher mean score on joy than the other posts.
   - Taken together, they can potentially influence the cafe in the following areas:
        - Choice of Beverages: espresso, jin jun mei, da hong pao, green tea, earl grey, japanese green tea, white tea, black tea, oolong tea, english breakfast
        - Method of Coffee/Tea-Making: nitro cold brew, filter coffee, dark roast, aeropress, french press, cold brew tea
        - Products related to Coffee/Tea: burr grinder, timemore chestnut c2, breville barista pro, gaggia classic pro, harney and sons
        
## 5. Conclusion

### 5.1 Possible Areas for Improvement

What the project has established thus far is solely based data scraped from the coffee and tea subreddit threads for classification of coffee/tea topics and to generate useful and helpful topics based on sentiment/ emotion analysis. The following can be considered for areas of improvement:-

1. **Increase the amount of data**
     - Increase the number of posts scrapped from each subreddit (current project only scrapped 15000 posts)
     - Expand the scope of datascrapping to other subreddits with relevant topics such as coffee-making machine or tea brewing etc.
     - Explore other similar platforms like twitter or facebook to scrape additional data
     
2. **Include related topics in a cafe set-up setting**
    - In addition to point 1, there could be other topics such as cake in a cafe set-up setting apart from coffee and tea

3. **Further testing on sentiment analysis**
    - We can consider using another model as comparison
    - Ideally, result of sentiment analysis can be more accurate by using human annotation
    
### 5.2 Limitation

The project has the following limitations:-
1. Data is only extracted from the Reddit platform, which lacks diversity;
2. Based on [market research](https://backlinko.com/reddit-users), close to 50% of the Reddit users are based in United States, which cannot be generalised for the global market; and 
3. Despite picking out the most common words using unigram, bigramm and trigram analysis, the posts containing noteworthy topics are little as compared to the those posts without. Nevertheless, the project tried to sidestep the issue by using mean score. 
4. The project did not demojize emoticons prior to sentiment analysis. The mistake was identified upon presentation. I will rectify this post-project submission. 

Nevertheless, despite the limitations, the project does serve as a helpful baseline for further exploration as recommended under possible areas for improvement.

### 5.3 Summary

In summary, this project wants to accomplish two things:-

1. Produce a classification model to classify texts according to either coffee or tea topic; and
2. Provide sentiment analysis on each subreddit in general and specific topics of interest under each subreddit.

After hyperparameter tuning, the best model was found to be logistic regression with training and test scores of 0.869 and 0.871 respectively. Moving forward, the hope is to utilise the model for further classfication tasks when gathering feedback and further analysis across other platforms. Nevertheless, a diversity of data from different sources would be necessarily to improve on the model for various context. 

As for sentiment analysis, Pysentimiento model was heavily utilised. The key findings were the noteworthy topics which perform better than the posts without them in the area of postivity and joy. Combining the findings across both datasets, the following topics are worth further exploration for cafe set-up business decisions:-
   - Choice of Beverages: espresso, jin jun mei, da hong pao, green tea, earl grey, japanese green tea, white tea, black tea, oolong tea, english breakfast
   - Method of Coffee/Tea-Making: nitro cold brew, filter coffee, dark roast, aeropress, french press, cold brew tea
   - Products related to Coffee/Tea: burr grinder, timemore chestnut c2, breville barista pro, gaggia classic pro, harney and sons