---
layout: post
title: Project Initiatives
subtitle: Our Observation, and Word Cloud, from Youtube Video Comments
gh-repo: sherr3h/sherr3h.github.io
gh-badge: [star, fork, follow]
tags: [FINA4350, text analysis, YouTube, comment, gaming, movie]
comments: true
---
by Sherry He

Check out the comment section of those Youtube gaming videos. What do they say about the giant gaming firms' upcoming financial performances?

Early research on stock market prediction was based on random walk theory and the Efﬁcient Market Hypothesis (EMH), which assumes that EMH stock market prices are largely driven by new information, i.e. news, rather than present and past prices. Since news is unpredictable, stock market prices will follow a random walk pattern.

Yet the assumptions are often challenged, and recent research suggests that news / social media may be unpredictable but that very early indicators can be extracted online to predict changes in various economic and commercial indicators. This may conceivably also be the case for the stock market.

<a href="https://youtu.be/a7ZpQadiyqs" target="_blank"><img src="https://hb.imgix.net/29fd791ab6e5738f14bb7f547675378b617a9e00.jpg?auto=compress,format&fit=crop&h=353&w=616&s=a8887ddc9d96988b11b46400691728ae" alt="Battlefield V" width="400" border="10" /></a>  << click to watch this trailer video

A case in point is Battlefield V. See the like/dislike ratio of the trailer video above. Apparently the video game producer (Electronic Art) mistakenly thought their market, i.e. the gaming community, were still keen on futurist / irrealist games. Although it's too early to say these were the only reason, EA stock price has indeed tumbled during the period:

<img src="https://github.com/sherr3h/sherr3h.github.io/blob/master/img/EA.png?raw=true" alt="EA" width="500" class="center"/>

Our project initiative is to use the comment section as an alternative data source and perdict gaming firms' financial performance and stock price movements. 

As a starter, I did some exploratory textual analysis. Here I'd like to share how to make a wordcloud. Below are example word clouds of some games.

Battlefield V (which unfortunately received excessive negative reviews):  
<img src="https://github.com/sherr3h/sherr3h.github.io/blob/master/img/battlefieldV_wordcloud2.png?raw=true" alt="BattleV" width="500"/> 

Call of Duty Infinite Warefare:   
<img src="https://github.com/sherr3h/sherr3h.github.io/blob/master/img/cod_wordcloud_try1.jpg?raw=true" alt="cod" width="500" class="center"/>  

Pokemon: Let's go, Pikachu!  
<img src="https://github.com/sherr3h/sherr3h.github.io/blob/master/img/pokemon_wordcloud_try2.png?raw=true" alt="Pikachu" width="500" class="center"/>  

Wordclouds are useful in several ways:
* It's a direct representation of what's included in the document 
* Wordclouds of different corpus can be used to intuitively compare the similarity (since our brain is certainly the best NLP machine :)
* It can be used to check whether data pre-processing stage is done properly, e.g. if all stopwords are removed
* It is eye-appalling and useful if you're going to talk about some boring data analysis later 

Here's the minimal version of python code to realise a wordcloud:
```python
# Start with loading all necessary libraries
import numpy as np
import pandas as pd
from os import path
from PIL import Image
from wordcloud import WordCloud, STOPWORDS, ImageColorGenerator

import matplotlib.pyplot as plt

# Load the dataframe that contains comments
# You can download all our raw on our github
comments_df = pd.read_csv("Battlefield V_comments.csv", index_col=0)

# Join comments 
text = " ".join(t for t in comments_df.Comment)

# Create stopword list:  
stopwords = set(STOPWORDS)
# we recommend using customised stop words apart from the set by word cloud
stopwords.update(["like", "wait", "game"])

# Generate a word cloud image
wordcloud = WordCloud(stopwords=stopwords, background_color="black").generate(text)

# Create and generate a word cloud image:
wordcloud = WordCloud().generate(text)

# Display the generated image:
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis("off")
plt.show()

wordcloud.to_file("img/first_review.png")
```

Hope you find this interesting. Stay tuned to see updates of our project!

