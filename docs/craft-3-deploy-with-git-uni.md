---

template:         article
reviewed:         2018-05-07
title:            Deploy Craft CMS with Git 
naviTitle:        Craft
lead:             Learn how to deploy Craft CMS to fortrabbit using Git foe th code base and rsync for runtime data. 
group:            craft
stack:            uni
proLink:          install-craft-3-pro

dontList:         no

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Prerequisite

For the best results make sure you to have this done:

1. Have completed all steps from the [get ready guide](/get-ready).
2. Have a [Craft 3 installed and running locally](/install-craft-locally).
3. Have installed [Craft using Composer](/install-craft-locally#toc-composer)

If you have just installed Craft locally by downloading the zip file, maybe better upload Craft with SFTP.

## Deploy Craft CMS with Git

Trigger the following commands in your **local** terminal:

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


### Uploading assets with rsync

Assets in Craft are the files that are managed by Craft, also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html). This user generated stuff, uploaded files, mostly images. The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to the App are [not represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyways.

So when you deploy using Git here, the assets must be managed and deployed independently. [SFTP](/sftp-uni#toc-accessing-sftp) is one way to to do it. We recommend giving `rsync` on the command line a try. It's easier than you think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.

That's it, basically. Thanks for following so far!

## Tuning

Continue reading to dig even deeper. Learn how to tweak Craft CMS and tackle problems.

### Database export and import

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

### Table prefixes

You can set a MySQL table prefix in the `.env` file locally or in the ENV vars settings with your App in the Dashboard like so:

```
# Align the prefix with the one you are using locally
DB_TABLE_PREFIX=craft_
```

### Updating Craft

From time to time a new minor Craft version will come out, like an update from 3.0.5 to 3.0.6. We recommend to always use the latest version for security reasons. Depending on your deployment workflow (see above), there are two ways to update Craft:


#### A. Update Craft with a Git workflow

Don't click the shiny update button in the interface. Don't follow [the official guides](https://docs.craftcms.com/v3/updating.html) here. User Composer instead! First update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**: 

```shell
# Make sure to be in the projects root folder (locally)
$ composer update

# The output looks something like this: 
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Package operations: 0 installs, 17 updates, 1 removal
  - Updating craftcms/cms (3.0.0-RC7 => 3.0.0-RC12): Downloading (100%)
  - Updating yiisoft/yii2 (2.0.13.1 => 2.0.14): Downloading (100%)
 [...]
  - Updating ostark/craft-async-queue (1.1.5 => 1.3.0):  Checking out c262aa5e21
  - Updating symfony/var-dumper (v3.4.3 => v3.4.4): Downloading (100%)
```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App. During the [Git deployment](/git-deployment), `composer install` will run automatically. This way your local composer changes get applied on the remote.

Some plugins or the Craft core may include database migrations. Don't forget to run the following SSH remote execution command after the updated packages are deployed:

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft setup/update"
```

#### B. Update Craft with a SFTP workflow

Just use the shiny update button interface. Follow [the official guides](https://docs.craftcms.com/v3/updating.html). You need to do that twice: Once for your local installation, once for the one on remote (ony your App).


### Mastering the .env file

In your local environment the Craft settings are stored in the `.env` file. This hidden file is excluded from Git as it stores sensitive information only suited for a single environment. Edit the `.env` file with text editor. Don't upload it to fortrabbit. These settings include:

#### Security key

We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard.


#### MySQL

Your Craft CMS needs to connect to two databases: 1. To the database on your local machine, 2. to the fortrabbit database. This is what the according lines in your local `.env` file can look like:

```
# Use your own local settings as values of course
DB_DATABASE=database-name
DB_SERVER=127.0.0.1
DB_USER=mysql-user
DB_PASSWORD=mysql-password
```

The actual values for accessing your local database are depending on your [local development environment](/local-development). For your Craft fortrabbit App everything is already set and done.


#### Environment settings

We expect fortrabbit to be your production environment, so it has been set accordingly in the ENV vars. Your local development environment will be "dev". This is what your `.env` file needs to contain:

```
ENVIRONMENT=dev
```

It's maybe there already. **Pro tip**: See below on how to [enable Dev Mode](#toc-dev-mode).








### Dev Mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what Dev Mode is for. See the official Craft CMS docs for more details:

* [craftcms.com/support/dev-mode](https://craftcms.com/support/dev-mode)

Here is an example of a configuration group `config/general.php`:

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

The `ENVIRONMENT` which is defined in the ENV vars, maps with the array key `production` (usually fortrabbit), or `dev` (usually locally).


### Older versions of Craft

We have an install guide for Craft Version 2 [over here](/install-craft-2-uni) as well, but recommend to use Craft 3 instead. Consider an upgrade when you have an old installation. 


### Upgrading from Craft 2

You already have Craft 2 CMS installed and you are now looking to upgrade it to version 3? Here is what you need to know. First of all, please read the official upgrade guide:

* [docs.craftcms.com/v3/upgrade.html](https://docs.craftcms.com/v3/upgrade.html)

Craft 2 was way more simple. So we have recommended to use SFTP deployment. With Craft 3 many things are different, modern and more powerful. Just the way we like it:

* configuration with `dotenv` files
* new directory structure
* Composer based workflow
* Git support

Now you have two options to upgrade from Craft 2 to Craft 3:

#### 1. Quick update

The above linked guide offer you a way to upgrade with as little intervention as possible. Go that route if you are looking a quick way to upgrade. Keep your current directory structure and continue the SFTP workflow. 

#### 2. Full update

So you want to make use Git and Composer and resemble the new Craft 3 directory structure? Then you best: Start a new Craft 3 project from scratch, import your templates, configs and contents.

For more details on this workflow, see the [offcial guide](https://docs.craftcms.com/v3/upgrade.html#if-you-want-your-directory-structure-to-resemble-a-new-craft-3-project).


<!-- TODO:

MISSING FOLDERS
as by support request: what about an non-existing "storage" or "assets" folder?

DOMAINS
https://craftcms.com/support/site-url
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/480927/conversations/16114188408
adding domains? which config needs to be changed in Craft?

MULTI-SITE
Multi-Site VS fortrabbit. what do we say here? https://docs.craftcms.com/v3/sites.html

 -->