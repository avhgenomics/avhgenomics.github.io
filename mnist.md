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
* The value of each pixel is scaled between 0 and 255.  So what would a number look like?  We can see by restructuring a few of the values in a 28x28 matrix.  After doing so, one receives an image that looks something like this:

![three](/img/data/mnist/plots/8.png)

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

# The Current Model: Implementing a Neural Net through H2O and R

## The Setup

[H2O](https://www.h2o.ai) is a system that can work through R to implement a wide range of machine learning algorithms.  To get started with this, we first need to import the data and prepare it for modeling.

{% highlight R %}

library(h2o)
localH2O = h2o.init(nthreads=-1)

train <- h2o.importFile("input/train.csv")
test <- h2o.importFile("input/test.csv")

{% endhighlight %}

Before this data can be used, the features have to be properly tagged so H2O can aim for the right output.  The training set going in is 785 columns, shown as integers (all whole numbers), but one of these columns is supposed to be a factor; enum if using the GUI for H2O.  This will provide the target that each observation is supposed to represent.

* We need to convert column 1 to factor.  

* Then split the data 75%/25% to train the model and validate it.

{% highlight R %}

train[,1] <- as.factor(train[,1])
train.split<-h2o.splitFrame(train,ratios = c(0.75))

train.set<-h2o.assign(train.split[[1]],key = "training.hex")
valid.set<-h2o.assign(train.split[[2]],key = "validation.hex")


{% endhighlight %}


## Training the model

With the dataset prepared and split, the model can now be trained and tested.  Just as the randomForest did, using some "standard" inputs will provide a baseline to see how well this algorithm works out of the box.

* Train the model

* Validate it on the validation set.

* Look at the Confusion Matrix and see where things can be improved.

{% highlight R %}

mnist.dnn <- h2o.deeplearning(x=2:785,
                              y=1,training_frame = train.set,
                              model_id = "seagulls",
                              validation_frame = valid.set,
                              variable_importances = T,
                              score_each_iteration = T,
                              activation = "RectifierWithDropout",
                              train_samples_per_iteration = -1,
                              stopping_rounds = 10,
                              sparse = TRUE,
                              epochs = 100,
                              hidden = c(200,200))

h2o.performance(model = mnist.dnn,train = T)
plot(mnist.dnn)

{% endhighlight %}

Unfortunately, this model doesn't perform very well on submitted data.  But now, we have a baseline for improvements, which can be implemented by tuning the parameters of the model.

## Tuning the model

* Use the grid system in H2O to tune certain parameters.

* Check which model is the best in terms of accuracy and performance.

{% highlight R %}

hyper_params = list(hidden = list(c(1000,1000,2000)),
                     input_dropout_ratio = c(0,0.01,0.03,0.05),
                     l1 = seq(0, 1e-7,1e-3),
                     l2 = seq(0,1e-7,1e-3)
                     )
search_criteria = list(strategy = "RandomDiscrete", max_runtime_secs = 2000, max_models = 100, stopping_rounds=10, stopping_tolerance=1e-5)

mnist.grid <- h2o.grid(algorithm = "deeplearning",
                       grid_id = "seagulls.grid",
                       x=2:785,
                              y=1,training_frame = train.set,
                              model_id = "seagulls",
                              validation_frame = valid.set,
                              variable_importances = F,
                              score_each_iteration = T,
                              activation = "RectifierWithDropout",
                              train_samples_per_iteration = -1,
                              stopping_rounds = 10,
                              sparse = TRUE,
                              epochs = 1000,
                              hyper_params = hyper_params,
                              search_criteria = search_criteria
                       )


grid <- h2o.getGrid("seagulls.grid")
model <- h2o.getModel(grid@model_id[[1]])
h2o.confusionMatrix(model)


{% endhighlight %}

* At some point I went back to this grid and tried different parameters... The grid that gave me the parameters used isn't shown above.


## The current developed model

{% highlight R %}

mnist.dnn2 <- h2o.deeplearning(x=2:785,
                              y=1,training_frame = train,
                              checkpoint = "seagulls_hp_c1",
                              model_id = "seagulls_hp_c2",
                              validation_frame = valid.set,
                              score_each_iteration = T,
                              activation = "RectifierWithDropout",
                              train_samples_per_iteration = -1,
                              stopping_rounds = 10,
                              sparse = TRUE,
                              epochs = 5000,
                              rate = 0.005,
                              rate_annealing = 1e-06,
                              rate_decay = 1,
                              input_dropout_ratio = 0.05,
                              l1 = 7e-06,
                              l2 = 3.4e-05,
                              hidden = c(500,500))
{% endhighlight %}

This model was what I had come up with; it still needs improvement.  Currently, it received an accuracy of roughly ~98% when submitted for scoring at Kaggle.  Room to improve includes:

* Altering the images.  Manipulating the whole set in terms of size, angle, etc; This would help to bolster the effectiveness of recognizing a handwritten digit.  As shown by a few numbers, not all of them are written at a perfect angle; but the goal is to build a generalized, robust model.

* Moving these calculations to the GPU.  I think this model can be improved through testing multiple node setups, but have been hitting memory issues.  Shifting this info to the GPU won't fix that problem, but it will speed up the process of generating models considerably.  To fix the size, more memory would be required.
