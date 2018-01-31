---
template:         article
reviewed:         2018-02-01
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft 3 Beta/RC
lead:             Note that this install guide is for the Craft 3 (pre-stable) version.
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

Please mind, the official Craft 3 stable release is expected for 2018/04/04. If you don't feel confident using a pre-release [head over to the Craft 2.6 install guide](/install-craft-2-uni). 

## Get ready

We assume you've already created an App and chose Craft CMS in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard).

### Root path

If you created your App after 2018/02/01 the root path is already set to **web** for Craft Apps. Go to the Dashboard to verify the current settings and [modify the root path](/app#toc-root-path) of your App's domains if needed. 

<div markdown="1" data-user="known">
[Change the root path for App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


## Quick start

Follow the [offical Craft 3 install guide](https://github.com/craftcms/docs/blob/master/en/installation.md) and use `composer create-project` to run Craft locally first.

Craft 3 uses ENV vars to access environment specific settings. In your local environment these settings are stored in a `.env` file. On fortrabbit you manage ENV vars the Dashboard using the .env syntax.

Your ENV vars in the Dashboard are pre-configured already. You may need to change the value of `ENVIRONMENT` to match the name in your config files.

```osterei32
# Crypto key
SECURITY_KEY=ClickToGenerate

# The environment Craft is currently running in dev, staging, production, etc.
ENVIRONMENT=production

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

Here is an example of a configuration group - `config/general.php` is structured like this. The `ENVIRONMENT` you've defined earlier, maps with the array key `production`, or `dev` which is the default for your local setup.

```
<?php
return [
	// Global settings
    '*' => [
        'cpTrigger' => 'brewery',
    ],
    // ENVIRONMENT specific 
    'production' => [
        'devMode' => false,
    ],
    'dev' => [
        'devMode' => true,
    ],
];
```

## Deploy with Git

Now that you have the configuration done, let's get the code up. If your local Craft installation is not under Git version control already, then you do it now:

```
# Initialize Git
$ git init .

# Add your App's Git remote to your local repo:
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# Add changes to Git
$ git add -A

# Commit changes
$ git commit -m 'My first commit'

# Push to deploy to fortrabbit
$ git push -u fortrabbit master
```

For reference, with this setup, further deployments are triggered by two commands:

```
$ git commit -m 'I changed something'
$ git push 
```



## Export/import the database

Database migrations are first-class citizen in Craft 3. This is great keep schema changes consistent across all enviroments.
  
However, this does not help with the actual data which is stored locally already. You need to export a mysqldump first and then import that dump to your App's database. Here is how:

```bash
# On your local machine
$ mysqldump -ulocal-db-user -plocal-db-password local-db-name > dump.sql
```

Now you need to open a tunnel and import the just created dump file into your database. This requires two terminal windows: One containing the open tunnel, the other to execute the import.

```bash
# Open the tunnel
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@deploy.{{region}}.frbit.com

# !!! in a new terminal window !!!
# Import the dump
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```



## Moving assets (existing uploads) to fortrabbit

sftp or [rsync](https://blog.fortrabbit.com/deploying-code-with-rsync)




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
