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

## Group 1 Discussion
 - supporting functions file
    - load each file in the utility script
    - put them together then clean them up 
    - try to make them into one data frame
    - what about duplicates
      - all the data packets are the same but the timing is off by a slight amount?
    - function to identify indices of duplicates
    - looking at raw data from first file, we see third to fifth line has duplicates
      - duplicates may have the same time stamp...could be helpful to then remove them
      - identify if time stamps are roughly similar
      - make sure that all columns are the exact same
    - mainly just understanding the problem of duplicate values will be difficult
  - at 15 minute mark we do not need averages
    - at 60 minutes we just average everything in a 60 minute time period (probably 4 observations)
 - how can we make the time values continuous?
    - want to make time continuous rather than discrete
 - duplicates are inflating metrics
 - we need to find the mean of the data points at 15 minute blocks
    - concern with duplicate packets is that they will inflate values at different time intervals
    
 # 3/16/2021
 Continue group work. Answered some of the Case Study 1 approach questions in the discussion forum.
 
 
 # 3/18/2021
 NOTE arrived about 20 minutes late to class...need to update notes
 
 - the second case study is introduced
   - the problem with this case study is we need to fill in some values 
 - data
  - SW down
    - average of solar flux over a given day
  - H_0
    - daily extraterrestrial solar radiation
  - f_FEC
    - conversion of value from flux units to the amount of photon energy in shortwave solar radiation
    - value is constant
  - I_0
    - values composed of equation from this link: https://bitbucket.org/labprentice/gepisat/src/master/
    - solar noon is when the sun is directly above you
      - we have to correct this solar noon to conventional time (local clock time)
      - there is an equation of time, EOT
      - timezone? every 15 degrees east or west we move an hour in time
      - a majority of these equations are in the SPLASH code

 - SPLASH
    - we care about the $ra_j.m2 is the value we are looking for. This gives us H_0 for each day
    - declination angle, distance factor, heliocentric, sunset angle, etc. are all already calculated
    - issue is SPLASH goes straight into the integral of the daily energy
      - we do not have the half-hourly
      - hint hint: it is available in the GePiSaT code (available in Python not R)
      - the R version is available in the repository
      - will have to add the missing pieces from solar.py to solar.R
      - then, for each day, just calculate the whole half-hourly time series from 0:00 to 23:59
      - recreate the blue/red figure from section 2.2 in the notebook
      - red = gap-filled dataset, blue = original data



