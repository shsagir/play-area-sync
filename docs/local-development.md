---

template:     article
reviewed:     2016-08-03
title:        Local development
naviTitle:    Local development
lead:         Why and how to set a local PHP development environment.
group:        getting_started

keywords:
     - LAMP
     - vhosts
     - localhost
     - 127.0.0.1

tags:
    - beginner

seeAlsoLinks:
    - app
    - dashboard
    - multi-staging


---

## Problem

fortrabbit [Apps](app) are made for production (live) or staging (review). They are not well suited for online development. The deployment is fast but still takes too long when developing rapid changing aspects of your App. Also, it is never a good idea to have your live application display errors and debugging informations.


## Solution

Set up a local PHP development environment so that your Apps can run on your local machine as well as remote on the fortrabbit. Our workflow is to run every live App locally under `{{app-name}}.dev`, while `{{app-name}}.frb.io` runs on fortrabbit, of course. To make this happen you need to have some open source software running:

* **Apache** – the webserver
* **MySQL** – the database - [see dedicated article](/mysql) on remote usage
* **PHP** – the server side scripting language
* **Git** – the version control - [see dedicated article](/git)
* **Composer** – the dependency manager for PHP - [see below](#toc-composer)


## Setup

There are multiple ways to get your local development stack up and running: You can use a prepared solution stack, bundled up and controlled by a user friendly GUI (best for beginners), you can set up what you need manually or you can box everything neatly into a [virtual machine](#toc-virtualization).

### GUIs

These solution stacks are easy to handle through a graphical interface and they interfere with the rest of your system as little as possible. The most commonly used here are:

* [XAAMP](https://www.apachefriends.org/index.html) GUI for Windows, Mac OS, Linux
* [MAMP](https://www.mamp.info/) GUI for Mac OS and Windows

Those don't include [Git](git) and [Composer](#toc-composer).

### Manual setup

Well, it's all manual so you should attempt it only if you know what you are doing. To get you started, we looked up some helpful guides:

* [Manually install Apache, PHP, MySQL & Composer on Windows](http://heiswayi.github.io/http://heiswayi.github.io/manually-install-apache-php-mysql-composer-on-windows.html.html)
* [Superuser: Manually install Apache PHP & MySQL on Windows](http://superuser.com/questions/748117/how-to-manually-install-apache-php-and-mysql-on-windows)
* [PHP Dev on Mac OS X 10.10 (PHP, MySQL, Apache, Composer)](https://gist.github.com/suvozit/6dda7971e240f0a3f282)

### Virtualization

Utilizing virtualization for development allows you better replication of your local setup across your team. Also having a everything stashed into a virtual machine very much nullifies any interference with your local host system. In a sentence: It keeps things clean.

There are two primary virtualization approaches for development: [Desktop virtualization](https://en.wikipedia.org/wiki/Desktop_virtualization), for example [VirtualBox](https://www.virtualbox.org/) or [VMware](http://www.vmware.com/) or [container virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization), which means primarily [Docker](http://www.docker.com/) these days. There are a lot of fundamental difference between both approaches, but when considering development it boils down to: Desktop virtualization is easier to setup but eats up more resources - container virtualization is harder to setup, but once done easier to use and is very economical in terms of resource usage.

Many developers today use VirtualBox controlled by [Vagrant](https://www.vagrantup.com/), which helps a lot when setting up reproducible development environments from scratch - or rather from config file. Many Laravel developers use [Homestead](https://laravel.com/docs/5.2/homestead), which builds upon Vagrant and simplifies Laravel development even further.

Have a look at [Docker for Mac](https://docs.docker.com/docker-for-mac/) or [… for Windows](https://docs.docker.com/docker-for-windows/), it's on the rise.

### Other solutions

The Laravel guys came up with a easy-to-use solution just for Laravel developers on Mac OS called [Valvet](https://laravel.com/docs/5.2/valet). If you fit that description then it's definitely worth looking into.

## Considerations

What else you need to think about when setting up your local development environment.

### Composer

You will most likely need Composer, as it is the one and only PHP dependency manager (besides PEAR, but who uses that anymore?):

* **[Offical Composer install guides](https://getcomposer.org/download/)**
* [Composer on Mac OS for MAMP](https://gist.github.com/kkirsche/5710272)

### Environment detection

Our code examples in the [install guides](/#install-guides) always include checks to detect if the App is running locally or on fortrabbit. You should consider this when developing your code - or rather setting up your configuration. For example: MySQL will need different credentials locally then on fortrabbit.

### Multi-staging

You might want to have a dedicated remote testing environment with pubic access? Head over to our advanced [multi-staging article](multi-staging).


### Apache instead of NginX

Some might prefer a NginX web-server over Apache as it is easier to configure. Mind that fortrabbit is using Apache, some features might not be compatible. You will find `.htaccess` examples in these articles which only work with Apache.


### Virtual hosts

You might set up multiple vhosts - not just `localhost` or `127.0.0.1` - to serve multiple websites from one machine.
