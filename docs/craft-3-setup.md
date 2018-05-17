---

template:         article
reviewed:         2018-05-17
title:            Setup Craft CMS
naviTitle:        Setup Craft
lead:             Learn how to configure Craft CMS to run locally and on fortrabbit.
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Get ready

Make sure to have followed [our guides](/craft-3-about) so far. You should have already installed Craft locally and on fortrabbit.  



## craft-copy

We have published this little handy open-source command line tool to speed up common deployment tasks around Craft CMS on fortrabbit. It connects your local Craft CMS installation with an App on fortrabbit and then enables you to sync database and assets in a speedy and convenient way. It guides you through the process of setting everything up. Please head on to the GitHub page to learn how to use it:

* [github.com/fortrabbit/craft-copy](https://github.com/fortrabbit/craft-copy)

## Security key

<!-- TODO: Maybe describe it the other way around? so that the local one should be inserted with Dashboard.  -->

From the Craft Docs: "Each Craft CMS project should have a unique security key. This key is shared between the environments that the project runs on." This mandatory key is automatically generated, when using a Composer installation, you can also assign it manually in the `.env` file or trigger a terminal command to set it. 

It is crucial that the security key is the same on all of the environments the Craft projects runs in. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is what the specific line looks like:

```
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

When you have installed Craft with Composer that value might already contain a value. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard.


## MySQL setup

Your Craft CMS needs to connect to two databases: on your local machine and the fortrabbit database. This is what the according lines in your local `.env` file can look like:

```
# Use your own local settings as values
DB_DATABASE=database-name
DB_SERVER=127.0.0.1
DB_USER=mysql-user
DB_PASSWORD=mysql-password
```

There is no MySQL configuration required to make Craft connect to the database on fortrabbit, when have chosen Craft in [software chooser](/app#toc-software-preset) while you have created the App. In this case all the required settings (ENV vars and root path) are already set. The actual values for accessing your local database are depending on your [local development environment](/local-development). For your Craft fortrabbit App everything should already be set and done.

### MySQL table prefixes

You can set a table prefix in the `.env` file locally or in the ENV vars settings with your App in the Dashboard like so:

```
# Example Table prefix
DB_TABLE_PREFIX=craft_
```

When your local Craft installation contains a table prefix the one on the fortrabbit App should have the same one. Set the table prefix on fortrabbit with the Apps [ENV vars](/env-vars).

## MySQL import

You most likely need to export your local database and import it to the one on fortrabbit. This can easily be done with [craft-copy](#toc-craft-copy) or manually: Head on to our [MySQL export & import guide](/mysql#toc-export-amp-import).


## Next steps

You came a long way already. But there is more to discover. Follow our [Craft 3 tuning guides](/craft-3-tuning).