---
layout: post
title:  "OpenCV 3.0 Up & Running"
date:   2016-06-11 20:15:20 +0600
categories: OpenCV
permalink: /:slug
comments: true
---
Running [OpenCV 3X](http://opencv.org) is a huge pain on OS X. I have crawled through the internet and struggled a lot to make it work on my Mac. Some of the painfully long, yet didn't work for me ways [1](http://www.learnopencv.com/install-opencv-3-on-yosemite-osx-10-10-x/) and [2](http://www.pyimagesearch.com/2015/06/15/install-opencv-3-0-and-python-2-7-on-osx/). Here’s my shortcut. 

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

Alright, now it seems all OK, until it is not. If you choose to use Matplotlib which uses UI toolkits which eventually use framework version of the Python of OS X, may cause trouble. Why would you need Matplotlib? Because without it, it wouldn't be easy for you to output the image processing changes that you are going to make. The best way, in my opinion, is to run it in a virtual environment, and that's not going to solve the problem straight way either. Matplotlib's [FAQ](http://matplotlib.org/faq/virtualenv_faq.html) helps to figure it out.  

Create `fxpy` inside bin directory of the virtual environment with the following content:

{% highlight bash %}
#!/bin/bash
# what real Python executable to use
PYVER=2.7
PATHTOPYTHON=/usr/bin/ # `which python` and find out
PYTHON=${PATHTOPYTHON}python${PYVER}

ENV=`$PYTHON -c "import os; print os.path.abspath(os.path.join(os.path.dirname(\"$0\"), '..'))"`

# now run Python with the virtualenv set as Python's HOME
export PYTHONHOME=$ENV
exec $PYTHON "$@"
{% endhighlight %}

In the end, forget not to change its permission `chmod +x bin/fxpy`. Wait, why do we need this? Because if we don't, we will end up with the following error message which is quite self-explanatory:

    RuntimeError: Python is not installed as a framework. The Mac OS X backend 
    will not be able to function correctly if Python is not installed as a 
    framework. See the Python documentation for more information on installing 
    Python as a framework on Mac OS X. Please either reinstall Python as a 
    framework, or try one of the other backends. If you are Working with 
    Matplotlib in a virtual enviroment see 'Working with Matplotlib in Virtual 
    environments' in the Matplotlib FAQ.

Needless to say, we have skipped that message. Lets go ahead and execute the program 
{% highlight bash %}
source bin/activate
fxpy main.py
{% endhighlight %}

Hope this will help me in the future should I reinstall OS X.