# WeRateDogs Twitter Analysis
## by Oluwaseyi Afolayan


## Dataset

WeRateDogs is a Twitter account that posts and rates pictures of dogs. These ratings often are not serious and have numerators that are greater than the denominators.

This Wrangle Report is a part of a Data Analyst Nanodegree offered by Udacity. The project aims to gather data from Twitter API and Udacity provided tweet data, to create analysis about the tweets and the predicted dog’s breed.

Data Wrangling follows,

- Data Gathering
- Data Assessing
- Data Cleaning

### Data Gathering

I gathered data from 3 sources, stored in separate files:

1. WeRateDogs Twitter Enhanced archive, manually downloaded from the Udacity servers.
2. The image predictions file, programmatically downloaded from the Udacity servers.
3. The entire set of each tweets’ JSON data, downloaded by querying the Twitter API using the Tweepy
library. The favourite_count and retweet_count were extracted programmatically from this file.
I loaded the 3 raw data files into separate tables: archive, predictions and tweet_data.

### Assessment & Cleaning

I began the assessment by viewing the information on the `archive` table first, identifying several quality and
tidiness issues.

All rows containing non-null values in the retweeted_status_id , retweeted_status_user_id and
retweeted_status_timestamp , and also in the in_reply_to_status_id and in_reply_to_user_id
columns were dropped, as per the requirements. These columns were then also dropped.

The timestamp column was converted to datetime data type.

The 4 dog stage columns were melted into the stage column; tweets without stages were set to ‘none’.

Several had 2 stages set, so I kept only the one with the lower overall count.

The html strings in the source column were replaced with the display portion of itself.

The rating_numerator and rating_denominator columns were checked for value ranges; I decided to keep
only tweets with single ratings. Several tweets’ ratings were manually corrected with values from the text.
Tweets with large numerators were dropped, as the text didn’t contain a valid rating (# out of 10). After the
ratings were fixed, I dropped the rating_denominator column (it contained only ‘10’s) and renamed the
rating_numerator column to rating.

The odd words in the name column were replaced with ‘none’.

Tweets with missing values in expanded_urls , (not retweets or replies) were actually missing the urls from
the text itself. These tweets were dropped, and then the column itself.

The `predictions` table itself was not cleaned. There were many tweets with no dog breed predicted, these
were left as is. The best prediction for breed and associated confidence level were extracted and merged
into the archive table.

The `tweet_data` table itself was not cleaned. The retweet_count and favorite_count columns were merged
into the archive table, and the data type reset to int. One tweet was missing both counts so was dropped.

The remaining cleaned columns in the archive table were reordered, then the table was saved to the new
“twitter_archive_master.csv” file. The predictions and json_data tables had not been cleaned, so were not
saved.

## Summary of Findings

In my analysis, I examined WeRateDogs's archive through August 1, 2017. Most of the necessary Twitter data was provided by Udacity. In addition, Udacity ran the images on WeRateDogs's account through a neural network to generate three predictions for each image. After wrangling the data for original content with images, I wanted to answer the following questions:

- What are the Top 10 breed of dogs tweeted about?
- What dog breed got the highest retweet and favorite interactions?
- What is the correlation between ratings and the interactions by Twitter users?
- What is the weekly trend of interactions with WeRateDogs's posts?

## Key Insights for Presentation

1. Noticable top breeds being tweeted are, Golden retriver, Labrador Retriever, Pembroke, Chihuahua, Pug, Toy Poodle and Chow
2. Every other breeds gradually decreases in count
3. Noticable top 5 rated breeds are, Samoyed, Golden Retriever, Great Pyrenees, Pembroke, and Chow in order
4. Noticable top breed being retweeted and favored is, French Bulldog
5. Most rated & favorited stage of dog is puppo
6. Most retweeted stage of dog is doggo
7. Adding hashtags has a greater impact in retweet count & favorite count on average
8. Retweet and Favorite Count on average increases gradually from Monday - Wednesday
9. Sudden drop in Retweet and Favorite Count in Mid-Week (Thursday) on average
10. Gradually increases again before slight drop on Sunday
11. Rating very less likely to impact Retweet and Favorite Count
12. Retweets and Favorites are highly correlated

