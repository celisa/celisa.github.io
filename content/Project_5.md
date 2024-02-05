+++
title = "Political Orientation Classifier"
template = "page.html"
date = 2022-12-15T15:00:00Z
[taxonomies]
tags = ["neural network", "Text Classification"]
[extra]
summary = "A project aimed at predicting the political orientation of a Twitter user based on their tweet(s). This project compares the performance of discriminative and generative models against real and synthetic data."
mathjax = "tex-mml"
+++

<!-- more -->

### Project Motivation
According to Pew Research Center, roughly one-quarter of American adults use Twitter, and often it is used to share their views about politics and political issues. Twitter can have a huge impact on the political sphere in the US, and it can be helpful to determine the political orientation of certain tweets to identify the presence of Democratic vs. Republican voice on a social media platform and the issues each party cares the most about.

<br>For this project, we are solving a text / document classification problem, and more specifically, we are creating a generative and a discriminative model to predict the political orientation (‘Democratic’ or ‘Republican’) of a tweet.

## Methodology

We employed the naive bayes classifier as a generative model to classify tweets based on political affiliation. A naive bayes classifier is a machine learning model that uses model features to discriminate between different objects. This model is based on the bayes’ theorem, which assumes that all features are independent of each other, or in other words, the presence of a particular feature in a class is unrelated to the presence of any other feature.

We compared the results against a bidirectional LSTM (biLSTM) model, which was chosen to perform the text classification task for the following reasons: 
1) LSTMs are great at retaining relevant information while addressing the exploding gradient problem
2) Bi-directional structures are designed to infer information both from the past and future. 

We did also consider using transformers, which are great at capturing long-range dependencies, but decided that LSTMs are more appropriate for our purpose given the shorter lengths of tweets and the desire for a simpler model. LSTM is an artifical neural network that is capable of processing the entire sequence of data with a memory cell known as a ‘cell state’ that maintains its state over time.

## Experiment Results

Please see the [detailed report](https://github.com/celisa/Political-Orientation-Classifier/blob/main/NLP%20Final%20Paper.pdf) for more details.

## Conclusion
The generative model performed relatively better on the text classification task compared to the discriminative model on both real and synthetic data. The generative model is faster to train on a small dataset and is more intuitive to interpret than the discriminative model. However, the generative model relies on the assumption that the data is sampled from a particular probability distribution, which is rarely true in reality. Therefore, if you generate synthetic data that is sampled from the assumed distribution, the generative model is expected to achieve a higher model accuracy and F-1 score compared to if the model was trained and tested on real data. 

The discriminative model will learn the characteristics of the training dataset through backpropagation, and given that the test data is similar to the training data, the discriminative model should perform well. However, this is computationally very intensive and will require a lot of data, computing power, and time to achieve good results, which was not feasible for our analysis. 

### Links
[Github link](https://github.com/celisa/Political-Orientation-Classifier/tree/main) <br>
[Detailed report](https://github.com/celisa/Political-Orientation-Classifier/blob/main/NLP%20Final%20Paper.pdf) 