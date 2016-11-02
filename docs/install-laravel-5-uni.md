---

template:         article
reviewed:         2016-08-29
title:            Install Laravel 5
naviTitle:        Laravel
lead:             Laravel is the most PHPopular framework. Learn how to install and tune Laravel 5 on fortrabbit.
group:            Install_guides

websiteLink:      http://laravel.com?utm_source=fortrabbit
websiteLinkText:  laravel.com
category:         framework
image:            laravel-mark.png
version:          5.1
stack:            uni
oldLink:          install-laravel-5-old
proLink:          install-laravel-5-pro

keywords:
    - php
    - install
    - laravel

---


## Get ready

We assume you've already created a New App and chose Laravel in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard). You should also have a [PHP development environment](/local-development) running on your local machine.

If you haven't choosen Laravel stack, please set the following:

### Root path

Go to the Dashboard and [set the root path](/app#toc-set-a-custom-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>

### Application key

Go to the Dashboard and add an application key as a new as an [environment variable](/env-vars) named `APP_KEY` to your App. You can use this:

```osterei32
APP_KEY=ClickToGenerate
```

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

## Quick start

Following the fastest way to start with a fresh installation. Please scroll below for [migrating an existing Laravel](#toc-advanced-setup-and-migration).

```bash
# 1. Use Composer to create a local Laravel project named like your App
$ composer create-project laravel/laravel --prefer-dist {{app-name}}
# this installs Laravel locally and will take a while

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init .

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push changes to fortrabbit
$ git push -u fortrabbit master
# this will install Laravel on remote and take another while
# the next deployments will be much faster
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, when this is done, you can visit your App URL in the browser to see the Laravel welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Advanced setup and migration

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing Laravel installation or if you would like to setup Laravel so that you can run in a local development environment as well as in your fortrabbit App:

### Setup Git

If you used the above Quick start guide, Git is already setup and you can safely skip this topic.

Unless your local Laravel installation is already under Git version control, you need to enable it now:

```bash
# Change to your local directory, where you Laravel is
$ cd {{app-name}}

# Initialize a local Git repo
$ git init .

# Add all files
$ git add -A

# Commit files for the first time
$ git commit -m 'Initial'
```

Now you need to connect your local installation with your App's remote:

```bash
# Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# Push changes to fortrabbit
$ git push -u fortrabbit master
# this will install Laravel on remote and take another while
# the next deployments will be much faster
```

### MySQL configuration

If you have choosen Laravel in the stack chooser when creating your App, we will automatically populate the "right" environment variables for Laravel's MySQL connection for you. So, you don't need to change a thing, just keep `config/database.php` as is:

```php
<?php
return [
    // keep above
    'connections'   => [
        // keep above
        'mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', 'localhost'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => null,
        ],
        // keep below
    ],
    // keep below
];
```

If you have not chosen the Laravel stack when creating the App you need to use different ENV vars:

* The ENV var `DB_HOST` becomes the ENV var `MYSQL_HOST`
* The ENV var `DB_PORT` becomes static `3306`
* The ENV var `DB_DATABASE` becomes the ENV var `MYSQL_DATABASE`
* The ENV var `DB_USERNAME` becomes the ENV var `MYSQL_USER`
* The ENV var `DB_PASSWORD` becomes the ENV var `MYSQL_PASSWORD`

To make sure you still can use your Laravel installation in your local environment, you can "double wrap" the ENV var access like so:

```php
<?php
return [
    // keep above
    'connections'   => [
        // keep above
        'mysql' => [
            'driver' => 'mysql',
            'host' => env('MYSQL_HOST', env('DB_HOST', 'localhost')),
            'port' => env('DB_PORT', '3306'),
            'database' => env('MYSQL_DATABASE', env('DB_DATABASE', 'forge')),
            'username' => env('MYSQL_USER', env('DB_USERNAME', 'forge')),
            'password' => env('MYSQL_PASSWORD', env('DB_PASSWORD', '')),
            'charset' => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => null,
        ],
        // keep below
    ],
    // keep below
];
```


### Database migration

This is about migrating your existing MySQL data set, not running `artisan migrate`, which is described [below](#toc-migrate-amp-other-artisan-commands).

There are various use cases to export and import the database:

1. Export the database from your old webhosting
2. Export your local database to import it to the fortrabbit database
3. Export the remote database from fortrabbit to bring your local installation up-to-date

#### Migrating the database with a GUI

Read on in the [MySQL Article: Export & import > Using MySQL Workbench (GUI)](mysql-uni#toc-using-mysql-workbench-gui-).

#### Migrating the database in the terminal

Read on in the [MySQL Article: Export & import > Using the terminal](mysql-uni#toc-using-the-terminal).

#### MySQL access from local

Please see the [MySQL article](mysql-uni#toc-access-mysql-from-local) on how to access the database remotely from your computer.



### Migrate & other artisan commands

You can [execute remote commands via SSH](/remote-ssh-execution), for example:

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan migrate
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan migrate:rollack
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan tinker
```

If `APP_ENV` is set to `production` - which is the default - then Laravel expects `--force` for migrate commands.



### Logging

You can access your logs via SSH or SFTP. Larval, per default, stores it's log files in `storage/logs`. You can download them via SFTP from that folder. If you need to "tail" your logs live, you can:

```bash
# login via SSH
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com

# tail the logs (they contain the current date, per default)
$ tail -f storage/logs/laravel-2016-10-27.log
```

#### Setting time zone in Laravel

As Eloquent uses `PDO`, you can use the `PDO::MYSQL_ATTR_INIT_COMMAND` option. Extend your `mysql` configuration array in `app/config/database.php` or your specific environment `database.php` file:

```php
return [
    // other code …
    'mysql' => [
        // other code …
        'options'   => [
            \PDO::MYSQL_ATTR_INIT_COMMAND => 'SET time_zone = \'+02:00\''
        ]
    ],
    // other code …
];
```


### Queues & Redis

Laravel supports multiple queue and cache drivers. Please check out the [Laravel 5 Profession article](install-laravel-5-pro#toc-queue) on how to integrate those.

#### Using envoy

Easy. Here is an `Envoy.blade.php` example:

```php
@servers(['fr' => '{{ssh-user}}@deploy.{{region}}.frbit.com'])

@task('ls', ['on' => 'fr'])
    ls -lha
@endtask

@task('migrate', ['on' => 'fr'])
    php artisan migrate
@endtask
```

Then execute locally:

```bash
$ envoy run ls
$ envoy run migrate
```


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but Laravel provides a API over the popular SwiftMailer library. The mail configuration file is `app/config/mail.php`, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global form address for all messages delivered by the library.


## Add an existing project

You can also push your existing Laravel installation to fortrabbit. When you already using Git, you can add fortrabbit as an additional remote, like described [above](#toc-install) under point 6. When moving from another host to fortrabbit, please also read our [migration guide](/migrating) as well.
