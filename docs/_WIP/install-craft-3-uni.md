---

template:         article
reviewed:         2017-02-01
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft CMS
lead:             Work in progress
group:            Install_guides
stack:            uni
proLink:          install-craft-2-pro

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

## Deployment

Before pushing the first time to fortrabbit, make sure to `git add composer.lock -f`.

