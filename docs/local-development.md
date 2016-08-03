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

fortrabbit [Apps](app) are for production (live) or staging (review). They are not well suited for live online development, as the deployment is fast but still takes too long when debugging code. Also, you might not want your live version display errors and debugging informations.


## Solution

Set up a local PHP development environment so that your Apps can run on your local machine as well as on the fortrabbit remote. You want the browser address `{{app-name}}.dev` to be served from your local machine, while `{{app-name}}.frb.io` runs on fortrabbit. For this you'll need to have some open source software running:

* **Apache** – the webserver
* **MySQL** - the database - [see dedicated article](/mysql) on remote usage
* **PHP** - the server side scripting language
* **Git** - the version control - [see dedicated article](/git)
* **Composer** - the dependency manager for PHP - [see below](#toc-composer)


## Setup

There are multiple ways to this. You can use a GUI (best suited for beginners), do it manually or even work with [visualization](#toc-visualization).

* [XAAMP](https://www.apachefriends.org/index.html) GUI for Windows, macOS, Linux
* [MAMP](https://www.mamp.info/) GUI for macOS and Windows
* [Valvet](https://laravel.com/docs/5.2/valet)(macOS only) and [Homestead](https://laravel.com/docs/5.2/homestead)(virtual) for [Laravel](install-laravel) 
* [Manually install Apache, PHP, MySQL & Composer on Windows](http://heiswayi.github.io/http://heiswayi.github.io/manually-install-apache-php-mysql-composer-on-windows.html.html)
* [Superuser: Manually install Apache PHP & MySQL on Windows](http://superuser.com/questions/748117/how-to-manually-install-apache-php-and-mysql-on-windows)
* [PHP Dev on Mac OS X 10.10 (PHP, MySQL, Apache, Composer)](https://gist.github.com/suvozit/6dda7971e240f0a3f282)


- - -

### Composer

Environments differ. A Mac with MAMP for example is for sure a bit different to the LINUX fortrabbit environment. Virtualization is a way to make your local environment behave more like the one in production. The PHP dependency manager Composer also helps with this like so:

#### Installing Composer

* **[Offical Composer install guides](https://getcomposer.org/download/)**
* [Composer on macOS for MAMP](https://gist.github.com/kkirsche/5710272)


### Environment detection

Our code examples in the [install guides](/#install-guides) always include checks to detect if the App is running locally or on fortrabbit. For MySQL you will need to enter your local access credentials. 



### Visualization

Using virtualization for development is a more advanced approach — have your local development in a virtualized container, so that it not interferes with your local host system and so that is is more portable. [Vagrant](https://www.vagrantup.com/) is a standard tool. Docker is on the rise to replace that.


### Multi-staging

You might want to have a dedicated remote testing environment with pubic access? Head over to our advanced [multi-staging article](multi-staging).



