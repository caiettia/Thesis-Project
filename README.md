# Project 5
For my project, I decided to curate a text-based data set pertaining to blockchain in healthcare. The reason for this, is I want to better understand how Blockchain as a technology is percevied in the Healthcare industry. As such, I will be taking my text-based data set and performing Sentiment Analysis on said data set. From this, I hope to have a better understanding of how the technology is perceived by a public forum such as Twitter in regards to its application and use in the Healthcare sector. 
## Background
To help understand our problem statement, we can define Blockchain. Blockchain, at a high level, is a special kind of data base. It is built on a peer-to-peer network based architecture, where many computer systems in a network all hold copies for the Blockchain. More granularly, it is composed of a series of blocks where each block stores data in the form of a cryptographic hash, and is connected to the previous block in the chain by noting the previous blocks respective hash. Thus, by noting the previous blocks hash, the blocks are chained together: hence the name.

# Data
## Curating
My data set was curated from Twitter. I did this because of the abundance of data available on the platform to ensure that I would be able to retrieve enough tweets for my purposes. So, I utilized the Twitter API through a developer account as well as a webscraper, snscrape, to retrieve the tweets. 

To successfully get tweets based on a keyword search for a given time period, I had to first get the tweet IDs. Getting tweets with this method is not possible
through the sole use of the Twitter API, as the Twitter API does not let you search for tweets based on parameters such as time or topic. The API does, however, allow you to 
search for historical tweets from any given time stamp by passing the Tweet's ID. This ID is a unique value assigned to each tweet as it is created. Once this ID is retrieved,
the Twitter API has the historical lookup feature mentioned previously that is able to then process 900 tweet IDs per 15 minutes. 

### Tweet IDs
So, we first need to get the tweet IDs. To do this, I utilize the snscrape library. This allows me to retrieve tweet IDs based on a keyword search while also specifying 
temporal bounds for tweets. The library can be run in a python environment, however I chose to utilize the command line to run queries for tweet IDs, which are then stored in 
basic text files. The code to do this can be seen below.

![code_ids](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/examplecode.PNG)

In this example, the code is written so that my query includes tweets starting from 2019-01-01, and ending on 2019-12-31. The temporal bounds, when passed to the library, 
are not inclusive of the bound itself and thus need to be plus or minus a day from the desired boundary date. Next, I specify the keyword strings that I would like to query 
Twitter for, "blockchain healthcare". This will then only retrieve tweets relating to the topic of Blockchain in Healthcare. Finally, before executing my query, I am able
to simply specify where I would like the text file of tweet IDs to be stored. Once all of these parameters are specified, the OS library in Python is utilized to
programmatically execute snscrape commands on the command line interface. So, to retrieve tweet IDs, I specify temporal bounds from January 2016 to January 2021. This
then gives me all of the tweets about Blockchain in Healthcare that have been tweeted within that timeline. 

### Twitter API
Once I have been able to retrieve all of the tweet IDs, I can then utilize the Twittier API and its historical lookup function to retrieve more data about each tweet. This is 
done by utilizing the python library tweepy where I am able to specify my API keys first, then iteratively read tweet IDs from the above scraping method and pass them to the 
Twitter API. This can be seen in the code snippet below.

![apicode](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/apiexample.PNG)

Here, I pass my Twitter API access keys to the authenticator to initialize my tweepy instance. Then, I read the tweet IDs I retrieved from the previous step and pass them 
iteratively to the Twitter API. Each ID then returns a response object to which I take the fields seen in the for loop and store them in an overall data frame. 

### Pre-Processing 
When working with the tweet data I have retrieved, I need to clean up the text of each tweet before I can effectively apply Natural Language Processing methods to it. As 
such, I utilize the regular expressions library, seen as "re". The code snippet for this can be seen below.

![apicode](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/preprocess.PNG)

I take all of the text from each tweet and pass it through the clean_text function which applies various
regex expressions to remove text from the tweets. The text cleaning process overall removes: hyperlinks from each tweet, the @ symbol used when referencing different users, 
removes miscellaneous text artifacts the Twitter API would include in text objects, then finally drop any excessive spacing between words in a tweet. Thus, we are able to 
finally have all of the text from each tweet cleaned up and ready for processing by our model.

## Preliminary Observations
Overall, 207200 tweets by about 45000 unique accounts were retrieved from January 1, 2016 to January 31, 2021. We can see the number of tweets over time with a 12-month 
moving average imposed in the figure below.

![volume](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/volume_movingavg_month.png)

So, we understand that the volume of tweets over time appears to mimic a bell curve centered around the year 2018, with still a spike in mentions of Blockchain in Healthcare 
noted in mid 2017. To better understand what is happening here, we can further explore the tweets through sentiment analysis.


# Sentiment Analysis
To conduct this sentiment analysis task, we first need to treat this as a multi-class task with positive, neutral, and negative classes. This is so we may include and 
consider the polar classes rather than only considering one class in a binary instance (i.e positive vs. not positive). 

## Training Data 
Thus, we have a manually labeled training data set of tweets with 2586 positive tweets, 2586 neutral tweets, and 2398 negative tweets. Given that our sentiment analysis task 
is a multi-class classification task, we must then understand how we can arrange our training data set to effectively train a classifier that is not necessarily able to 
handle beyond the binary class task. To do this, we arrange our data into a One Versus One (OVO) set up. This means that, given our three class task, we train three separate 
models where each model only considers two classes in the data set. Each of the three models are trained on these two class data sets, then consolidated in an ensemble logic 
where a final class label is assigned based on the outputs of the various models. In terms of how the training data is then set up, we can see the combinations of classes fed 
to each binary model in the table below.

| Classes               | Respective Rows |
|-----------------------|-----------------|
| Positive vs. Neutral  | 2586 vs. 2586   |
| Positive vs. Negative | 2586 vs. 2398   |
| Neutral vs. Negative  | 2586 vs. 2398   |

We can see that, while the class balance is not perfect when considering the Negative class, a difference of 188 rows in the data set is close enough class balance to warrant
not having to worry too much about synthetic class generation.

## Model Selection
Since this is a text-focused task, we can utilize a newer model in the textual analysis arsenal: Bidirectional Encoder Representations from Transformers (BERT). This model 
comes from a 2018 publication by Google, and is a neural network based model that is pre-trained on 2,500 million words from Wikipedia and 800 million words from the 
BooksCorpus, a corpus of 7,000 literary works. BERT is first trained on these two data sets to learn the context of words and how they are used through two methods: Next 
Sentence Prediction (NSP) and Masked Language Modeling (MLM). Briefly, Next Sentence Prediction is a task BERT utilizes to understand if, when given two setences, BERT learns 
to predict if the second sentence provided follows the first from the document or not. Masked Language Modeling, on the other hand, is when BERT learns to predict what a word is in a given sentence, by first masking said word then attempting to predict it by referencing the remaining words in the sentence. Thus, BERT begins to develop an understanding of how words are used in a sentence and what those words might be. Both NSP and MLM are done simultaneously in the pre-training phase, and BERT develops a model from this of 110 million parameters in the BERT-base model, and 340 million parameters in the BERT-large model. For sake of computational efficiency while maintaining performance, BERT-base will be utilized.

Using BERT in this instance is particularly advantageous for this task, as BERT is pre-trained on these massive data sets. Thus, we need only to fine-tune our instance of a 
BERT model to then make sentiment classifications for our task. Taking advantage of this knowledge transfer is what has made BERT such a strong contender for one of the most
advanced Natural Language Processing models available. Also, as a fun fact, as of October of 2020 BERT models handle a majority of the english language search queries on 
Google.com! So, it is highly likely you have already encountered and used BERT without even realizing. 

In terms of implementation, the Transformers library is utilized. This allows us to use not only the BERT-base model as our classifier but also the BERTTokenizer, which 
tokenizes data to be then interpretted by the BERT model. So, in terms of feature extraction, BERT handles this by learning word-embeddings through the pre-training phase.
The fine-tuning of the model really only alters various parameters previously generated from pre-training, to then better fit the model to the training data provided. In 
terms of hyperparameters, all recommendations are followed from the original 2018 publication of the model.

## Model Evaluation
We first split out data set to create a test set comprised of 20% of the data. Then, with the remaining 80% of our dataset, we utilize K-Fold Cross Validation where K = 10. 
We can see the row-breakdown of this in the image below.

![split_dat](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/train_test_split_fig.png)

Thus, BERT is trained and validated with 80% of the data, while then performance metrics are generated from the 1514 rows in our test set. With this, we observe a performance 
of BERT of 94.9% overall accuracy in a One Versus One set up. We also observe, F-scores for the Positive, Neutral, and Negative classes of 94.24%, 94.95%, and 95.57% 
respectively. Seeing this admirable performance and balanced range of F-scores, we can explore the sentiment class distribution of the data set. 

# Sentiment Exploration
Overall, we can first look at the sentiment class distribution of tweets.

![rawperc](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/perc_comp_tweets.png)

Here we can see that nearly 50% of our data set is positive, while only about 8.5% of our data set is negative. This tells us that, when tweeting about Blockchain in Healthcare, byenlarge Twitter users speak positively on the topic. To further consider this temporally, we can look at the raw class composition aggregated by month.

![overtime](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/raw_perc_over_time.png)

When looking at this, we see that following 2017 the positive tweets are observed to be the majority class of all tweets for nearly every month in 2018 to 2021. Thus, even as the volume of tweets decreases following 2018, the optimism of the tweets that are produced is notably still visible in the years following. To understand the rate of increase of positive tweets, we can briefly fit a linear line of best-fit to each class and observe a more quantifiable story in the data.

![lines](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/perc_over_time__lines.png)

![formulae](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/bestfitformulae.png)

From this, we see that the percent composition of the positive class overtime is increasing at a much greater rate than the negative class. Yet it is still worth noting that 
we observe the negative class still increasing over time, albeit at a much slower rate relatively. So, people are much more increasingly positively talking about blockchain 
in healthcare relative to their skepticism about the technology in the healthcare sector. So overall, we observe that people are talking very positively about the idea of blockchain in healthcare, with the general trend of positivity persisting into 2021!







