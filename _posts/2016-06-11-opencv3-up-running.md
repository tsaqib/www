---
layout: post
title:  "OpenCV 3.0 Up & Running"
date:   2016-06-11 20:15:20 +0600
categories: OpenCV
permalink: /:slug
comments: true
---
Getting [OpenCV 3X](http://opencv.org) is a huge pain on OS X. I have crawled through internet and struggled a lot to make it work on my Mac. Some of the painfully long, yet didn't work for me ways [1](http://www.learnopencv.com/install-opencv-3-on-yosemite-osx-10-10-x/) and [2](http://www.pyimagesearch.com/2015/06/15/install-opencv-3-0-and-python-2-7-on-osx/). Here’s my shortcut. 

First off, make sure you have brew and Xcode essentials installed: 

{% highlight bash %}
xcode-select --install
{% endhighlight %}

Install OpenCV 3X with the support for both Python (default) and Java, and also make sure to install OpenCV contrib project alongside, which would bring some more power to your OpenCV library: 

{% highlight bash %}
brew install opencv3 --with-contrib --with-java
{% endhighlight %}

As soon as it is installed, make sure it is accessible on the PYTHONPATH, by appending the following to `~/.bash_profile` file. 

{% highlight bash %}
export PYTHONPATH=$PYTHONPATH:/usr/local/Cellar/opencv3/3.1.0_3/lib/python2.7/site-packages
{% endhighlight %}

Once you have done that, `source ~/.bash_profile` to reload it into the memory or simply open a new Tab on the Terminal.

Uninstall and install latest Numpy:

{% highlight bash %}
sudo -H pip uninstall numpy
sudo -H pip install numpy
{% endhighlight %}

Cool! Now that OpenCV is successfully installed and configured, go ahead and check it out by yourself:

{% highlight bash %}
python
>>> import cv2
>>> cv2.__version__
'3.1.0'
{% endhighlight %}

Hope this will help me in the future should I reinstall OS X.