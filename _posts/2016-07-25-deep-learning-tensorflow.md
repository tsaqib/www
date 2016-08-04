---
layout: post
title:  "Walkthrough: Deep Learning using TensorFlow"
date:   2016-07-25 19:40:19 +0600
categories: Deep Learning
permalink: /:slug
comments: true
thumbnail: /assets/deep-learning-momentum.png
---
This is a walkthrough to writing a Deep Learning implementation using TensorFlow. I'm no expert in Machine Learning, therefore, I expect that you learn the theory by yourself before trying to understand the code here. However, there is no harm in tinkering with it. 

## Deep Learning: The momentum
Buzzwords take over our world. In recent history, 'social media', 'internet of things' and now 'deep learning' really take over the planet in momentum. Lets take a look what world think interesting in last more than 10 years among Computer Science-related fields of study.

![Deep Learning momentum]({{ site.url }}/assets/deep-learning-momentum.png)

From academic perspective, among the fields of study compared as above, Deep Learning seems to be at #1 position, followed by Analysis of Algorithms at #2, Computer Security is #3, and surprisingly Artificial Intelligence is at #4. Lastly, Computer Graphics is at the bottom of people's interest.

## TensorFlow: Why?
It's been less than a week, I have got my hands on coding to solve problems using Deep Learning. It is not wise to write about an academic topic which deserves to be mastered in the first place. However, I wore my developer hat intended to write something about how I wrote my first [TensorFlow](https://www.tensorflow.org) based Deep Learning solution so that it won't get lost. 

![TensorFlow momentum]({{ site.url }}/assets/tensorflow-momentum.png)

I have used [Theano](http://deeplearning.net/software/theano/) before (briefly), and I am currently in the middle of a decision on which toolchain I should settle in. I'm currently also considering [H2O](http://www.h2o.ai) stack. The former has a huge community around the world, and the latter is sheer eye-candy. However, Google's TensorFlow is most definitely having its time with hype, utility, and developer-friendliness. Just look at the spike in popularity at launch. It sure takes serious $ to make that happen, however, they have managed to keep a steady interest among the practitioners apparently. By the way, both Theano and TensorFlow are GPU-capable.   

## Dataset and expected outcome 
The [Car Evaluation](http://archive.ics.uci.edu/ml/datasets/Car+Evaluation) dataset is publicly available and its simplicity makes it approachable for noobs such as myself. I encourage you to go ahead and check it out by yourself. The dataset itself boils down to some sort of survey, which essentially takes people's feedback on cars based on its price, maintenance cost, how many doors it has, how many people can ride, etc. 

Sample data looks like this:

{% highlight bash %}
vhigh,vhigh,2,2,small,low,unacc
vhigh,vhigh,2,2,small,med,unacc
vhigh,vhigh,2,2,small,high,unacc
vhigh,vhigh,2,2,med,low,unacc
vhigh,vhigh,2,2,med,med,unacc
...
{% endhighlight %}

The program that I have written is capable of parsing the data file which is in CSV, and training a multi-layer perceptron model with backpropagation so that it may mimic human intellect and decide for us when unseen data points are encountered. 

The program should output as follows (but, fret not!):

{% highlight bash %}
Shape: (1727, 7)

Data types:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 1727 entries, 0 to 1726
Data columns (total 7 columns):
vhigh      1727 non-null int8
vhigh.1    1727 non-null int8
2          1727 non-null int8
2.1        1727 non-null int8
small      1727 non-null int8
low        1727 non-null int8
unacc      1727 non-null int8
dtypes: int8(7)
memory usage: 11.9 KB

Top 5 rows: 
   vhigh  vhigh.1  2  2.1  small  low  unacc
0      3        3  0    0      2    2      2
1      3        3  0    0      2    0      2
2      3        3  0    0      1    1      2
3      3        3  0    0      1    2      2
4      3        3  0    0      1    0      2

Data statistics: 
          count      mean       std  min  25%  50%  75%  max
vhigh    1727.0  1.499131  1.118098  0.0  0.5  1.0  2.0  3.0
vhigh.1  1727.0  1.499131  1.118098  0.0  0.5  1.0  2.0  3.0
2        1727.0  1.500869  1.118098  0.0  1.0  2.0  2.5  3.0
2.1      1727.0  1.000579  0.816615  0.0  0.0  1.0  2.0  2.0
small    1727.0  0.999421  0.816615  0.0  0.0  1.0  2.0  2.0
low      1727.0  1.000000  0.816970  0.0  0.0  1.0  2.0  2.0
unacc    1727.0  1.552982  0.876136  0.0  1.0  2.0  2.0  3.0
-

Accuracy on test dataset: 1.000000
Accuracy on training dataset: 1.000000

{% endhighlight %}

The last two lines of the output are more relevant than anything else. This basically says that the training conducted gave the ability to predict its output class 100% of the time. Although, our goal is always to maximize accuracy, it is not always the case, especially due to the fact that we randomize the training and test data. More on that later. The first part of the output are essentially bunch of statistics which help us to make sense out of the data, or simply give us more information. Feel free to ignore that. 

If you have paid attention to the dataset, especially take a look at the top 5 rows, you will quickly notice that the dataset was changed to some numeric representation in our output.

## TensorFlow toolchain
Google is trying to bring a complete toolchain for TensorFlow, although incomplete at the moment, I think they are on right track. TensorFlow is fun, but its simply too explicit and hinder developer productivity. Therefore, they have been building [TFLearn](http://tflearn.org), their [Scikit Learn](http://scikit-learn.org)-inspired API for TensorFlow, that is awesome fun to write code against. Some of the other tools that I used, along with TFLearn and Scikit Learn, in my pipeline were [pandas](http://pandas.pydata.org/), [NumPy](http://www.numpy.org/) and obligatory [SciPy](https://www.scipy.org/). It is easy to setup all of them at once.  

## Setting up the environment

First of, download the code from [here](https://github.com/tsaqib/samples/tree/master/categorical_dnn), open a Terminal, and execute the following:

{% highlight bash %}
cd downloaded_folder
virtualenv .
source bin/activate
pip install -r requirements.txt
{% endhighlight %}

Now that you have all the required packages installed, go ahead and run by `python main.py`. You should expect to see a similar output as stated above.

## Code Walkthrough
The code consists of the following four files:

- car-data.csv: as you can understand this is the dataset
- requirements.txt: this file contains the list of packages to be installed
- main.py: this is the entry point for the code
- categorical_dnn.py: the class which contains all the Deep Learning logic 

The contents of the main.py are fairly straight forward. As it can be seen that it only instantiates the `CategoricalDNN` class and passes some important parameters. Learning rate, training size, and iterations can be specified from here. By training size, it is meant that what % of the data would be used for training and the rest for the test.  

{% highlight python %}
from categorical_dnn import CategoricalDNN

LEARNING_RATE = 0.1
TRAINING_SIZE = 0.75 # 75%;
ITERATIONS = 1800

def main():

    dnn = CategoricalDNN('car-data.csv', TRAINING_SIZE, LEARNING_RATE,
                            ITERATIONS)
    dnn.data_insights()
    dnn.train()

if __name__ == "__main__":
    main()
{% endhighlight %}

It is important to note that the `CategoricalDNN` class assumes that the invoker has no clue about the data, meaning that the class itself is able to extract meaning from the data, preprocess and train itself. However, needless to mention that it would only work better for categorical data. 

### The Categorical DNN

Let us begin by looking at the packages this class makes use of:
{% highlight python %}
import pandas as pd
import tensorflow as tf
import tensorflow.contrib.learn as tflearn
from sklearn import metrics
from sklearn.cross_validation import train_test_split
import os.path
import numpy as np
{% endhighlight %}

Although the code is fairly self-explanatory with complementary inline comments, important parts are discussed here in the sequence of invocation. 

#### Preprocessing data
First of, `_load_csv` is a (meant to be) private method, which loads the CSV into memory using pandas. It then goes on to count how many columns the dataset contains. It assumes that the last column contains the `label` or `class` information, against which the dataset needs to be trained.  

{% highlight python %}
def _load_csv(self):

    # Loading CSV into the system using Pandas
    self._input_map = []
    self._raw_data = pd.read_csv(self._file_name, sep=',', 
                                    skipinitialspace=True)
    self._datadim = len(self._raw_data.columns)

    # Find uniques in the last column aka. `label` or `class`
    self._classes = self._raw_data[
                        self._raw_data.columns[self._datadim - 1]
                    ].unique()

    # Create a map of all unique values by columns
    for i in range(self._datadim):
        self._raw_data[self._raw_data.columns[i]] = \
            self._convert_categorical_nominal(self._raw_data, i)
{% endhighlight %}

It then figures the unique classes the last column contains, which is an important metric for designing the classifier, again using pandas. In the end, all the columns are looped over, and all strings are converted into respective categorical values for our classifier to deal with. Actually a nifty [pandas trick](https://github.com/tsaqib/samples/blob/master/categorical_dnn/categorical_dnn.py) does the heavy-lifting.

#### Shuffling and Splitting
The dataset is now randomized and split into the ratio previously defined using `TRAINING_SIZE`. 
{% highlight python %}
def _shuffle_split(self):
    self._raw_data.iloc[np.random.permutation(len(self._raw_data))]
    self._testdata, self._traindata = train_test_split(
                        self._raw_data, test_size=self._training_size)

    # TF Learn / TensorFlow only takes int32 / int64 at the moment 
    # as oppose to int8
    self._train_label = [int(row) for row in self._traindata[
                            self._raw_data.columns[self._datadim - 1]]]
    self._test_label = [int(row) for row in self._testdata[
                            self._raw_data.columns[self._datadim - 1]]]
    self._traindata = self._traindata.ix[:, range(self._datadim - 1)]
    self._testdata = self._testdata.ix[:, range(self._datadim - 1)]
{% endhighlight %}

The classifier can only work with int32 / int64, therefore, it was required to convert both the training and test labels. In the end, it was also made sure that the labels themselves aren't being fed into the classifier, so they are filtered out in the last two lines.

#### Training the Model and Measuring Performance
Now that we are done with all the preparations for the training, let us go ahead and design the network. The hidden layers can be arranged in any way that seems suitable or yields better results, but from our observation the following design suits us best. There are also literatures which propose or establish many best practices, but I am not going into that discussion, simply because I do not know enough.

{% highlight python %}
def train(self):

    # The rule: my own rule aka. own intuition
    hidden_Layers = [self._datadim - 1,
                        ((self._datadim - 1) + len(self._classes)) / 2,
                        len(self._classes)]

    classifier = tflearn.DNNClassifier(hidden_units=hidden_Layers,
                                        n_classes=len(self._classes),
                                        activation_fn=tf.nn.relu)

    classifier.learning_rate = self._learning_rate
    classifier.fit(self._traindata, self._train_label, steps=self._iterations)
{% endhighlight %}

I have used `ReLU` as the activation function, which can be `tanh`, `sigmoid`, `softmax`, etc. as required. Now that the model is trained it is time to measure it's performance.
 
{% highlight python %}
    score = metrics.accuracy_score(self._test_label, 
                                classifier.predict(self._testdata))
    print 'Accuracy on test dataset: %f' % score
    score = metrics.accuracy_score(self._train_label, 
                                classifier.predict(self._traindata))
    print 'Accuracy on training dataset: %f' % score
{% endhighlight %}

We take Scikit's help again to measure the model's accuracy.

## Summary
In this post, I have tried to walk you through a very basic categorical Deep Neural Network using TensorFlow, which was really a cakewalk. Well, if you learn something what isn't? I tried to explain important blocks of the code. Hope you find it useful.  

You may explore the source code [here](https://github.com/tsaqib/samples/tree/master/categorical_dnn).