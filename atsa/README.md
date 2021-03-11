# 3/9/2021
Discussed the next block, block 2, and talked about the first case study.

- this case study looked at a network of bluetooth relays in an environment
  - in Pennsylvania
- occasionally these bluetooth relays can lose packets (data is lost in transit)
- there is "asymmetric interference" between relays
  - data can be received but not acknowledged by the receipient node
  - the relay will re-send the data 4 more times
     - relay gives up if failure after the 4th time
  - this can cause data duplication
- goal of the study is to analyze this data duplication
  - if we allow duplicates, the duplicated data can bias explanatory statistics by inflating them
- goal of our work is to get
  - a discrete time series in 15 minute intervals of the data (chunk the data from 7:00 - 7:15)
  - essentially, we are going to bin the data in these intervals into discrete 15 minute bins
  - repeat this then for 60 minute, 12 hour, and 24 hour intervals

# 3/11/2021
- adc0, adc1, adc2
  - raw voltage (mV)
  - adc3 to adc 6 are ignorable

- fields to look at
  - battery (voltage), water potential (adc0), soil moisture (adc1, adc2)

- challenges for the steps of analysis
  - transmission lags are of random length and some could be from multiple minutes following the initial transmission from a node
  - binning observations by time groupings are not uniform across bins because of random events like packet loss and transmission lag
  - the watershed utilized had small impact of human development and was largely in a protected water shed
    - the goal was then to understand how water distributes itself in nature
    - how much water is being distributed from soil into trees then back into the atmosphere?\
    - the natural area was appealing for this study
 

