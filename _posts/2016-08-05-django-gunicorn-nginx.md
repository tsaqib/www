---
layout: post
title:  "Gunicorn and Nginx: Django Deployment Combination"
date:   2016-08-05 02:00:10 +0600
categories: Python
permalink: /:slug
comments: true
---
[Django](https://www.djangoproject.com/) + [Gunicorn](http://gunicorn.org/) + [Nginx](https://nginx.org) is probably the most used combination. Here is a basic tie up, with which you may turn your http://localhost:8080 dev. codebase to a http://production_ip like deployment. 

## Background
Django provides with an easy access to your application in development time by `./manage.py runserver`, however that is not how it should be run on server, because that tiny server is no way ready for a production-like deployment. For example, single-thread/worker model is definitely a no-no. That's where Gunicorn comes in. It acts like a middle-man between a real server like nginx, and the Python-driven server like that which is embedded into Django. Needless to mention that Nginx is one of the most popular web servers, especially used as a reverse proxy.

## Setting up the VM
Assuming that you are configuring a fresh Ubuntu VM. You need to install nginx, some build essentials and Python tools including but not limited to pip, virtualenv, etc. 

{% highlight bash %}
sudo apt-get update
sudo apt-get install nginx python-pip python-dev build-essential libpq-dev
sudo pip install --upgrade pip 
sudo pip install --upgrade virtualenv 
sudo apt-get install git
clone https://github.com/user/my_project.git
cd my_project
virtualenv .
source bin/activate
pip install -r requirements.txt
./manage.py runserver 0.0.0.0:8080
{% endhighlight %}

You should then clone/download the source code that you want to setup, and also go ahead and install all the packages. Give the codebase a spin by running the embedded server at 8080, and hitting the browser with http://localhost:8080.   

## Installing and configuring Gunicorn
Begin with installing Gunicorn to the virtual environment for the project. Please note that the `my_project` is the project name when you issued a command similar to this `django-admin startproject my_project` to create the project itself in the beginning. `my_project.wsgi` or similar file does not need to be present into the directory from where you are issuing the following commands, but make sure that you are just inside the directory where you created the project at the first place. 

{% highlight bash %}
pip install gunicorn
gunicorn my_project.wsgi:application
sudo vim gunicorn_start.sh
{% endhighlight %}

Create a bash file called `gunicorn_start.sh` with the following contents:

{% highlight bash %}
#!/bin/bash
cd /home/user/my_project
source bin/activate
exec gunicorn my_project.wsgi:application --name my_project --workers 4 -b 0.0.0.0:8080
{% endhighlight %}

This essentially means that when this bash file will be executed it will launch your web application with the help of Gunicorn and it will have four worker threads to serve requests asynchronously at 8080 port.

{% highlight bash %}
chmod +x gunicorn_start.sh
{% endhighlight %}

Forget not to permit the file with an executable privilege. 

## Startup Script

Now we need a startup script so that our Gunicorn script gets activated on server reboot. Lets go ahead and create a new bash script by issuing `sudo vim /etc/init.d/gunicorn-my-project` and save with the following contents: 

{% highlight bash %}
exec /home/user/my_project/gunicorn_start.sh
{% endhighlight %}

Now provide with permissions and set as one of the startup scripts that the OS will load on reboot:

{% highlight bash %}
sudo chmod 755 /etc/init.d/gunicorn-my-project
sudo chown ubuntu: /etc/init.d/gunicorn-my-project
sudo update-rc.d gunicorn-my-project defaults
{% endhighlight %}

## Setting up Nginx

The final step is to glue Gunicorn to the Nginx instance. Create a new file with the following contents `sudo vim /etc/nginx/sites-available/my-project`:

{% highlight bash %}
server {
    listen 80;
    server_name localhost;

    access_log /home/user/my_project/access.log;
    error_log /home/user/my_project/error.log;

    location /static/ {
        # do not put trailing /static/ please
        root /home/user/my_project/; 
    }

    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}
{% endhighlight %}

Create a link to this file into `sites-enabled` as well.

{% highlight bash %}
sudo ln -s /etc/nginx/sites-available/my-project /etc/nginx/sites-enabled/my-project

# only if needed, your site isn't being served at port 80
sudo rm /etc/nginx/sites-enabled/default    
sudo service nginx restart
{% endhighlight %}

Congratulations! Your Django app is running at http://production_ip at port 80.