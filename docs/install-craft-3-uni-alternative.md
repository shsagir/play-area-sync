---

template:         article
reviewed:         2018-05-01
title:            Install Craft CMS 3 on fortrabbit
naviTitle:        Craft
lead:             Craft is a CMS you and your clients love. Learn how to deploy Craft using Git on fortrabbit.
group:            Install_guides
stack:            uni
proLink:          install-craft-3-pro

dontList:         yes
workInProgress:   yes

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.4

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

We assume you've already created an App and chose Craft CMS 3 in the software stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard). 


### Root path

If you have selected the Craft 3 preset in the software chooser - while setting up the App on fortrabbit - the root path is already set to **web** for Craft 3. In the Dashboard you can verify the current settings and [modify the root path](/app#toc-root-path) of your [domains](/domains) if needed. 

<div markdown="1" data-user="known">
[Change the root path for App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### Run a local web server with PHP

We assume that you have a PHP development environment with Apache, MySQL, PHP and Git setup and running on your local machine. This is crucial, because you first will install Craft locally. Find more details on how to do that in our [local development article](local-development).


## Install Craft CMS locally

Use the [official Craft 3 install guide](https://github.com/craftcms/docs/blob/v3/en/installation.md) as your guideline to install Craft on your local machine first.

The way you do that will set the course on how you will deploy Craft CMS on fortrabbit. There are two ways to get Craft 3: A. via Composer or B. by download. Those ways are matching the different [deployment workflows - SFTP and Git](/deployment-methods) here on fortrabbit. Choose your workflow now:

### A. Install Craft with Composer + deploy with Git

This is the recommended - more sophisticated - way. You will use Git and Composer in the Terminal. Run this command on you local machine to create a Craft 3 project on your local machine:

```
$ composer create-project craftcms/craft {{app-name}}
```

Later on you will [configure Craft](#toc-configure-craft-cms) and then use Git to deploy your project code files and rsync to deploy the assets.


### B. Download the Craft zip file + upload with SFTP

Looking for simple download+upload? We've got you covered here as well. SFTP also works here. Download Craft directly from the Craft website:

* [craftcms.com/latest-v3.zip](https://craftcms.com/latest-v3.zip)

Unpack that zip file to get to the actual project files. Later on you will [configure Craft](#toc-configure-craft-cms) and upload your files by SFTP to your App.


## Configure Craft CMS

By now: you should have your fortrabbit App waiting there and the Craft project files on your local machine. But it's not running anywhere yet. It's time for some settings now. We gonna configure Craft CMS in a way that the same set of configurations will work for your local installation and for the one on fortrabbit.

Craft 3 uses environment variables to access environment specific settings like the MySQL database connection. With your fortrabbit App, those variables are already pre-populated, so the only settings you actually need to do, are the ones to make it work on your local machine.

In your local environment these settings are stored in the `.env` file. This hidden file is excluded from Git as it stores sensitive information only suited for a single environment. Edit the `.env` file with text editor. Don't upload it to fortrabbit. These settings need to be done:

### Security key

<!--  TODO: maybe wrong? -->

Use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard.


### MySQL

Your Craft CMS needs to connect to two databases: 1. To the database on your local machine, 2. to the fortrabbit database. This is what the according lines in your local `.env` file can look like:

```
# Use your own local settings as values of course
DB_DATABASE=database-name
DB_SERVER=127.0.0.1
DB_USER=mysql-user
DB_PASSWORD=mysql-password
```

For the fortrabbit side everything is already set and done.


### Environment settings

We expect fortrabbit to be your production environment, so it has been set accordingly in the ENV vars. Your local development environment will be "dev". This is what your `.env` file needs to contain:

```
ENVIRONMENT=dev
```

It's maybe there already. **Pro tip**: See below on how to [enable Dev Mode](#toc-dev-mode).

- - -

**Wrap up**: Congratulations. You came a long way already. By now your Craft CMS should already by able to run locally. You can run the [Setup Wizard](https://github.com/craftcms/docs/blob/v3/en/installation.md#step-6-run-the-setup-wizard) in your browser now.

- - -

Now it's time to show your great Craft website to the world for the first time. Deploy your local Craft CMS to fortrabbit. Start here if you already have an existing Craft installation.

So depending on how you downloaded, Craft CMS, you'll now continue with either A. or B.:


## A. Deploy with Git

Use Git deployment when you have already used Composer to create your Craft installation. Trigger the following commands in your **local** terminal:


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


### Uploading assets in a Git workflow

<!-- TODO: 
    Explain and link to what assets are, is it:

    A: User Uploads
    B: Minified CSS, JS and IMGs?
    C: Both

If it's A: briefly touch the topic of over-write but not delete sync strategy to explain why this is the case here.

-->

The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes this: `/web/assets/*`. So, all assets are excluded from Git and must be deployed independently. [SFTP](/sftp-uni#toc-accessing-sftp) is one way to to do it. 

We recommend giving `rsync` on the command line a try, it's easier than think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.


## B. Upload Craft CMS via SFTP

Use SFTP to upload your Craft website, when you started by downloading the archive file.

This workflow is so simple and common that it doesn't actually needs much explanations. Just grab your personal SFTP login credentials from the Dashboard. Use any SFTP client. Upload all contents of your local Craft folder into the `htdocs` folder of your fortrabbit App. 


- - -

That's it basically. Thanks for following so far!

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

### Updating Craft with a Git workflow

We assume that you are not going to update 

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

## Dev Mode

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


## Older versions of Craft

We have an install guide for Craft Version 2 [over here](/install-craft-2-uni) as well.

<!--

TBD: 

* Is there no way to do SFTP with Craft 3? We should mentioned it, why not? DRAFT:


## Deploy with SFTP

Terminal, Composer and Git are maybe not your thing? You just want to upload your Craft CMS using SFTP? Well, that's not so easy any more. Craft 3 depends on Composer to manage the dependencies. You might can upload your local SFTP and then execute Composer on the remote machine. But for that you'll also need to login via SSH and run commands in the Terminal.

-->
