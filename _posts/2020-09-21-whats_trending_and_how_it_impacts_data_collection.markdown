---
layout: post
title:      "What's Trending and How It Impacts Data Collection"
date:       2020-09-21 17:48:04 +0000
permalink:  whats_trending_and_how_it_impacts_data_collection
---


Setting out on my journey with Sentiment Analysis, I decided to take an indepth look at twitter data. Specifically, I wanted to look at tweets with the hashtag BLM standing for Black Lives Matter. Knowing that people on all sides of opinions utilize this hashtag, I want to see if I can look at what words are commonly used and the sentiment that can be detected. 

![](https://i.imgur.com/m291hTvb.jpg?1)

I utilized twint to be able to download historical tweets and at first I tried just downloading every tweet with the hashtag that ever existed. I soon realized that this was beyond the limitations that twint could scrape as the kernel would die after scraping thousands of tweets and not be able to fully process the tweets.

```
import twint
import nest_asyncio
nest_asyncio.apply()

# Initializing the twint scraper
c = twin.Config()

# The keyword twint will scrape
c.Search = "#BLM"

# Ensuring just English tweets returned
c.Lang = "en"

# The format the tweets will be returned 
c.Format = "Username: {username} | Tweet: {tweet}" 

# Ensuring the output is compatible with pandas 
c.Pandas = True

twint.run.Search(c)
```

So I began limiting my scraping searches based on the year, starting with when Black Lives Matter was founded in 2013.

This worked well as I journeyed from 2013 to 2019. Each year I was able to gather all of the tweets without needing to put other parameters to limit the scrape so it wouldn't time out.

```
c.Search = "#BLM"
c.Lang = "en"
c.Format = "Username: {username} | Tweet: {tweet}
c.Since = "2013-01-01 00:00:00"
c.Until = "2013-12-31 11:59:59"
c.Pandas = True

twint.run.Search(c)
```

Then I began to scrape 2020. I tried several attempts to capture tweets from January to September but found my kernel kept dying before processing it all and I was unable to save the tweets scraped. I thought this made sense because the movement had caught on nationally and so the hashtag was used more so I limited my scraping to go every two months. 

![](https://i.imgur.com/0ekj6tt.png)

January and February worked well and March to April also didn't have a problem. So I thought - great! I've figured this out! Then I tried May to June. I tried it again and again and the same result. Then I tried May by itself. Same problem. Finally I split May up to May 1st through May 15th and then May 16th through May 31st and I was able to get it!

Learning from this and realizing that BLM was definitely trending during this time period, I thought no problem, I'll split up June the same way 1st through 15th. It didn't work. I tried it again, same result - dead kernel. So I adjusted it to be the 1st through the 10th. Same result. Then 1st through the 8th. Dead kernel. 

Feeling frustrated that it took me just one day to process the tweets from the years 2013-2019 and the year 2020 was taking up multiple days to be able to download, I vented to my partner. She said, "I wonder if it has something to do with Blackout Tuesday, when everyone posted the black box." Sure enough, I checked the date of Blackout Tuesday and it was June 2nd. Even though this was a trend on Instagram, it significantly increased the amount of twitter hashtags of BLM.

![](https://i.imgur.com/F6qF3gHb.jpg)

The reason I began this process in the first place was because of the national attention. Now I could see just how Blackout Tuesday literally clogged the BLM hashtag. To be able to scrape, I ended up having to go day by day in June up through June 21st. 

While I continue to analyze tweets with this hashtag, I am considering the sheer volume of tweets because of the national attention that the Black Lives Matter movement and how that impacts the types of tweets. 

Stay tuned for my blog that will be an overview of my sentiment analysis over time of the hashtag BLM.
