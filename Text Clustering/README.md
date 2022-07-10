# README

----------

## Problem Statement

Please refer to `question.md`

----------

## Setup 

- All code and corresponding graphs are presented in `Solution.ipynb`.
- An external package `mallet-2.0.8` was used for modelling purposes. It can be downloaded from [here](http://mallet.cs.umass.edu/dist/mallet-2.0.8.zip).
- I made a pickle file of the best models I got which is not included in this repo.
- Refer to `requirements.txt` for all package imports.
- Python version = `Python 3.9.10`

----------

## Solution

The solution is divided into 10 sections. Steps and explanation for each section is described below.

### 1. Data Exploration

1. *Output[7]:*
    - Maximum frequency of all words in the news article titles is `3` and that too for `get`. It's a verb which is not significant in most context. 
    - Titles are just small summaries of the main content so we can rather explore the `content` column and expect an exhaustive result.

2. *Output[8]:*
    - Punctuation marks are also being considered as a word/part of a word, e.g.:
        - 3rd highest word is `...`.
        - Several words have uneccesary symbols attached to them (`"The`, `"I`, `insisted:`).
    - `said` is mentioned twice in Top 20, possibly due to new-line characters. 
    - Consequently, it can mean that same words in different cases will be treated differently, which can affect clustering even when the meaning of the words remain the same.
    - Top 20 words include insignificant verbs such as `also`, `would`, `said`, `could` that are used frequently in English language, but don't necessarily add much weightage to the jist of the article.
        - Hence, it is possible that articles with a lot of these words might get clustered together even though their topics are extremely different.
        
3. *Output[10]:*
    - Maximum bigrams are hyphenated words (`hip-hop`, `on-demand`).
    - Abbreviation of words like `we've`, `we're`, `we'll` are treated as 2 different words.
    - Numbers with `,` to denote 1000 is treated as 2 different words (`500,000`).
    - `hip hop` is the highest frequency bigram. It can mean that either several articles in the dataset use the term `hip-hop` or only 1 article uses `hip-hop` extensively, thereby dominating the frequency chart.
    
4. *Output[11]:*
    - The trigram `video-on-demand` is a hyphenated word and is used the most. 
    
### 2. Data Cleaning

Multiple steps are taken to clean the data. 
    - Remove new line characters.
    - Remove single quotes (`'`).
    - Remove double quotes (`"`).
    - Remove hyphens (`hip-hop` --> `hiphop`).
    - Remove stop words such as `the`, `of`, `is`, etc.
    - Remove punctuations and unecessary symbols (`,`, `:`).
    - Convert entire text to small-case in order to treat the same words equally (`World` = `world`).

1. *Output[16]:*
    - Word frequency distribution of all the news articles is ploted. 
    - `mr`, `people`, `year`, `new` dominate the plot. It can be inferred that several articles were referring to men and people in general.

### 3. Use the LDA Clustering Algorithm

- Since the `news_data` dataset is unlabelled, we need to apply unsupervised learning techniques in order to gain insights. 

- We can create `k` clusters denoting `k` different topics and assign each article to one of the clusters. Therefore, there would be a 1-1 mapping between a topic and a news article. However, an article can be a combination of more than one topic (many-to-one). That is, each article can be represented as a distribution of `k` topics, with each topic having a probability associated with it. Hence, the LDA algorithm is used since it produces more than one topic per document. 

- The `content` is preprocess before being fed as an input to the model.
    - Sentences are tokenized into individual words. 
    - Words are lemmatized (`made` and `make` are changed to their base word `make`).

- A `corpus` is created which represents each article as a list of processed words.

- A `bag of words` corpus is used instead of `tf-idf`. Since `tf-idf` tells us the weightage of a word in a document, and `LDA` cares about distribution of words in topics, `tf-idf` is unecessary and complex. 
    
- *Assumption*: There are 60 articles in the dataset. Assuming that each article belongs to a single, mutually-exclusive topic, `no. pf topics` is initially set to `60`.

1. *Output[20]:*
    - 60 topics with 10 most prominent keywords representing each topic are shown. Each keyword has a probability associated with it.
    - For e.g. `Topic 23` seems to be about gaming consoles, `Topic 3` about local broadband services, `Topic 4` about mobile camera analysis. 
    
    ```
    # Topic numbers are 0-indexed, hence 3 represents Topic 4.
    (3,
  '0.000*"camera" + 0.000*"phone" + 0.000*"mobile" + 0.000*"image" + 0.000*"people" + 0.000*"feature" + 0.000*"sold" + 0.000*"quality" + 0.000*"report" + 0.000*"said"')
    ```

2. *Output[21]:*
    - The visualization is interative.
    Each bubble represents a topic. The larger the bubble, the higher percentage of the number of tweets in the corpus is about that topic.
Blue bars represent the overall frequency of each word in the corpus. If no topic is selected, the blue bars of the most frequently used words will be displayed.
Red bars give the estimated number of times a given term was generated by a given topic. As you can see from the image below, there are about 22,000 of the word ‘go’, and this term is used about 10,000 times within topic 1. The word with the longest red bar is the word that is used the most by the tweets belonging to that topic.
The further the bubbles are away from each other, the more different they are. For example, it is difficult to tell the difference between topics 1 and 2. They seem to be both about social life, but it is much easier to tell the difference between topics 1 and 3. We can tell that topic 3 is about politics.

A good topic model will have big and non-overlapping bubbles scattered throughout the chart. As we can see from the graph, the bubbles are clustered within one place. Can we do better than this?

Yes, because luckily, there is a better model for topic modeling called LDA Mallet.

3. *Output[23]:*

### 4. Data Cleaning

5. *Output[11]:*

6. *Output[11]:*

7. *Output[11]:*

### 5. Data Cleaning

### 6. Data Cleaning

### 7. Data Cleaning

### 8. Data Cleaning

### 9. Data Cleaning

### 10. Data Cleaning
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    