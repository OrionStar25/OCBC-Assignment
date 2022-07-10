### Observations: 

- Most commom words in the column "title":
    - Maximum frequency of all words in the news article titles is 3 and that too for "get". Its a verb which is not that significant in most context. 
    - Titles are just small summaries of the main content so we can rather explore the content column and expect an exhaustive result.

- Most commom words in the column "content":
    - Punctuation marks are also being considered as a word/part of a word, e.g.:
        - 3rd highest word is "..."
        - Several words have uneccesary symbols attached to them ("The, "I, insisted:)
    - "said" is mentioned twice in top 20, possibly due to new-line characters. 
    - Consequently, it can mean that same words in different cases will be treated differently, which can affect clustering even when the meaning of the words remain the same.
    - Top 20 words include insignificant verbs such as "also, would, said, could" that are used frequently in English language, but don't necessarily add much weightage to the jist of the article.
        - Hence, it is possible that articles with a lot of these words might get clustered together even though their topics are extremely different.
        
- Plotting Bigrams:
    - Maximum bigrams are hyphenated words (hip-hop, on-demand)
    - Abbreviation of words like we've, we're, we'll are treated as 2 different words.
    - Numbers with "," to denote 1000 is treated as 2 different words (500,000).
    - hip hop is the highest frequency bigram. It can mean that either several articles in the dataset use the term hip-hop or only 1 article uses hip-hop extensively, thereby dominating the frequency chart.
    
- Plotting Trigrams:
    - The hyphenated word "video-on-demand" is the maximum trigram being used. 