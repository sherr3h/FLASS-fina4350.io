---
layout: post
title: Question Asking and Answering with Sentence-BERT and Berkeley Neural Parser 
gh-repo: sherr3h/sherr3h.github.io
tags: [Natural Language Processing, Transformer]
comments: true
---

This post is extended from a Natural Language Processing course project I did with two teammates. The task is to generate questions as well as answer questions give an Wikipedia article. We got the 2nd highest grade, and I was asked by the instructor to be a teaching assistant for the course (but I graduated that semester).

In a nutshell, for the question asking part, we applied [Berkeley Neural Parser](https://pypi.org/project/benepar/) to create basic parsing rules for both binary and Wh-questions. For the question answering part, we utilized [Sentence-BERT](https://arxiv.org/abs/1908.10084), which uses a siamese network to train sentence embeddings, found candidate answers by comparing cosine-similarity, and handled different types of question variations.
