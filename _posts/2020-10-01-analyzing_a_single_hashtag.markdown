---
layout: post
title:      "Analyzing a Single Hashtag"
date:       2020-10-01 14:09:28 -0400
permalink:  analyzing_a_single_hashtag
---


![Wordcloud made from the words in tweets with the #BLM](https://i.imgur.com/0L4cNJw.png)

### Introduction

The Black Lives Matter (BLM) movement began in 2013 after the acquittal of George Zimmerman in the murder of Trayvon Martin [(source)](https://en.wikipedia.org/wiki/Black_Lives_Matter). Over the years, the Black Lives Matter (BLM) movement has gained attention across many platforms. One such platform, Twitter, has been a major place where the message of BLM has been articulated. With the events in May and June of 2020, the deaths of Ahmaud Arbery, Breonna Taylor, and George Floyd, BLM has been in the spotlight more than ever before. With that, there has been a variety of messages about what BLM means and what does it really accomplish. There are some who see it as a new wave of the Civil Rights movement from the 1960s. There are others that consider BLM as a dangerous terrorist organization. <br />

In order to sort out the message of BLM, I scraped twitter data from the past 7 years by pulling tweets using twint and searching the hashtag BLM. I recognize and acknowledge that those that use the hashtag might not be for Black Lives Matter and could even be a critic of it. I felt it was important to gather all that I could to see what rose to the surface when analyzing the text. I also recognize that people have the ability to delete tweets, and so I do not have a full collection of every tweet. <br />

With those acknowledgements, I was able to obtain 220,504 tweets from over 140,000 Twitter users. For more information about my scraping process you can read my [blog](https://roweyerboat.github.io/whats_trending_and_how_it_impacts_data_collection) about scraping with twint on a trending topic. <br />

### Table of Contents
1. [Cleaning the Data](#cleaningthedata) <br />
2. [Sentiment Analysis](#sentimentanalysis) <br />
3. [Word Analysis](#wordanalysis) <br />
4. [Topic Modeling](#topicmodeling) <br />
5. [Final Thoughts](#finalthoughts) <br />


### Cleaning the Data
<a id='cleaningthedata'></a>
After gathering all of that data, I began the process of preprocessing it, utilizing the Regular Expressions  Operations library (re) to remove unwanted urls and punctuation and then to tokenize the words.

```
def remove_picture_url(text):
    '''The input value is any text and the output removes the specific url used when a picture is added to a tweet.'''
    text = re.sub(r"pic.twitter.com\S+", '', text)
    return text
def remove_url(text):
    '''The input is any text and the output removes any url beginning with http.'''
    text = re.sub(r"http\S+","", text)
    return text
def remove_punc(text):
    '''The input value is any text or column in a dataframe. The output is the text with punctuation and numbers removed.'''
    text = "".join([char for char in text if char not in string.punctuation])
    text = re.sub('[0-9]+', ' ', text)
    return text
def tokenization(text):
    '''The input is text and the output is the text tokenized or separating the text into a list of words.'''
    text = re.split('\W+', text)
    return text

```

Then I used the Natural Language Toolkit (nltk) to remove English stopwords and lemmatized using WordNetLemmatizer.
```
def remove_stopwords(text):
    '''Input is text and the output is that text with stopwords. Stopword list must first be defined.'''
    text = [word for word in text if word not in stopword]
    return text

wn = nltk.WordNetLemmatizer()

def lemmatizer(text):
    '''Input is text and the output is the lemmatized text where similar words are grouped together.'''
    text = [wn.lemmatize(word) for word in text]
    return text
```

 ### Sentiment Analysis
 <a id='sentimentanalysis'></a>
 
 ![images of emoticons](https://www.talkwalker.com/uploads/2017/dan/Neg%20Sent%20Emoji%20cloud.png)
 
To get a better understanding of the words used, I decided to do a sentiment analysis. Since this data is directly from Twitter it is unlabeled. I decided to utilize two different sentiment analyzers and see how they compared. I implemented VADER (Valence Aware Dictionary and sEntiment Reasoner) as well as TextBlob sentiment analysis.

```
# VaderSentiment Analyser Initialization
analyser = SentimentIntensityAnalyzer()

def analyze_text(input_text, analyzer):
    '''
    This takes two inputs, the input text and the analyzer you want to use. The output is the sentiment score based on the analyzer used.'''
    if analyzer == 'VADER':
        result = analyser.polarity_scores(input_text)
        score = result['compound']
    elif analyzer == 'TextBlob':
        score = TextBlob(input_text).sentiment.polarity
    return score
```

I took a look at some of the results to see how well these were working to capture the sentiment of the tweets.


![3 tweets with highest polarity from VADER](https://i.imgur.com/8aIqPli.png?1)
![3 tweets with highest polarity from Textblob](https://i.imgur.com/94B3UGN.png?2)
![3 tweets with the lowest polarity from VADER](https://i.imgur.com/WHGy9CU.png?1)
![3 tweets with the lowest polarity from Textblob](https://i.imgur.com/hbRpcTa.png?1)


While this is interesting to see how well the sentiment analysis ran, it didn't actually tell me more about either how people felt towards #BLM. A negative sentiment wasn't necessarily negative towards BLM and likewise a positive sentiment didn't clarify if this person was promoting the message and values of BLM. 

Looking at the spread of the sentiment, there was a very even spread of both positive and negative sentiment. <br />
![image of the spread of both sentiment analysis](https://i.imgur.com/kxJaxnI.png) <br />

Therefore I found the sentiment analysis to be inconclusive.

### Word Analysis
<a id='wordanalysis'></a>

Moving from sentiment, I knew I needed to do more with looking at the words and phrases that are used most.

I utilized CountVectorizer and Bag of Words to collect the common words.

```
def get_top_n_words(corpus, n=None):
    '''This function takes in two inputs, the corpus or the text and n. It runs through the corpus and counts the frequency and reports back the top n words used in the corpus.'''
    vec = CountVectorizer().fit(corpus)
    bag_of_words = vec.transform(corpus)
    sum_words = bag_of_words.sum(axis=0)
    words_freq = [(word, sum_words[0, idx]) for word, idx in vec.vocabulary_.items()]
    words_freq = sorted(words_freq, key = lambda x: x[1], reverse=True)
    return words_freq[:n]
```

I ran this on the lemmatized version of the tweets and was able to pull the top 50 words used.

![Graph showing top 50 words used in tweets](https://i.imgur.com/YzNjTQt.png)

It isn't surprising as the BLM movement is about black people and so seeing the top 10 words makes a lot of sense. However it is also interesting to note how far down antifa is on the list. It is the 49th most used word. This stuck out to me with our current news coverage seemingly aligning the two groups.

But words by themselves definitely don't give the full picture, so next I pulled the bigrams - two words used next to each other - to see what pairs of words are most often used together.

![Graph showing top 50 bigrams used in tweets](https://i.imgur.com/x5sDbl9.png)

Here we see antifa and blm being used more often when paired. We also see more themes coming through like "support blm", "george floyd", "police brutality", "peaceful protest", etc.

Looking specifically at words, I then pulled the trigrams - three words used consecutively - to see if that would give more context to the themes of the tweets.

![Graph showing top 50 trigrams used in tweets](https://i.imgur.com/TLYIUh6.png)

This graph is really telling in comparison to the other two. We now see the phrases most often used are "justice peace blm", "protect black woman", "protect black man", and "protect black child". 

### Topic Modeling
<a id='topicmodeling'></a>
 
Topic modeling is the process of sorting text based on common themes found in the documents. It is commonly used in many areas. Consider how your gmail knows how to sort emails between "Primary", "Social", and "Promotions" without you having to add filters to your inbox.

Topic modeling isn't exact as it is unsupervised because it is working with unlabeled data, however, it is helpful to be able to draw out themes.

![image describing topic modeling](https://i.imgur.com/nfpfu5l.png?2)

For this project I used two different approaches to topic modeling: Latent Semantic Analysis and Latent Dirichlet Allocation.


#### Latent Semantic Analysis
I utilized two different approaches to topic modeling. The first was Latent Semantic Analysis (LSA). LSA is a way to retrieve information and it analyzes and finds patterns in an unstructured collection of text. It also looks at the relationship between these patterns. LSA is an unsupervised process to discover synonyms in a group of texts.
For this LSA, I used the Natural Language Processing with tfidf vectorizer and a truncated Singular Value Decomposition (SVD) to analyze the words used in the tweets. 

```
tfidf_vectorizer = TfidfVectorizer(stop_words='english', use_idf=True, smooth_idf=True)
reindexed_data = blm_tweets['tweet_lemmatized_no_blm']

reindexed_data = reindexed_data.values
document_term_matrix = tfidf_vectorizer.fit_transform(reindexed_data)
n_topics = 6
lsa_model = TruncatedSVD(n_components = n_topics)
lsa_topic_matrix = lsa_model.fit_transform(document_term_matrix)

def get_keys(topic_matrix):
    '''
    returns an integer list of predicted topic 
    categories for a given topic matrix'''
    keys = topic_matrix.argmax(axis=1).tolist()
    return keys

def keys_to_counts(keys):
    '''
    returns a tuple of topic categories and their accompanying magnitudes for a given list of keys'''
    count_pairs = Counter(keys).items()
    categories = [pair[0] for pair in count_pairs]
    counts = [pair[1] for pair in count_pairs]
    return (categories, counts)

def get_top_n_words(n, keys, document_term_matrix, tfidf_vectorizer):
    '''
    return a list of n_topic strings, where each string contains the n most common
    words in a predicted category, in order'''
    top_word_indices = []
    for topic in range(n_topics):
        temp_vector_sum = 0
        for i in range(len(keys)):
            if keys[i] == topic:
                temp_vector_sum += document_term_matrix[i]
        temp_vector_sum = temp_vector_sum.toarray()
        top_n_word_indices = np.flip(np.argsort(temp_vector_sum)[0][-n:],0)
        top_word_indices.append(top_n_word_indices)
    top_words = []
    for topic in top_word_indices:
        topic_words = []
        for index in topic:
            temp_word_vector = np.zeros((1,document_term_matrix.shape[1]))
            temp_word_vector[:,index] = 1
            the_word = tfidf_vectorizer.inverse_transform(temp_word_vector)[0][0]
            topic_words.append(the_word.encode('ascii').decode('utf-8'))
        top_words.append(" ".join(topic_words))
    return top_words

lsa_keys = get_keys(lsa_topic_matrix)
lsa_categories, lsa_counts = keys_to_counts(lsa_keys)

top_n_words_lsa = get_top_n_words(3, lsa_keys, document_term_matrix, tfidf_vectorizer)

```

This resulted in the following 6 topics:
![graph showing the 6 topics from LSA](https://i.imgur.com/kgD6l4E.png)

Again we see what we expect with black lives mattering. An interesting topic to arise was "protest today peaceful". We also see even though it is more recent in the BLM movement, George Floyd is a prominent name used. 

#### Latent Dirichlet Allocation

For my second round of topic modeling, I used Latent Dirichlet Allocation(LDA). LDA starts with the assumption that each group of text is a list of topics and each topic is a list of words. It uses the conditional probability that a word is linked to a particular topic. 

```
# Creating an instance for LDA
lda = LDA(n_components = 4, random_state = 1)

# Initialise the count vectorizer with the English stop words
count_vectorizer = CountVectorizer(stop_words='english')

#Fit and transform the processed titles
count_data = count_vectorizer.fit_transform(blm_tweets['tweet_lemmatized_no_blm'])

lda.fit(count_data)
```

I first ran this looking for 6 topics and then 5 after the LSA, but it seemed that there was so much overlap, reducing it to 4 topics made each topic more distinguished.

I made these wordclouds to look at the different topics to get a better picture of what each topic's theme:

![4 wordclouds of topics in #blm](https://i.imgur.com/K9WBxBZ.png)

Once again we see clear themes coming through with the words that are collected in each.

### Final Thoughts
<a id='finalthoughts'></a>

We can see with both the LSA and the LDA that the main topics of tweets with the hashtag BLM are referencing protecting black lives, communities that have been affected, victims of police brutality, and peaceful protesting. There is a definite topic that centralizes on politics but that topic (number 2) has the least amount of tweets and the shortest tweets. Terrorism and antifa are not as highly correlated as I anticipated based on mainstream media. Along with that, the sentiment analysis was inconclusive because of the context of the tweets, you couldn't say based on the sentiment if a tweet was positive towards BLM or negative towards BLM. Future work:

* side by side comparing with major news outlets
* side by side comparing with politicians statements or tweets
* digging deeper into the engagement of these tweets with likes and retweets and seeing which tweets have more impact

The full [github repository](https://github.com/roweyerboat/Twitter_Hashtag_Analysis) of the project.

Thank you!
