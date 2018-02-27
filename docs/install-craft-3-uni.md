---

template:         article
reviewed:         2018-02-27
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
version:          3.0.0-RC12

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

We assume you've already created an App and chose Craft CMS 3 in the software stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard). 


### Root path

If you selected Craft 3 in the software chooser, the root path is already set to **web** for Craft Apps. In the Dashboard you can verify the current settings and [modify the root path](/app#toc-root-path) of your [domains](/domains) if needed. 

<div markdown="1" data-user="known">
[Change the root path for App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


## Quick start

First, have a local Craft CMS running. For this, create a new Craft project by using [Composer](/composer) like so: 

```
$ composer create-project -s RC craftcms/craft {{app-name}}
```

Follow the [official Craft 3 install guide](https://github.com/craftcms/docs/blob/v3/en/installation.md) for more details.

### ENV vars

Craft 3 uses Environment variable to access environment specific settings like the MySQL database connection - check out our [ENV vars](/env-vars) article to learn about this. So your App will connect to your local database on your local machine and to the fortrabbit database on fortrabbit — all this without code changes and without exposing the credentials in Git.

In your local environment these settings are stored in a `.env` file, which is excluded from Git. On fortrabbit the ENV vars are set in the fortrabbit Dashboard. 

The fortrabbit ENV vars are pre-populated already, when you have chosen Craft 3 in the software chooser. So you don't need to enter the fortrabbit MySQL database password. Craft knows where to pick it up. Here is what a standard ENV configuration looks like:

```osterei32
# Crypto key
SECURITY_KEY=ClickToGenerate

# The environment Craft is currently running in 
# dev, staging, production …
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


### Multi staging

One of the pre-populated ENV vars is the `ENVIRONMENT` key. We expect fortrabbit to be your production environment, so it has been set accordingly. You may want to change that value to match the name in your config files. Here is an example of a configuration group `config/general.php`:

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

The `ENVIRONMENT` you've defined in the ENV vars, maps with the array key `production`, or `dev` which is the default for your local setup.


## Deploy with Git

Now that you have the configuration done, let's get the code up. We do so with Git:

```
# 1. Initialize Git (when not already done)
$ git init .

# 2. Add your App's Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Download our Craft .gitignore file
$ curl -O  https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master
# The first push takes a little longer
# as it runs Composer

# 7. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, finally you can visit your fortrabbit Craft App in your browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


### Uploading assets

<!-- TODO: 
    Explain and link to what assets are, is it:

    A: User Uploads
    B: Minified CSS, JS and IMGs?
    C: Both

If it's A: briefly touch the topic of over-write but not delete sync strategy to explain why this is the case here.

-->

The fortrabbit [Craft starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes this: `/web/assets/*`. So, all assets are excluded from Git and must be deployed independently. [SFTP](/sftp-uni#toc-accessing-sftp) is one way to to do it. We recommend giving `rsync` on the command line a try, it's easier than think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.


## Database export and import

Database migrations are first-class citizen in Craft 3. Using this pattern keeps changes consistent across all environments. However, this does not help with the actual data which is stored locally already. To get started, you'll need to export a mysqldump first and then import that into your Apps database. Here is how to this in the Terminal:

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

### Updating Craft

The latest Craft update is just a `composer update` away. When you run this command in the terminal **locally**, the output looks something like this: 

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


<!--

TBD: 

* Is there no way to do SFTP with Craft 3? We should mentioned it, why not? DRAFT:


## Deploy with SFTP

Terminal, Composer and Git are maybe not your thing? You just want to upload your Craft CMS using SFTP? Well, that's not so easy any more. Craft 3 depends on Composer to manage the dependencies. You might can upload your local SFTP and then execute Composer on the remote machine. But for that you'll also need to login via SSH and run commands in the Terminal.

-->