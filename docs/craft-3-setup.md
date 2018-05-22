---

template:         article
reviewed:         2018-05-21
title:            Setup and tune Craft CMS
naviTitle:        Setup and tune Craft
lead:             Learn how to configure Craft CMS to run locally AND on fortrabbit, smoothly.
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5

otherVersions:
    2 : install-craft-2-uni


keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already installed Craft locally and on fortrabbit. This page has two parts: The [required setup](#toc-setup) you need to do to make it work, the recommended [tuning part](#toc-tune) with even more details around Craft CMS.

## Setup

### craft-copy

We have published this little handy open-source command line tool to speed up common deployment tasks around Craft CMS on fortrabbit. It connects your local Craft CMS installation with an App on fortrabbit and then enables you to sync database and assets in a speedy and convenient way. It guides you through the process of setting everything up, it even has a table to show you which parts are still missing. Please head on to the GitHub page to learn how to use it:

* [github.com/fortrabbit/craft-copy](https://github.com/fortrabbit/craft-copy)


### Craft environment configuration

Instead of hard coding secret credentials into your config files directly — like with WordPress — Craft 3 uses a much smarter approach: [environment detection](local-development#toc-environment-detection). So you can run your Craft locally and on remote without code or configuration file changes.

Locally, your `.env` file will be modified and read. On fortrabbit the [environment variables](/env-vars) are getting feeded from the ones you can set in the Dashboard. When have chosen Craft in [software chooser](/app#toc-software-preset) while you have created the App, all ENV vars at fortrabbit are already pre-populated, all set and done.


### Security key

<!-- TODO: Maybe describe it the other way around? so that the local one should be inserted with Dashboard.  -->

From the Craft Docs: "Each Craft CMS project should have a unique security key. This key is shared between the environments that the project runs on." This mandatory key is automatically generated, when using a Composer installation, you can also assign it manually in the `.env` file or trigger a terminal command to set it. 

It is crucial that the security key is the same on all of the environments the Craft projects runs in. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard.


### MySQL setup

Your Craft CMS needs to connect to two databases: on your local machine and the fortrabbit database. This is what the according lines in your local `.env` file can look like:

```
# Use your own local settings as values
DB_DATABASE=database-name
DB_SERVER=127.0.0.1
DB_USER=mysql-user
DB_PASSWORD=mysql-password
```

Usually there is no MySQL configuration required to make Craft connect to the database on fortrabbit, when have chosen Craft in [software chooser](/app#toc-software-preset) while you have created the App. In this case all the required settings (ENV vars and root path) are already set. The actual values for accessing your local database are depending on your [local development environment](/local-development). For your Craft fortrabbit App everything should already be set and done.

#### MySQL table prefixes

You can set a table prefix in the `.env` file locally or in the ENV vars settings with your App in the Dashboard like so:

```
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. Set the table prefix on fortrabbit with the Apps [ENV vars](/env-vars).

### MySQL import

You most likely need to export your local database and import it to the one on fortrabbit. This can easily be done with [craft-copy](#toc-craft-copy) or manually: Head on to our [MySQL export & import guide](/mysql#toc-export-amp-import).



## Tune

### Updating Craft

From time to time a new minor Craft version will come out, dot or a dot-dot, like an update from 3.1.5 to 3.1.6. We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. Depending on your deployment workflow — [Git](/craft-3-deploy-git) or [SFTP](/craft-3-upload-sftp) — there are two ways to update Craft:

#### A. Update Craft with a Git workflow

<!-- TODO: Describe how to turn off web updates in Craft -->

**Don't click the shiny update button in the interface. Don't follow [the official guides](https://docs.craftcms.com/v3/updating.html).** Use Composer to first update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**: 

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


#### Environment settings

We expect fortrabbit to be your production environment, so it has been set accordingly in the ENV vars. Your local development environment will be "dev". This is what your `.env` file needs to contain:

```
ENVIRONMENT=dev
```

It's maybe there already. **Pro tip**: See below on how to [enable Dev Mode](#toc-dev-mode).

### Dev Mode

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


### Older Craft versions

We have an install guide for Craft Version 2 [over here](/install-craft-2-uni) as well, but recommend to use Craft 3 instead. Consider an [upgrade](/craft-2-3-upgrade) when you have an old installation. 

### Multi site

Craft now supports to host multiple websites in a single installation, see the [offical docs](https://docs.craftcms.com/v3/sites.html) on that topic. Use cases for this, is a one website in very similar versions, for example the same website in different languages or marketing landing pages that are very similar. Please don't try to install all you different Craft [websites in one App](/app#toc-one-app-one-website).

### The storage folder

The `storage` folder within Craft is part of the [fortrabbit custom `.gitignore` file](). So we 

* [docs.craftcms.com/v2/folder-structure.html#craft-storage](https://docs.craftcms.com/v2/folder-structure.html#craft-storage)
* [craftcms.com/support/craft-storage-gitignore](https://craftcms.com/support/craft-storage-gitignore)


<!-- TODO:

Licsence keys


What about this?
https://github.com/fortrabbit/craft-starter
Is thi still up-to-date?


Describe what needs to be done to set a domain with Craft and fortrabbit.

DOMAINS
https://craftcms.com/support/site-url
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/480927/conversations/16114188408
adding domains? which config needs to be changed in Craft?



cache headers images:
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/conversation/16319087993

<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.+)\.(\d+)\.(bmp|css|cur|gif|ico|jpe?g|js|png|svgz?|webp|webmanifest)$ $1.$3 [L]
</IfModule>
 -->