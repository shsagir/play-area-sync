---

template:         article
reviewed:         2018-06-05
title:            Setup Craft CMS
naviTitle:        Setup Craft
lead:             Learn how to configure Craft CMS to run locally AND on fortrabbit, smoothly.
group:            craft
stack:            all
order:            10

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.9

otherVersions:
    2 : install-craft-2-uni


keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

<!--

TODO: 

* check contents of old articles, where is the craft-copy part? integrate (elsewhere)
* …

-->


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local) and deployed it your fortrabbit App. 


<!--

TODO: rethink headlines:

* The Craft in the headline is notz needed here?
* What does the next headline and the one after actually say? whta makes them different?

-->

## Craft environment configuration

<!--

TODO: Storing credentials in an ENV var and enviroment detection are not the same thing IMO. I would separate the two topics. 1st of all, the credentials are stored in ENV. Second: use ENV detection to differentiate between local and prod.

-->

Instead of hard coding secret credentials - like the database username and password - into your config files directly — like with WordPress — Craft 3 uses a much smarter approach: [environment detection](local-development#toc-environment-detection). So you can run your Craft locally and on remote without code or configuration file changes.

Locally, your `.env` file will be modified and read. On fortrabbit the [environment variables](/env-vars) are getting feeded from the ones you can set in the Dashboard. When have chosen Craft in [software chooser](/app#toc-software-preset) while you have created the App, all ENV vars at fortrabbit are already pre-populated, all set and done.


### Environment settings

<!-- TODO: make clear which file is edited here! -->

We assume fortrabbit to be your production environment, so it has been set accordingly in the `ENVIRONMENT` ENV var. 

```
<?php
return [
    // Global settings
    '*' => [
        'cpTrigger' => 'brewery',
        'securityKey' => getenv('SECURITY_KEY'),
        'siteUrl' => getenv('SITE_URL')
    ],
    // ENVIRONMENT specific 
    'production' => [
        'devMode' => false,
        'allowUpdates' => false
    ],
    'dev' => [
        'devMode' => true,
    ],
];
```

The `ENVIRONMENT` which is defined in the ENV vars, maps with the array key `production` (usually fortrabbit), or `dev` (usually locally).

### Security key

<!-- TODO: shorten -->

From the Craft Docs: "Each Craft CMS project should have a unique security key. This key is shared between the environments that the project runs on." This mandatory key is automatically generated, when using a Composer installation, you can also assign it manually in the `.env` file or trigger a terminal command to set it. 

It is crucial that the security key is the same on all of the environments the Craft projects runs in. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```dotenv
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard.

### MySQL table prefixes

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. Set the table prefix on fortrabbit with the App [ENV vars](/env-vars).

```dotenv
# Example Table prefix
DB_TABLE_PREFIX=craft_
```


### cpTrigger

### devMode

### allowUpdates

### siteUrl



## Database synchronization

This can easily be done with the [Craft Copy Plugin](https://github.com/fortrabbit/craft-copy) we developed for this purpose. Follow these steps:

```bash
# Install and initialize
$ composer require fortrabbit/craft-copy
$ ./craft install/plugin
$ ./craft copy/setup

# Sync
$ php craft copy/db/up
```

If you prefer to export/import manually: Head over to our [MySQL export & import guide](/mysql#toc-export-amp-import) to learn how to access the database on fortrabbit.





<!-- TODO:

HTTPS?

I'd like to see the TLS/HTTPS topic covered in the help pages here for Craft, it's new for many users how this is done here. We have the X-forward header thing in the general article. Maybe there is a setting in Craft CMS?

https://craftcms.stackexchange.com/questions/4128/how-do-i-force-ssl-on-craft?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa

- - 


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
