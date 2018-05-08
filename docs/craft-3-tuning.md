---

template:         article
reviewed:         2018-05-07
title:            Tune Craft CMS
naviTitle:        Tune Craft
lead:             Learn how to tweak Craft CMS on fortrabbit, master advanced topics, overcome obstacles and tackle problems.
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Updating Craft

From time to time a new minor Craft version will come out, like an update from 3.1.5 to 3.1.6. We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. Depending on your deployment workflow — [Git](/craft-3-deploy-with-git-uni) or [SFTP](/craft-3-upload-with-sftp) — there are two ways to update Craft:

### A. Update Craft with a Git workflow

Don't click the shiny update button in the interface. Don't follow [the official guides](https://docs.craftcms.com/v3/updating.html). Use Composer instead! First update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**: 

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

### B. Update Craft with a SFTP workflow

Just use the shiny update button interface. Follow [the official guides](https://docs.craftcms.com/v3/updating.html). You need to do that twice: Once for your local installation, once for the one on remote (ony your App).

## Table prefixes

You can set a MySQL table prefix in the `.env` file locally or in the ENV vars settings with your App in the Dashboard like so:

```
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. Set the table prefix on fortrabbit with the Apps [ENV vars](/env-vars).


## The .env file

In your local environment the Craft settings are stored in the `.env` file. This hidden file is excluded from Git as it stores sensitive information only suited for a single environment. **Don't upload it to fortrabbit.** Edit the `.env` file with text editor to configure Craft to run locally. These settings include:

### Security key

We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard.

### MySQL credentials

Your Craft CMS needs to connect to two databases: on your local machine and the fortrabbit database. This is what the according lines in your local `.env` file can look like:

```
# Use your own local settings as values
DB_DATABASE=database-name
DB_SERVER=127.0.0.1
DB_USER=mysql-user
DB_PASSWORD=mysql-password
```

The actual values for accessing your local database are depending on your [local development environment](/local-development). For your Craft fortrabbit App everything should already be set and done.

#### Environment settings

We expect fortrabbit to be your production environment, so it has been set accordingly in the ENV vars. Your local development environment will be "dev". This is what your `.env` file needs to contain:

```
ENVIRONMENT=dev
```

It's maybe there already. **Pro tip**: See below on how to [enable Dev Mode](#toc-dev-mode).


## Dev Mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what Dev Mode is for. See the [Craft docs](https://craftcms.com/support/dev-mode) for more details. Here is an example of a configuration group `config/general.php`:

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


## Older Craft versions

We have an install guide for Craft Version 2 [over here](/install-craft-2-uni) as well, but recommend to use Craft 3 instead. Consider an [upgrade](/craft-2-3-upgrade) when you have an old installation. 

## Multi site

Craft now supports to host multiple websites in a single installation, see the [offical docs](https://docs.craftcms.com/v3/sites.html) on that topic. Use cases for this, is a one website in very similar versions, for example the same website in different languages or marketing landing pages that are very similar. Please don't try to install all you different Craft [websites in one App](/app#toc-one-app-one-website).


<!-- TODO:

MISSING FOLDERS
as by support request: what about an non-existing "storage" or "assets" folder?

DOMAINS
https://craftcms.com/support/site-url
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/480927/conversations/16114188408
adding domains? which config needs to be changed in Craft?

 -->