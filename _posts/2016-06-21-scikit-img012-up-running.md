---
layout: post
title:  "Scikit Image 0.12 Up & Running"
date:   2016-06-21 20:00:29 +0600
categories: Scikit Image
permalink: /:slug
comments: true
---
I previously [logged](http://tanzimsaqib.com/opencv3-up-running) how OpenCV 3.0 could be installed on OS X. Fortunately, setting up Scikit Image 0.12 is easier than that. I'm keeping it on the record here.  

First off, change the directory to your source directory, and create a Virtual Environment: 

{% highlight bash %}
virtualenv .
{% endhighlight %}

Create `fxpy` inside bin directory of the Virtual Environment with the following content:

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

 Wait, why do we need this? Because if we don't, we will end up with the following error message which is quite self-explanatory:

    RuntimeError: Python is not installed as a framework. The Mac OS X backend 
    will not be able to function correctly if Python is not installed as a 
    framework. See the Python documentation for more information on installing 
    Python as a framework on Mac OS X. Please either reinstall Python as a 
    framework, or try one of the other backends. If you are Working with 
    Matplotlib in a virtual enviroment see 'Working with Matplotlib in Virtual 
    environments' in the Matplotlib FAQ.

Needless to say, we have skipped that message. In the end, forget not to change its permission and activate the Virtual Environment.

{% highlight bash %}
chmod +x bin/fxpy
source bin/activate
{% endhighlight %}

Now go ahead and execute your program by `fxpy main.py`. 