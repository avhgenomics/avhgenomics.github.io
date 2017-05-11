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
* The value of each pixel is scaled between 0 and 255.  So what would a number look like?  We can see by restructuring a few of the values in a 28x28 matrix.  After doing so, one receives an image that looks something like this: ![three](/img/data/mnist/plots/8.png)

# Using the randomForest package to classify MNIST (R)

## Setting the Benchmark

The first approach to developing a model to classify the MNIST data is to try a randomForest model.  

* I used R to do this with the package 'randomForest'

To provide a benchmark, the initial data.matrix was run through the randomForest() with the target as the respective label; the number of trees was set to 1000.  
Surprisingly, this resulted in a high degree of accuracy on the test set,0.96771 when scored by Kaggle.  Not bad for a first run, but a couple of notes and improvements can be made.

* This model was made without splitting the training data.  Normally, it seems to be good practice to split the training data (~75-80%/20%).  Your initial model can be trained on the 80% then tested on the 20%, of which you know the true outcome.  Multiple sets of sampling like this can help to build robustness and reproducibility in your model.  

* Training this model took ~1-2 hours.  But plotting to investigate the ntrees value vs accuracy revealed that gains were minimal beyond ntrees=~200.  Therefore, we can refine this option to 250, speeding up our times considerably without sacrificing much in terms of accuracy.

* One interesting piece of information that is returned is the "Importance" of each variable in the classification.  Initially just a row of 784 numbers, we can do as before and turn this into a 28x28 grid, then plotted to see what parts of the image play large roles in determining the label.  
![Importance.bm](/img/data/mnist/plots/importance_nocol_lines_benchmark.png)
