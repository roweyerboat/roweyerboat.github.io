---
layout: post
title:      "When Is Sentiment Analysis Not Enough?"
date:       2020-11-14 02:32:05 +0000
permalink:  when_is_sentiment_analysis_not_enough
---

![Image of faces ranging from happy to angry](https://i.imgur.com/k77ULpR.jpg)

### What is Sentiment Analysis

Sentiment analysis is the process of mining data for the sentiment or emotion of a text. It combines both machine learning and natural language processing tools to score the words as either positive or negative. It has become very popular, that even now with the Google Chrome Add On from [Grammerly](https://chrome.google.com/webstore/detail/grammarly-for-chrome/kbfnbcaeplbcioakkpcpgfkobkghlhen), the words I am using are being analyzed for sentiment and I'm given a different emoticon for the sentiment of my text. 

The basics for sentiment analysis are rather simple, words that are known for their polarity are given a score and then a sentence's scores are added up to give an overall polarity score. An example scoring list is [AFINN](https://gist.githubusercontent.com/damianesteban/06e8be3225f641100126/raw/a51c27d4e9cc242f829d895e23b4435021ab55e5/afinn-111.txt)

For example, if we had the phrase, "I love hiking but I'm afraid of heights."

In the AFINN list, "love" would be scored with a +3, while afraid is a -2. The overall score for the phrase would be +1 or mildly positive. 

![image of sentiment scale](https://i.imgur.com/5NRn9N6.png)

This is just a very basic demonstration. When using algorithms such as [Textblob](https://textblob.readthedocs.io/en/dev/) or [VADER](https://github.com/cjhutto/vaderSentiment), the scoring is more complex and accurate as the systems continue to learn as they score text.


Sentiment analysis has been adopted across a variety of industries. It is widely used to monitor campaigns, brands, stock markets, compliance, and customer feedback. Once implemented, you can tag products that are getting negative reviews or measure how campaign slogans are landing. 

When working with unstructured (or unlabeled) data, sentiment analysis can be helpful to be able to draw connections across text that is being used. 


### Limitations to Sentiment

![twitter image](https://i.imgur.com/U6NKuJH.png)

In my project to analyze Twitter data, I thought that Sentiment Analysis was a perfect fit. I was taking hundreds of thousands of tweets that used the same hashtag to see if I could find out how people felt toward the Black Lives Matter movement. I used both VADER and Textblob to label the sentiment of all the tweets. 

I first looked at the tweets provided by VADER as high polarity or "positive" sentiment:

![tweets with highest polarity VADER](https://i.imgur.com/8aIqPli.png?1)

<br />

Then I looked at tweets provided by Textblob again with high polarity or "positive" sentiment:

![tweets with highest polarity textblob](https://i.imgur.com/94B3UGN.png?2)

<br />

Just to continue checking I looked at the lowest polarity or "negative sentiment from both:

![tweets with lowest polarity vader](https://i.imgur.com/WHGy9CU.png?1)

![tweets with lowest polarity textblob](https://i.imgur.com/FKiK0D6.png?2)

<br />

It become very obvious very quickly, when discussing matters of death, brutality, violence, there is going to be a negative tone to the text used. So, I wasn't able to distinguish if people were supportive or against the Black Lives Matter movement based solely on sentiment. It's important to consider the context of what you're analyzing. 

Looking at the spread of the labeling from both VADER and Textblob also showcases how reading these tweets for sentiment was difficult, as many tweets were labeled as neutral.

![graph of sentiment label](https://i.imgur.com/kxJaxnI.png)


### Alternative Analysis

Aside from just looking at sentiment, there are many other tools that can be used to analyze text. 

* Intent Analysis - looks at the intention behind the text and identifying if it is an opinion, news, marketing, complaint, suggestion, appreciation or query. <br />

* Contextual Semantic Search - takes thousands of messages and a concept as input and filters all the messages that match closes with the given concept

As these new applications grow, companies are able to better implement adjustments by using the power of AI, Deep Learning, and intelligent classifirs. The more accurate the data, the better conclusions you'll have.

