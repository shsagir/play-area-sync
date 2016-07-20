---

template:         article
reviewed:         2016-02-20
title:            Install Slim Framework 3
naviTitle:        Slim Framework
group:            Install_guides
lead:             Slim is a PHP micro framework that helps you write simple web applications and APIs quickly. Learn how to install and tune Slim v3 on fortrabbit.

websiteLink:      http://www.slimframework.com/?utm_source=fortrabbit
websiteLinkText:  slimframework.com
category:         micro framework
image:            slim-logo.png
version:          3

otherVersionLinks:
    - install-slim-framework-2

tags:
    - php
    - install

---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You also need a local Slim framework installation. Slim framework is extremely easy to setup thanks to our Composer support. You can either use an existing or a new one.


## Install

This guide explains you how to start locally from scratch:

```bash
# change into your projects directory
$ cd Projects

# clone your fortrabbit app
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}} YourApp

# change into the App directory
$ cd YourApp

# install slim via composer (currently unstalble, hence the @dev)
$ composer require slim/slim "3.*@dev"
```

Now just create your first hello world `index.php` file containing:

```php
<?php

include __DIR__. '/vendor/autoload.php';

$app = new \Slim\App;
$app->get('/hello/{name}', function ($request, $response, $args) {
    return $response->write("Hello " . $args['name']);
});
$app->run();
```

You also want to create an `.htaccess` file, so you have nice URLs:

```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [QSA,L]
```

Create a `.gitignore` file to exclude the `vendor/` folder:

```bash
$ echo vendor/ > .gitignore
```

Now deploy everything and you are done:

```bash
# add everything to git
$ git add -A

# create an initial commit
$ git ci -am 'Initial'

# push and remember the remote
$ git push -u origin master
```

That's it. You can see the hello-world in the browser: `http://{{app-name}}.frb.io/hello/world`.
