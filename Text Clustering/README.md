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
    
- **Assumption**: There are 60 articles in the dataset. Assuming that each article belongs to a single, mutually-exclusive topic, `no. of topics` is initially set to `60`.

1. *Output[20]:*
    - 60 topics with 10 most prominent keywords representing each topic are shown. Each keyword has a probability associated with it.
    - For e.g. `Topic 23` seems to be about gaming consoles, `Topic 3` about local broadband services, `Topic 4` about mobile camera analysis. 
    
    ```
    # Topic numbers are 0-indexed, hence 3 represents Topic 4.
    (3, '0.000*"camera" + 0.000*"phone" + 0.000*"mobile" + 0.000*"image" + 0.000*"people" + 0.000*"feature" + 0.000*"sold" + 0.000*"quality" + 0.000*"report" + 0.000*"said"')
    ```

2. *Output[21]:*
    - The visualization is interative.
    - Each bubble represents a topic. The larger the bubble, the higher percentage of the number of articles in the corpus is about that topic.`Topic 8` is the largest bubble talking about argentina programs and crisis involving the government. 
    - Blue bars represent the overall frequency of each word in the corpus. The word `said` was generated the most which also has the highest frequency as per `Output[8]`.
    - Red bars give the estimated number of times a given term was generated by a given topic. The word `camera` is generated `20` times with approximately `18` times in `Topic 15`.
    - The further the bubbles are away from each other, the more different they are. 
        - `Topic 32 and 55` seem to be about sports and are hence close together. 
        - `Topics 5 and 22` are related to finance, hence closer to each other but far from `Topic 32 and 55`.
        - `Topics 9 and 12` overlap as they seem to be a mix of politics and internet.

3. *Output[23]:*
    - A good topic model should have big and non-overlapping bubbles scattered throughout the chart. Since we see a lot of bubbles clustered in one place, we evaluate its performance. We get a coherence score of `0.5087956911487027`.

### 4. Use the LDA Mallet Clustering Algorithm

- Multiple existing literature states that the LDAMallet model provides a better coherence score over the vanilla LDA model. Specifically, Gensim’s (vanilla) LDA uses a Variational Bayes sampling method which is faster but less precise that Mallet’s Gibbs Sampling. [Ref](https://mimno.github.io/Mallet/index).

1. *Output[25]:*
    - Coherence score of an `ldamallet` model with the same no. of topics as before (`60`) is `0.5596391456880173`. This is a significant improvement over our benchmark of vanilla LDA with 60 topics. 

### 5. Compare models to get the best clustering algorithm (LDA vs LDA Mallet)

- It is possible that the coherence score of LDAMallet was higher than LDA for just one instance, i.e., when the no. of topics is 60. In this case, our assumption that LDA Mallet is the better performing algorithm would be wrong. Hence, we plot the trend of coherence scores between the 2 algorithms as a function of number of topics.

1. *Output[28]:*
    - For any given no. of topics, starting from 2 and limiting at 60, LDAMallet performs better. 
    - Eventhough the coherence score is increasing with the increase in no. of topics, score at `60` is lesser than score at `50, 54, 58`. Hence, we need to find the optimal no. of topics.

### 6. Evaluate best model coherence measure

- Since coherence scores measure the similarity of words in a topic to each other, it is important to validate if we are relying on the correct coherence measure.

1. *Output[31] and Output[102]:*
    - 4 types of measures namely CV, UMass, UCI, NPMI are compared for the LDAMallet model for topics ranging from 2 to 60.
    - The plot shows the CV is the most stable in all the 4 cases. (Steadily increasing with increasing no. of topics, with few local maxima).

### 7. Analyse optimal number of topics

1. *Output[37]:*
    - This is a visualization for LDAMallet model with 54 topics, model with the highest coherence score of `0.5867063198582754`.
    - A lot of bubbles are non-overlapping and spaced out throughout the chart.
    - However, a large cluster of overlapping topics is created in the 3rd quadrant. It is also difficult to understand the underlying topics of these bubbles using the given keywords. 
    
2. *Output[38]:*
    - Using the `elbow-rule` and `Output[102]`, **I selected `36` as the number of topics for my optimal model**. After this, the increase in coherence score is slow and unstable.
    - We see non-overlapping and spaced out bubbles throughout the chart.
    - There is still an overlapping cluster in the 2nd quadrant, but its not as dense.
    - It's easier to understand the underlying topics using the keywords. E.g.,
        - `Topics 35, 6, 7, 27` all are simlar to Politics, hence overlapping. 
        - `Topics 29, 32, 10, 11` relate to people in specific countries and are therefore close together.
        - `Topics 23, 8` are broadly about computing devices.

### 8. Find the most dominant topic in each news article

1. *Output[41]:*
    - It shows the most prevalent topic associated with each news article and its percentage contribution in the article.
    - It also shows the most important keywords present in the article which are associated with the topic.
    - E.g.` Topic 25` is dominant for `Article 1` which is mostly about `different genders and winning money as prize in tennis matches` (derived from the list of keywords present).

### 9. Find the most dominant news article for each topic

1. *Output[43]:*
    - It shows the most relevant article associated with each topic and its percentage contribution in the topic.
    - This can help us to gain more insight about a particular topic by reading only 1 article instead of all the articles that contribute to that topic. 

### 10. Find the number of news articles belonging to 1 topic

1. *Output[101]:*
    - It shows the no. of articles contributing to each topic. The sum of this is 60, denoting total no. of news articles provided in the dataset. 
    - Maximum value of this is 4 for `Topic 26`.
    
-----------------
    
## Conclusion

- The given news article dataset was successfully clustered into 36 topics using a LDAMallet model with a coherence score(CV) of 0.54.
- Futher insights relating to the generated topics and individual articles was also derived. 

----------------

## Future Scope

- 
    



    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    