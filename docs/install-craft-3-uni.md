---
template:         article
reviewed:         2018-01-30
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft 3 Beta
lead:             Note that this install guide if for the Craft 3 BETA version.
group:            Install_guides
stack:            uni
dontList:         false
workInProgress:   true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.0-RC7
group:            Install_guides

keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

---


## Intro

We are super excited about Craft 3! However, if you plan to launch new Craft sites within the next weeks you should go with the current stable version. [Here is the install guide for Craft 2](/install-craft-2-uni). If you are curious about the future this is the place to be.


## Get ready

We assume you've already created an App and chose Craft CMS in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard).

### Root path

Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **web**, the default in Craft 2 was **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


## Quick start

Follow the [offical Craft 3 install guide](https://github.com/craftcms/docs/blob/master/en/installation.md) and use `composer create-project` to run Craft locally first. 
Leave the `config/db.php` untouched, but add this ENV vars to your App. It's a mapping from our default ENV vars to the names Craft expects. 

```osterei32
# Crypto key
SECURITY_KEY=ClickToGenerate

# The environment Craft is currently running in dev, staging, production, etc.
ENVIRONMENT=dev

# DB Mapping (don't use actual values)
DB_DATABASE=${MYSQL_DATABASE}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
DB_PASSWORD=${MYSQL_PASSWORD}

# DB Driver
DB_DRIVER=mysql
```

<div markdown="1" data-user="known">
[Add ENV vars to your App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

## Git Deployment

Before pushing the first time to fortrabbit, make sure to `git add composer.lock -f`.


## Updating Craft

The lastest beta is just a `composer update` away. When you run this command in the terminal locally, the output looks something like this: 

```plain
Loading composer repositories with package information
Updating dependencies (including require-dev)

 [...]
 
  - Removing yiisoft/yii2-debug (2.0.7)
  - Installing yiisoft/yii2-debug (2.0.11)
    Downloading: 100%

  - Removing craftcms/cms (3.0.0-beta.26)
  - Installing craftcms/cms (3.0.0-beta.29)
    Downloading: 100%
```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App's `master` branch. The packages got installed during the deployment. As you can see it takes just a few seconds:

![commit and push](https://static.frbit.name/img/help/craft3-composer-update.gif)

That's it. 


## Using the craft cli

Craft 3 ships with a Yii based cli tool. To use the cli remotely SSH in an type `php craft`. This will give you a list of all available commands.

```
This is Yii version 2.0.12.

The following commands are available:

- cache                       Allows you to flush cache.
    cache/flush               Flushes given cache components.
    cache/flush-all           Flushes all caches registered in the system.
    cache/flush-schema        Clears DB schema cache for a given connection component.
    cache/index (default)     Lists the caches that can be flushed.

- help                        Provides help information about console commands.
    help/index (default)      Displays available commands or the detailed information.
    help/list                 List all available controllers and actions in machine readable format.
    help/list-action-options  List all available options for the $action in machine readable format.
    help/usage                Displays usage information for $action

- install                     Craft CMS CLI installer.
    install/index (default)   Runs the install migration

- migrate                     Manages Craft and plugin migrations.
    migrate/create            @inheritdoc
    migrate/down              Downgrades the application by reverting old migrations.
    migrate/history           Displays the migration history.
    migrate/mark              Modifies the migration history to the specified version.
    migrate/new               Displays the un-applied new migrations.
    migrate/redo              Redoes the last few migrations.
    migrate/to                Upgrades or downgrades till the specified version.
    migrate/up (default)      Upgrades the application by applying new migrations.
       

- queue                       Manages application db-queue.
    queue/exec                Executes a job.
    queue/info (default)      Info about queue status.
    queue/listen              Listens db-queue and runs new jobs.
    queue/run                 Runs all jobs from db-queue.

```
