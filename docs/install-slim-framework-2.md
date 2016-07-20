---

template:         article
reviewed:         2016-02-20
title:            Install Slim Framework 2
naviTitle:        Slim Framework
group:            Install_guides
dontList:         true
lead:             Slim is a PHP micro framework that helps you write simple web applications and APIs quickly. Learn how to install and tune Slim v2 on fortrabbit.

websiteLink:      http://www.slimframework.com/?utm_source=fortrabbit
websiteLinkText:  slimframework.com
category:         micro framework
image:            slim-logo.png
version:          2

otherVersionLinks:
    - install-slim-framework-3

tags:
    - php
    - install

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You will also need a local Slim framework installation. [Slim framework](http://www.slimframework.com/) is easy to setup thanks to Composer.

## Install

You can either use an existing or a new one. This guide explains you how to start locally from scratch:

```bash
# change into your projects directory
$ cd Projects

# clone your fortrabbit app
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}} YourApp

# change into the App directory
$ cd YourApp

# install slim via composer
$ composer require slim/slim "2.*"
```

Now just create your first hello world `index.php` file containing:

```php
<?php

include __DIR__. '/vendor/autoload.php';

$app = new \Slim\Slim();
$app->get('/hello/:name', function ($name) {
    echo "Hello, $name";
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
