# Project 5
For my project, I decided to curate a text-based data set pertaining to blockchain in healthcare. I did this by 

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
Overall, 207200 tweets by about 45000 unique accounts were retrieved from January 1, 2016 to January 31, 2021. We can see the number of tweets over time in the figure below.

![volume](https://raw.githubusercontent.com/caiettia/Thesis-Project/main/project5/volume_movingavg_month.png)

So, we understand that the volume of tweets over time appears to mimic a bell curve centered around the year 2018, with still a spike in mentions of Blockchain in Healthcare 
noted in mid 2017.
