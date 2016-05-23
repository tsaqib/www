---
layout: post
title:  "Jekyll + Github Combo Awesomeness"
date:   2016-05-22 22:56:23 +0600
categories: Blogging
permalink: /:slug
comments: true
---
I'm guilty of changing my blogging platform pretty much every year, but I have settled down with this setup, and I think it's for the right reasons and an efficient decision. 

### Why not hosting services?
I previously blogged at [MSDN](https://blogs.msdn.com/saqib/), [Wordpress.org](https://tanzimsaqib.wordpress.com/), [ASP.NET Weblogs](http://weblogs.asp.net/tanzimsaqib), Blogger [I don't even remember the URL now], [Geeks with Blogs](http://www.geekswithblogs.net/Saqib/), my own personal website which I lost many times for being casual, and where not. Oh, I wrote my own static site generator but found it to be not the best use of my time to maintain it according to my ever-growing demands. I love self-hosted Wordpress, too, but as I get more busy everyday it is not possible to look after yet another hosting service and its performance along with Azure's, which is my primary cloud for work and fun.

### Why static site generators?
Because, I do not think I need to own all the comments people might make on my posts, and the posts themselves are really static output. As I am embracing serverless architecture more and more, I am incorporating that principle for my personal projects, too. In that direction, [disquss](https://disqus.com/) is a fine discussion platform hosted on their server, and embeddable into my blog. Therefore, why would anyone store a blog/website content into a database? Database queries are the root of all performance problems in such a scenario. 

### Why not Azure?
Why didn't I move to Azure/any other cloud vendor for a Wordpress hosting you ask? I already have a lot of VMs, I just didn't want to add one just for hosting, and that too requires maintenance and management. Why not as a Azure's PaaS/Web App Service? Because, Azure's partner ClearDB offers MySQL hosting for the Web App Service, but it's just too awfully  slow, that it's unusable. 


### Why not blogging platforms?
Why not Medium? I think it's an excellent platform, and I will use that for other cases, but for my personal website, I'd rather require a bit more control over the markups that are going to representing me.     

### Jekyll + Github awesomeness
My own static site generator written in Python, too provides the ability to write blog posts in markdown, but [Jekyll](https://jekyllrb.com) was done way better. Developers and the community behind it spent fair amount of time and put enough effort to make it the leading static site generator, and I have had fun so far. One caveat though, I would miss spell checking as I am using [Microsoft Visual Studio Code](https://code.visualstudio.com/) to compose this blog post in markdown.

Now about, Github. Apart from all the goodness of source control and community engagment brought into your project via such a truly amazing platform, they offer [Github Pages](https://pages.github.com/), their own way to publish your static website to a Github URL, which they also allow you to [map your own domain](https://help.github.com/articles/using-a-custom-domain-with-github-pages/) to it for free. 

All for free. 

Deployment is done by pushing the codebase to git, which is exactly why I am such a big fan and user of Azure Web App Service. However, the blazing performance of Github threw everybody else out of the competition. It loads way faster than Azure and everybody else.       