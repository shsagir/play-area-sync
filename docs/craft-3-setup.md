---

template:         article
reviewed:         2018-06-09
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


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already [installed Craft locally](craft-3-install-local) and deployed it your fortrabbit App. 

## Craft configuration on fortrabbit

Craft 3 uses modern `.env` style configuration, learn more about the concepts [here](/env-vars). In result, you can run your Craft locally and on remote without code or configuration file changes. Locally, your `.env` file will be modified and read.

On fortrabbit the [environment variables](/env-vars) are getting seeded from the ones you can set in the Dashboard. When you have chosen Craft in the [Software Preset](/app#toc-software-preset) while have creating the App, all ENV vars at fortrabbit are already pre-populated, all set and done. It will work out of the box. If not, see [here](/????)

## Security key

From the Craft Docs: "Each Craft CMS project should have a unique security key. This key is shared between the environments that the project runs on." This mandatory key is automatically generated, when using a Composer installation, you can also assign it manually in the `.env` file or trigger a terminal command to set it. 

It is crucial that the security key is the same on all of the environments the Craft projects runs in. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```dotenv
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard.

## MySQL table prefixes

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. You can set the table prefix on fortrabbit with the App's [ENV vars](/env-vars) like so:

```dotenv
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

### cpTrigger

### siteUrl

## Database setup



## Database synchronization

You will probably often want to synchronize your local database with the one on fortrabbit. The manual way is a bit mundane, so we have developed a very cool tool: [Craft Copy](https://github.com/fortrabbit/craft-copy)

If you still prefer to export/import manually: Head over to our [MySQL export & import guide](/mysql#toc-export-amp-import) to learn how to access the database on fortrabbit.


## Image tuning 

Image uploads to Craft are usually getting processed by ImageMagick. [Some people suggest](https://nystudio107.com/blog/creating-optimized-images-in-craft-cms) to further optimize images with jpegoptim or optipng. These tools are not available here, see [here why](/quirks#toc-no-root-shell). But there are some good alternatives. We evangelize to use dedicated specialized third party image optimization services, like imgix, tinypng, kraken or imageoptim to do the job best. These two Craft plugins are supporting external services:

* [Imager Craft](https://github.com/aelvan/Imager-Craft/)
* [Craft Imageoptimize](https://github.com/nystudio107/craft-imageoptimize)

Don't forget that this is only tuning — making images a little smaller. Also check out our [application design article](/app-design) on website performance best practices.

<!--

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
