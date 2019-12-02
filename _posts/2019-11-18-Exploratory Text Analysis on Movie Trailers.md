---
layout: post
title: Exploratory Text Analysis on Movie Trailers
subtitle: Visualisation and Topic Modelling
gh-repo: sherr3h/sherr3h.github.io
gh-badge: [star, fork, follow]
tags: [FINA4350, text analysis, YouTube, comments, movies]
comments: true
---
by Sherry He

Bello!👋🏽<br /><br />
This blog post is supplementary to our group project report and covers texual analysis of exploratory nature. I will mainly show visualised results on bag-of-word models (word cloud) and topic models, using Youtube comment data under movie trailers. 

These two models are generally applied to understand (i.e.extract information from) short texts such as reviews, Twitter or emails, becuase the semantic structures of such texts are often very mixed-up 🤯.

**Bag-of-word models** are used because of simplicity. **Topic models** are important in that they form interpretable representations over documents.

As mentioned in other posts, we focused on movies produced by the 8 studios as follows:
* 21 Century Fox
* Disney
* Lion Gates
* Marvel
* Paramount 
* Universal
* Sony Pictures
* Warner Bros

## ☁️ Word cloud ☁️ ## 
Although the actual scientific merit of wordcloud is up for debate, I think scientific communication is a crucial part in data science. And pretty images are the best tool to make a great first impression. A marvelous illustration:

![try](/img/DIS_Marvel_word_cloud.png.png)

In my code, [Gensim][gensim] package is used for the td-idf model, and [Andreas Mueller’s word cloud][wordcloud] package for visuals. Also [NLTK][nltk] package is used to perform pre-processing. While other packages can do this too, I prefer to use a package specifically designed to perform these types of operations and allow rooms for customisation. 

## 🗣️Topic Modelling 🗣️ ## 

### A PLACEHOLDER ### 
<br /><br />


Due to time limit, we were not able to incorporate such textual information into our supervised machine learning task to predict entertainment firms' financial performances.

[wordcloud]: http://amueller.github.io/word_cloud/
[gensim]: https://github.com/RaRe-Technologies/gensim
[nltk]: https://github.com/nltk/nltk
