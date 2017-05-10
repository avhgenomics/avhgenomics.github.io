---
layout: page
title: Recognizing Handwritten Digits using Machine Learning
subtitle: Explanations and visualizations of my submissions to Kaggle's MNIST competition
bigimg: /img/path.jpg
published: yes
---

This is some of my work done to implement machine learning approaches to image recognition, focusing on the [Digit Recognizer competition over at Kaggle](https://www.kaggle.com/c/digit-recognizer).  
For current submission rankings, visit [my profile page there (hopefully updated soon)](https://www.kaggle.com/lyssavirus).  


# Background of the Dataset and Approaches

## The MNIST Dataset

The MNIST dataset is a great first step to understanding how machine learning approaches can be applied to datasets in order to achieve a goal.  In the case of MNIST, Kaggle provides 2 files:  

* A training file, consisting of 42,000 observations and 785 Variables.  
* A test file, consisting of 28,000 observations and 784 Variables.

*Why does the test file have one less variable?*  
The model that we want to develop need to be trained on a dataset that has known outcomes.  The extra variable is the output, in this case the number that is written.

This model, once trained, can then be applied to the test set to generate the labels to be submitted for scoring.

## Description of the Data

* Every observation is complete; there is no missing data, or inf, etc.
* The columns represent pixels in an image.  For this dataset, the image is 28x28 pixels, generating 784 total pixels.
* The value of each pixel is scaled between 0 and 255.  So what would a number look like?  We can see by restructuring a few of the values in a 28x28 matrix.  After doing so, one receives an image that looks something like this (not in yet): ![three](img\404-southpark.jpg)
