---
layout: post
title: Question Asking and Answering with Sentence-BERT and Berkeley Neural Parser 
gh-repo: sherr3h/sherr3h.github.io
tags: [Natural Language Processing, Transformer]
comments: true
---

This post is extended from a Natural Language Processing course project I did with two teammates. The task is to generate questions as well as answer questions give an Wikipedia article. We got the 2nd highest grade, and I was asked by the instructor to be a teaching assistant for the course (but I graduated that semester).

In a nutshell, for the question asking part, we applied [Berkeley Neural Parser](https://pypi.org/project/benepar/) to create basic parsing rules for both binary and Wh-questions. For the question answering part, we utilized [Sentence-BERT](https://arxiv.org/abs/1908.10084), which uses a siamese network to train sentence embeddings, found candidate answers by comparing cosine-similarity, and handled different types of question variations.


## 1. Task Setup and Our Results 

Inputs of the question asking task include a Wikipedia article without any processing and the number of questions required n. We need to generate different types of questions, with some parts of the question substitued with synonyms or wrong informations. For example, these are our questions generated from the Wikipedia entry about [Kangaroos](https://en.wikipedia.org/wiki/Kangaroo).

<img src="/img/KangarooQuestions.png" width="720" >

Inputs of the question answering task include a Wikipedia article and questions. We need to answer the question correctly and as concisely as possible. Below are our answers about Kangaroos. Some answers are incorrect unfortunately.

<img src="/img/KangarooAnswers.png" width="720" >

## 2. Sentence-BERT (SBERT) Overview

S-BERT is a pre-trained sentence embedding model that builds on the BERT architecture and uses a Siamese network to generate high-quality sentence embeddings. The Siamese network is composed of two identical subnetworks, each taking a sentence as input and producing a fixed-length vector representation. The subnetworks share weights and architecture, which allows them to learn to encode sentences into meaningful vector representations that capture their semantic meaning.

During training, SBERT is fed pairs of sentences and learns to predict whether the two sentences have the same meaning or not. The network then adjusts its weights to minimize the contrastive loss function, which measures the difference between the predicted similarity scores and the true labels.

Once trained, SBERT can encode any given sentence into a dense, fixed-length vector representation that captures its semantic meaning. These sentence embeddings can be used for a range of NLP tasks, such as text classification, semantic similarity analysis, and information retrieval.

One of the key benefits of SBERT is its ability to handle sentence-level similarity. By encoding similar sentences into similar vector representations, SBERT can be used for tasks such as finding the most similar pair of sentences or determining whether two sentences have the same meaning. Additionally, SBERT can be fine-tuned on specific tasks using only a small amount of task-specific data, making it a flexible and powerful tool for NLP applications.

In conclusion, Sentence-BERT is a pre-trained sentence embedding model that uses a Siamese network to generate high-quality sentence embeddings. By sharing weights and architecture between subnetworks, SBERT can encode sentences into dense, fixed-length vector representations that capture their semantic meaning. This approach has proven to be effective for a range of NLP tasks, making SBERT an essential tool in the NLP toolbox.
