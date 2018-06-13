---

template:         article
reviewed:         2018-06-13
title:            Setup Craft CMS
naviTitle:        Setup Craft
lead:             How to configure Craft CMS to run locally AND on fortrabbit.
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

On fortrabbit the [environment variables](/env-vars) are getting seeded from the ones set in the Dashboard. When you have chosen Craft in the [Software Preset](/app#toc-software-preset) while have creating the App, all ENV vars at fortrabbit are already pre-populated. **No need to configure the MySQL database connection for fortrabbit.** If not, see [here](craft-3-tune#toc-manually-set-env-vars).

## Security key

<!-- 

  TODO: 
  * Review! that way, or the other way around? 
  * Or craft-copy? (it's already shortened)
  * I don't understand: Why is the security key stored with ENV var, not with config?
  

-->


<!--

  Version 1: - use our key! TBD or delete!
  GOTCHA: We recommend to use our Sec key, but a local one has been set and used, we say 1st install locally, 2nd config then deploy and sync database up. But isn't the admin PW tied to that key?

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is an example what the specific line looks like:

```dotenv
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

The ENV var might already be set. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard. You can also assign it manually in the `.env` file or trigger a terminal command to set it. 

-->

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use your local security key as the master key. Open your local (hidden) `.env` file from the root folder of your project and find a line that looks like this:

```dotenv
SECURITY_KEY=69UzZSEquw9E7RdCyRRTRb1lxe7h0EPd
```

It will contain a value when you have [installed Craft 3 correctly](/craft-3-install-local). Copy that line. Go to the App's ENV vars settings in the Dashboard and paste that line. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

That ENV var is already set. Just replace it with your local one. Also see the [official Craft guide on that topic](https://docs.craftcms.com/v3/installation.html#step-3-set-a-security-key) to learn about the different ways to create the key.


## Database synchronization

Now, your [local Craft installation](/craft-3-install-local) should already have created a MySQL database with a few tables in it, at least for the admin to login. The fortrabbit database, on the other side, is still empty. Now, export your local database and import it to the fortrabbit remote. Head over to our [MySQL export & import guide](/mysql#toc-export-amp-import) to learn how to access the database on fortrabbit and export/import tables.

PRO TIP: You will probably often need to synchronize development and production databases. We have developed a handy command line tool: **[Craft Copy](https://github.com/fortrabbit/craft-copy)** to speed that up.

```bash
# Sync database up (local ⟶ fortrabbit)
$ php craft copy/db/up

# Sync database down (local ⟵ fortrabbit)
$ php craft copy/db/down
```


## Next steps

Craft CMS is configured to run locally and is also ready for fortrabbit. You can [deploy it with Git](/craft-3-deploy-git) or [upload it by SFTP](/craft-3-upload-sftp) next. Don't forget our [Craft tuning guide](/craft-3-tune) afterwards.
