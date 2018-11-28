---

template:         article
reviewed:         2018-09-02
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
version:          3.0.33

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local), [configured](/craft-3-setup) and deployed it your fortrabbit App. This guide helps you with running, tuning and troubleshooting.



## Multi-Environment configuration

Craft embraces the idea of storing environment specific configurations in ENV vars. The database config ([config/db.php](https://github.com/craftcms/craft/blob/master/config/db.php)) is a good example for that.

Additionally you can create groups in every config file. The top level array key maps with the `CRAFT_ENVIRONMENT` constant, which defaults to the `ENVIRONMENT` ENV var. With this flexible approach you decide which configurations are under version control to share it with your team and which are not.

We assume fortrabbit to be your production environment, so the `ENVIRONMENT` ENV var is set to `production` on remote and locally to `dev`.

```dotenv
# Local environment ENV setting
ENVIRONMENT=dev
```

### Craft config example

Below is an example of a `config/general.php`. It sets some useful shared settings, like [userSessionDuration](#toc-usersessionduration) and [cpTrigger](#toc-cptrigger), as well as [allowUpdates](#toc-allowupdates) (false) in production and [DevMode](#toc-dev-mode) (true) for local development.


```php
<?php
return [
    // Global settings
    '*' => [
        'cpTrigger'           => 'godmode',
        'userSessionDuration' => 'P24H',
        'securityKey'         => getenv('SECURITY_KEY'),
        'siteUrl'             => getenv('SITE_URL') ?: '@web'
    ],    
    // fortrabbit
    'production' => [
        'devMode'      => false,
        'allowUpdates' => false
    ],
    // local
    'dev' => [
        'devMode' => true
    ]
];
```

### Dev Mode

Sometime while developing you might want to see some error output directly on your browser screen. That's what Dev Mode is for. See the [Craft docs](https://craftcms.com/support/dev-mode) for more details. See [above](#toc-craft-config-example) for an configuration groups example.


### allowUpdates

Craft CMS has the option to run updates directly from the Craft control panel. A client or editor might be tempted to use that update button in production. This is not a good idea, as Git is a [one-way street](/deployment-methods-uni#toc-git-works-only-one-way) and that those changes get lost. So you better prevent the shiny "update me" button from showing up at all. You can do that in your Craft configuration, see an [example above](#toc-craft-config-example). Also see the [official guide](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-allowupdates).

### cpTrigger

"Security through obscurity" is a widely discussed concept. We suggest to obscurify the control panel URL of your Craft installation, just because you can. If you don't set this value it defaults to `admin`.

### siteUrl

Using `'siteUrl' => getenv('SITE_URL') ?: '@web'` like in the [example above](#toc-craft-config-example), tells Craft to use the `SITE_URL` ENV var or the `@web` fallback, which is a good default. For multi site setups, you may need to hardcode the domains for each environment. Here is an example for that:

```php
<?php
return [

    // fortrabbit
    'production' => [
        'siteUrl' => [
            'en' => '//www.site.com',
            'nl' => '//www.site.nl',
        ]
    ],
    // local
    'dev' => [
        'siteUrl' => [
            'en' => '//en.site.dev',
            'nl' => '//nl.site.dev',
        ]
    ]
];
```


### userSessionDuration

The amount of time a user stays logged in seconds as an integer value or a [period](http://php.net/manual/en/dateinterval.construct.php) as a string.


## Manually setting ENV vars

Our [Software Preset](/app#toc-software-preset) will populate the ENV vars on fortrabbit for you, see [here](/env-vars) for more on ENV vars. So when you have not chosen Craft while creating the App, or you are just curious how this works, or your ENV vars have been deleted accidentally, here is what will be set:

### Craft ENV vars on fortrabbit

```osterei32
DB_DATABASE=${MYSQL_DATABASE}
DB_DRIVER=mysql
DB_PASSWORD=${MYSQL_PASSWORD}
DB_SERVER=${MYSQL_HOST}
DB_USER=${MYSQL_USER}
ENVIRONMENT=production
SECURITY_KEY=LongRandomString
```

### MySQL table prefixes

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. You can set the table prefix, locally in your `.env` file and on fortrabbit with the App's [ENV vars](/#toc-craft-config-example) like so:

```dotenv
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

## Updating Craft

We recommend to always use the latest version for security reasons. Mind that you are responsible for the software you write yourself and use. Test updates locally first! Depending on your deployment workflow — [Git](/craft-3-deploy-git) or [SFTP](/craft-3-upload-sftp) — there are two ways to update Craft:

### A. Update Craft with a Git workflow

When you have used a Git to deploy Craft, your update workflow likely looks like this: Use Composer to first update your local installation, then push the changes to trigger the update on remote. Run the following command in the terminal on your computer **locally**:

```bash
# Make sure to be in the projects root folder (locally)
$ composer update

# The output looks something like this:
Loading composer repositories with package information
Updating dependencies (including require-dev)
Package operations: 0 installs, 9 updates, 0 removals
  - Updating symfony/process (v4.0.11 => v4.1.0): Downloading (100%)
  - Updating symfony/console (v4.0.11 => v4.1.0): Downloading (100%)
  - Updating craftcms/cms (3.0.11 => 3.0.25): Downloading (100%)
  [...]
  - Updating symfony/event-dispatcher (v4.0.11 => v4.1.0): Downloading (100%)
Writing lock file
Generating optimized autoload files
```

The `composer.lock` file reflects the exact package versions you've installed locally. Commit your updated lock file and push it to your App. During the [Git deployment](/git-deployment), `composer install` will run automatically. This way your local Composer changes get applied on the remote. Some plugins or the Craft core may include database migrations. Don't forget to run the following SSH remote execution command after the updated packages are deployed:

```bash
$ ssh {{app-name}}@deploy.{{region}}.frbit.com "php craft migrate/all"
```

You can also add this migrate command to your `composer.json` to have it run automatically every time you push changes.

```json
"scripts": {
    "post-install-cmd": [
        "php craft migrate/all",
    ],
}
```

With that in place, any time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.


### B. Update Craft with a SFTP workflow

Just use the shiny update button in the control panel locally and upload the changes. Then, to run the database migrations, access the control panel on remote and hit the "Finish" button.


## Image tuning

Image uploads to Craft are usually getting processed by ImageMagick. [Some people suggest](https://nystudio107.com/blog/creating-optimized-images-in-craft-cms) to further optimize images with jpegoptim or optipng. These tools are not available here on fortrabbit, see [here why](/quirks#toc-no-root-shell). But there are some good alternatives. We evangelize to use dedicated specialized third party image optimization services, like imgix, tinypng, kraken or imageoptim to do the job best. These two Craft plugins are supporting external services:

* [Imager Craft](https://github.com/aelvan/Imager-Craft/)
* [Craft Imageoptimize](https://github.com/nystudio107/craft-imageoptimize)

Don't forget that this is only tuning — making images a little smaller. Also check out our [application design article](/app-design) on website performance best practices.

## Cache & Session on the Professional Stack

In multi node environments you can not rely on the file based cache or session storage. Instead you store this data in Memcache, a key-value-storage which is accessible from all nodes.  
With this little extension, no further configuration is required, you just need to pull it in to your `composer.json`:

```bash
$ composer require fortrabbit/yii-memcached
```


## Troubleshooting



### Large assets upload problems

Most likely this is a setting within Craft CMS itself. The setting you are looking for is `maxuploadfilesize` and it's default is 16MB, please see the official [Craft Docs on that](https://docs.craftcms.com/api/v3/craft-config-generalconfig.html#property-maxuploadfilesize). This can also be caused by the `post_max_size`, `memory_limit`, `upload_max_filesize` or `max_execution_time` which you can set in the Dashbaord, but by default those are OK. If that still doesn't help, check the [logs](/logging-uni) if you can find some meaningful errors.



<!--

TODO:

integrate the following topics, headlines and snippets:

- -

cache headers images:
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/conversation/16319087993

<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^(.+)\.(\d+)\.(bmp|css|cur|gif|ico|jpe?g|js|png|svgz?|webp|webmanifest)$ $1.$3 [L]
</IfModule>


-->
