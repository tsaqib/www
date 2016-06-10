---
layout: post
title:  "Odoo 9.0 Up & Running"
date:   2016-06-09 21:21:23 +0600
categories: Odoo
permalink: /:slug
comments: true
---
I have recently setup Odoo 9.0 in fair amount of places, including inside Linux and Windows boxes and VMs on the Cloud, Docker, as well as on my local OSX computer. There were 1-2 tricks that I would like to keep documented lest I should forget in the future. 

### Odoo Ingredient List
Odoo was written in Python and it's based on PostgreSQL. Its UI is transformed using 'less' processor which can be installed using Node's NPM. So, first things first, make sure Python 2.7.9+ and Node.js are installed, but beware of the Python 3.X versions.

Here's the list of binaries that you may need to download and install:

- [Python 2.7.9+](https://www.python.org/downloads/)
- [Node.js](https://nodejs.org/en/download/)
- [Odoo 9 source-code](https://github.com/odoo/odoo/archive/9.0.zip) to be unzipped assuming into odoo9 directory
- [PIP installer](https://bootstrap.pypa.io/get-pip.py) (not a binary, rather a script)
- Install [PostgreSQL](https://www.postgresql.org/download/) 
- Optionally, [pgAdmin](https://www.pgadmin.org/download/)

### Installation Sequence
Now you may close the browser and focus on the Terminal:

- Command + Space to open the Finder, and look for SQL Shell (psql)
- Create an odoo user for Odoo to connect to the database:
{% highlight sql %}
CREATE USER odoouser WITH SUPERUSER CREATEDB CREATEROLE PASSWORD 'odoouser';
\q
{% endhighlight %}

Now that the database is setup, let us create a Python environment for Odoo, and install its dependencies.
{% highlight bash %}
export PATH=$PATH:/installation-path/postgresql/bin # example: /Library/PostgreSQL/9.5/bin
sudo python get-pip.py 
pip install -U pip setup tools virtualenv
brew install --upgrade openssl
brew unlink openssl && brew link openssl --force
virtualenv odoo9
cd odoo9    # assuming Odoo9 source code exists inside
source bin/activate
sudo -H pip install -r requirements.txt --upgrade
sudo npm install -g less less-plugin-clean-css 
python odoo.py -r odoouser -w odoouser --db_host localhost
{% endhighlight %}

That's it. Odoo 9.0 should be running at http://127.0.0.1:8069 now.

Note: Always make sure that you `source bin/activate` before running Odoo, otherwise, it will try to run outside of the environment that we have created for it.