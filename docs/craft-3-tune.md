---

template:         article
reviewed:         2018-06-10
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

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local), [configured](/craft-3-setup) and deployed it your fortrabbit App. This guide helps you with running, tuning and troubleshooting.

<!--

  TBD:
  The purpose here is to give tips on best practices, everything that is "nice to have", but not a must, nothing that is absolutely required to run Craft. Maybe one of the topics below is required to run Craft? We can move it to the setup article. I believe is absolutely necessary. I tried to group this by context, config settings are together, ENV vars are together … etc.

  TODO:
  check for errors there might be many. I found the headline "cpTrigger" and guessed the purpose.

-->


## Environment detection

In [modern Craft develop-and-deploy workflows](/craft-3-about) your local development environment is where changes are developed and tested first. See more on environment detection in our [local development](local-development#toc-environment-detection). Craft 3 knows about this concept and provides a convenient way to check where the installation currently runs. We assume fortrabbit to be your production environment, so it has been set accordingly in the `ENVIRONMENT` ENV var. We suggest to set your local `ENVIRONMENT` ENV var to `dev`:

```
# Local environment ENV setting
ENVIRONMENT=dev
```

### Craft config example

Below is an example of a `config/general.php`. It sets some useful shared settings, like [alllowUpdates](#toc-allowupdates) and [cpTrigger](#toc-cptrigger), as well as [DevMode](#toc-dev-mode) for local development.

<!--

  TODO: 
  shouldn't we include a siteURL in this example? DEV + Prod?

-->

```
<?php
return [
    // Global settings
    '*' => [
        'cpTrigger'    => 'godmode',
        'securityKey'  => getenv('SECURITY_KEY'),
        'siteUrl'      => getenv('SITE_URL'),
        'allowUpdates' => false
    ],
    // ENVIRONMENT specific 
    // fortrabbit
    'production' => [
        'devMode' => false
    ],
    // local
    'dev' => [
        'devMode' => true
    ]
];
```

## Dev Mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what Dev Mode is for. See the [Craft docs](https://craftcms.com/support/dev-mode) for more details. See [above](#toc-craft-config-example) for an configuration groups example.

## Manually setting ENV vars

Our [Software Preset](/app#toc-software-preset) will populate the ENV vars on fortrabbit for you, see [here](/env-vars) for more on ENV vars. So when you have not chosen Craft while creating the App, or you are just curious how this works, or your ENV vars have been deleted accidentally, here is what will be set:

### Craft ENV vars on fortrabbit

```dotenv
DB_DATABASE=${MYSQL_DATABASE}
DB_DRIVER=mysql
DB_PASSWORD=${MYSQL_PASSWORD}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
ENVIRONMENT=production
SECURITY_KEY=LongRandomString
```

## MySQL table prefixes

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. You can set the table prefix, locally in your `.env` file and on fortrabbit with the App's [ENV vars](/#toc-craft-config-example) like so:

```dotenv
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

## Updating Craft

From time to time a new minor Craft version will come out, dot or a dot-dot, like an update from 3.1.5 to 3.1.6. We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. Depending on your deployment workflow — [Git](/craft-3-deploy-git) or [SFTP](/craft-3-upload-sftp) — there are two ways to update Craft:

### A. Update Craft with a Git workflow

When you have used a Git to deploy Craft, your update workflow likely looks like this: Use Composer to first update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**: 

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

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App. During the [Git deployment](/git-deployment), `composer install` will run automatically. This way your local Composer changes get applied on the remote. Some plugins or the Craft core may include database migrations. Don't forget to run the following SSH remote execution command after the updated packages are deployed:

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft setup/update"
```

### B. Update Craft with a SFTP workflow

Just use the shiny update button interface. Follow [the official guides](https://docs.craftcms.com/v3/updating.html). You need to do that twice: Once for your local installation, once for the one on remote (ony your App).

## allowupdates

Craft CMS has the option to run updates directly from the Craft control panel. A client or editor might be tempted to use that update button in the interface directly on the fortrabbit App. This is not a good idea, as Git is a [one-way street](/deployment-methods-uni#toc-git-works-only-one-way) on the Uni Stack and that those changes even will get lost on the Pro Stack, due to [ephemeral storage](/app-pro#toc-ephemeral-storage). So you better prevent the shiny "update me" button from showing up at all. You can do that in your Craft configuration, see an [example above](#toc-craft-config-example). Also see the [official guide](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-allowupdates) and [this question](https://craftcms.stackexchange.com/a/27/4504). 

## cpTrigger

"Security through obscurity" is a widely discussed concept. We suggest to obscurify the control panel URL of your Craft installation, just because you can. The "cpTrigger" variable in your 

## The storage folder

The `storage` folder within Craft is part of the [fortrabbit custom `.gitignore` file](). So we 

* [docs.craftcms.com/v2/folder-structure.html#craft-storage](https://docs.craftcms.com/v2/folder-structure.html#craft-storage)
* [craftcms.com/support/craft-storage-gitignore](https://craftcms.com/support/craft-storage-gitignore)


<!--

  TODO! 
  Write something on domains! 
  App URL, how to add your first real domain!
  https://trello.com/c/XgaKA9JV/918-default-domain-enhancements


## siteUrl

* https://craftcms.com/support/site-url

-->


## Image tuning 

Image uploads to Craft are usually getting processed by ImageMagick. [Some people suggest](https://nystudio107.com/blog/creating-optimized-images-in-craft-cms) to further optimize images with jpegoptim or optipng. These tools are not available here on fortrabbit, see [here why](/quirks#toc-no-root-shell). But there are some good alternatives. We evangelize to use dedicated specialized third party image optimization services, like imgix, tinypng, kraken or imageoptim to do the job best. These two Craft plugins are supporting external services:

* [Imager Craft](https://github.com/aelvan/Imager-Craft/)
* [Craft Imageoptimize](https://github.com/nystudio107/craft-imageoptimize)

Don't forget that this is only tuning — making images a little smaller. Also check out our [application design article](/app-design) on website performance best practices.



## Older Craft versions

We have an install guide for Craft CMS Version 2 [over here](/install-craft-2-uni) as well, but recommend to use Craft CMS 3 instead. Consider an [upgrade](/craft-2-3-upgrade) when you have an old installation. 

## Multi site

Craft now supports to host multiple websites in a single installation, see the [offical docs](https://docs.craftcms.com/v3/sites.html) on that topic. Use cases for this, is a one website in very similar versions, for example the same website in different languages or marketing landing pages that are very similar. Please don't try to install all you different Craft [websites in one App](/app#toc-one-app-one-website).




<!--

TODO:

integrate the following topics, headlines and snippets:




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

- - -

cache headers images:
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/conversation/16319087993

<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.+)\.(\d+)\.(bmp|css|cur|gif|ico|jpe?g|js|png|svgz?|webp|webmanifest)$ $1.$3 [L]
</IfModule>
 

-->