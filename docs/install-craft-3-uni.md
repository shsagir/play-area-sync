---
template:         article
reviewed:         2018-02-23
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft 3 Beta/RC
lead:             Note that this install guide is for the Craft 3 (pre-stable) version. If you don't feel confident using a pre-release <a href='/install-craft-2-uni'>head over to the Craft 2.6 install guide</a>.
group:            Install_guides
stack:            uni
dontList:         false
workInProgress:   true

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.0-RC11

keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

---
 
## Get ready

We assume you've already created an App and chose Craft CMS in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard). 

<!--
    TBD: this is not specific to Craft CMS. Maybe include this is in all install guides or don't.

Using a SSH Key to authenticate is highly recommended. If you haven't stored your public key with your fortrabbit Account yet, [do it now](/ssh-keys#toc-save-your-public-ssh-keys-with-your-fortrabbit-account).
-->


### Root path

If you selected Craft 3 in the software chooser, the root path is already set to **web** for Craft Apps. Go to the Dashboard to verify the current settings and [modify the root path](/app#toc-root-path) of your App's domains if needed. 

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
# 1. Initialize Git
$ git init .

# 2. Add your App's Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Download a proper .gitignore file
$ curl -O  https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master

# From there on only
$ git push
```

## Export/import the database

Database migrations are first-class citizen in Craft 3. Using this pattern keeps changes consistent across all environments. However, this does not help with the actual data which is stored locally already. To get started, you'll need to export a mysqldump first and then import that into your Apps database. Here is how:

```bash
# On your local machine
$ mysqldump -ulocal-db-user -plocal-db-password local-db-name > dump.sql
```

Now you need to open a tunnel and import the just created dump file into your remote database. This requires two terminal windows: One containing the open tunnel, the other to execute the import.

```bash
# Open the tunnel
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@deploy.{{region}}.frbit.com

# !!! in a new terminal window !!!
# Import the dump
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```

You can also do this with a MySQL GUI of course, please see our [MySQL guides](/mysql) for more on the topic.


## Uploading assets

<!-- 
    TODO: Please provide some context: What are assets, any why they have not been synced with Git already? Are the already excluded by default? Or do they need to get excluded?  
-->

You are probably used to use [SFTP](/sftp-uni#toc-accessing-sftp), that's one way to upload assets. We recommend giving `rsync` on the command line try, it's easier than think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

This [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`. It's highly recommended!



## Updating Craft

The latest Craft update is just a `composer update` away. When you run this command in the terminal locally, the output looks something like this: 

```plain
  Loading composer repositories with package information
  Installing dependencies (including require-dev) from lock file
  Package operations: 0 installs, 17 updates, 1 removal
    - Updating craftcms/cms (3.0.0-RC7 => 3.0.0-RC12): Downloading (100%)
    - Updating yiisoft/yii2 (2.0.13.1 => 2.0.14): Downloading (100%)
   [...]
    - Updating ostark/craft-async-queue (1.1.5 => 1.3.0):  Checking out c262aa5e21
    - Updating symfony/var-dumper (v3.4.3 => v3.4.4): Downloading (100%)

```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App's `master` branch. During the Git deployment we run `composer install` - this way your local composer changes get applied on the remote automatically.

Some plugins or the Craft core include database migrations. Don't forget to run the following command after the updated packages are deployed:

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft setup/update"
```

## Keeping remote and local in sync



<!-- 

    TODO: How to work with changes made to the contents of the page? What when the remote version contain files, that are not locally? Why are not there when I git pull? How do I get them best?

 -->

**That's it.** 

