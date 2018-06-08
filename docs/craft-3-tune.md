---

template:         article
reviewed:         2018-06-05
title:            Tune Craft CMS
naviTitle:        Tune Craft
lead:             Tips, tricks, best practices and advanced topics on how to run Craft CMS successfully on fortrabbit.
group:            craft
stack:            all
order:            20

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.9

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local), [configured](/craft-3-setup) and deployed it your fortrabbit App. This guide helps you with tuning and answers some common topics around running and tuning Craft.


## Updating Craft

From time to time a new minor Craft version will come out, dot or a dot-dot, like an update from 3.1.5 to 3.1.6. We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. Depending on your deployment workflow — [Git](/craft-3-deploy-git) or [SFTP](/craft-3-upload-sftp) — there are two ways to update Craft:

### A. Update Craft with a Git workflow

When you have used a Git to deploy Craft, your update workflow likely looks like this: You will update your local installation first, then, when everything works, you will deploy the changes to your fortrabbit production App. Use Composer to first update your local installation, then push the changes to trigger the update on remote.

Run the following command in the terminal on your computer **locally**: 

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

#### Disable updates from the admin interface

Craft CMS has the option to run updates directly from the admin control panel. A client or editor might be tempted to use that update button in the interface directly on the fortrabbit App. This is not a good idea, as Git is a [one-way street](/deployment-methods-uni#toc-git-works-only-one-way) on the Uni Stack and that those changes even will get lost on the Pro Stack, due to [ephemeral storage](/app-pro#toc-ephemeral-storage). So you better prevent the shiny "update me" button from showing up at all. You can do that in the Craft configs, see also the [official guide](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-allowupdates) and [this question](https://craftcms.stackexchange.com/a/27/4504), like so:

```
public $allowUpdates = false;
```


### B. Update Craft with a SFTP workflow

Just use the shiny update button interface. Follow [the official guides](https://docs.craftcms.com/v3/updating.html). You need to do that twice: Once for your local installation, once for the one on remote (ony your App).


## Environment settings

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

## The storage folder

The `storage` folder within Craft is part of the [fortrabbit custom `.gitignore` file](). So we 

* [docs.craftcms.com/v2/folder-structure.html#craft-storage](https://docs.craftcms.com/v2/folder-structure.html#craft-storage)
* [craftcms.com/support/craft-storage-gitignore](https://craftcms.com/support/craft-storage-gitignore)


<!--

TODO:

HTTPS?

I'd like to see the TLS/HTTPS topic covered in the help pages here for Craft, it's new for many users how this is done here. We have the X-forward header thing in the general article. Maybe there is a setting in Craft CMS?

https://craftcms.stackexchange.com/questions/4128/how-do-i-force-ssl-on-craft?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

- - 

License keys

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