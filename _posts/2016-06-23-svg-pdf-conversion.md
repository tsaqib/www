---
layout: post
title:  ".SVG to .PDF from OSX Command Line"
date:   2016-06-23 19:40:19 +0600
categories: Misc
permalink: /:slug
comments: true
---
If you happen to write a lot of LaTeX, file conversion such as .SVG to .PDF is tedious. The workflow is simply counter-productive. Here's how I have improved the experience a bit.  

### Background
I still use Microsoft Visio for day to day diagramming work, but recently I purchased Creatively Desktop which supposedly produce more beautiful diagrams without putting much effort. Visio is great for generating PDFs effortlessly, but Creatively unfortunately, is yet to support vector PDFs out of the box. However, it can produce .SVG files. If you are familiar with LaTeX typesetting system you know that it allows EPS and PDF files. Therefore, I needed a system to convert from .SVG to .PDF effortlessly. The bad news is, there's probably ain't one. 

### Inkscape works
You must install [Inkscape](https://inkscape.org/en/), which is kind of a bummer, especially on OS X, because Inkscape depends on [XQuartz](https://www.xquartz.org) to render its UI system. You must install XQuartz first and then log out and log back in to install Inkscape and once Inkscape runs for the first time, it often takes a while to properly initialize the system for it to use. The good news is that Inkscape supports command line parameters.

### The command line utility
I like to write tiny command line tools that save the day, for example, [Docker-Scripts](https://github.com/tsaqib/docker-scripts). I keep `utils.sh` file somewhere in my file system where I write all my tiny command line utilities and give it `chmod +x utils.sh` and put it into the ~/.bash_profile as this:

{% highlight bash %}
source ~/scripts/utils.sh
{% endhighlight %}

And now write a function in `utils.sh`:

{% highlight bash %}
svgpdf() {
    "/Applications/Inkscape.app/Contents/Resources/bin/Inkscape" "$PWD"/$1 -A="$PWD"/$1.pdf --without-gui
}
{% endhighlight %}

Now you may change directory to your graphics directory and execute command such as `svgpdf sample.svg` and it will create a sample.svg.pdf file without opening any GUI or sort. This command is saving a lot of my time lately.