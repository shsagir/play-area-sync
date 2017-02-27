---

template:         article
reviewed:         2017-02-27
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft CMS
lead:             Work in progress
group:            Install_guides
stack:            uni
dontList:         true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0
group:            Install_guides

keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

---


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

```plain
# Mapping
DB_DATABASE=${MYSQL_DATABASE}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
DB_PASSWORD=${MYSQL_PASSWORD}

# Driver
DB_DRIVER=mysql
```

<div markdown="1" data-user="known">
[Add ENV vars to your App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

## Git Deployment

Before pushing the first time to fortrabbit, make sure to `git add composer.lock -f`.


## Updating Craft

The lastest beta is just a `composer update` away. The output looks something like this: 

```plain
Loading composer repositories with package information
Updating dependencies (including require-dev)

 [...]
 
  - Removing yiisoft/yii2-debug (2.0.7)
  - Installing yiisoft/yii2-debug (2.0.8)
    Downloading: 100%

  - Removing craftcms/cms (3.0.0-beta.3)
  - Installing craftcms/cms (3.0.0-beta.5)
    Downloading: 100%
```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App's `master` branch. The packages got installed during the deployment. As you can see it takes just a few seconds:

![commit and push](https://static.frbit.name/img/help/craft3-composer-update.gif)





## Using the craft cli

SSH in an type `php craft`. The `php` prefix is required for now.



