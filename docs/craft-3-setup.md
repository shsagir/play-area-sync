---

template:         article
reviewed:         2018-06-10
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

On fortrabbit the [environment variables](/env-vars) are getting seeded from the ones you can set in the Dashboard. When you have chosen Craft in the [Software Preset](/app#toc-software-preset) while have creating the App, all ENV vars at fortrabbit are already pre-populated, all set and done. **No need to configure the database connection for fortrabbit.** It will work out of the box. If not, see [here](craft-3-tune#toc-manually-set-env-vars).

## Security key

<!-- 

  TODO: 
  * Review! that way, or the other way around? 
  * Or craft-copy? (it's already shortened)
  * I don't understand: Why is the security key stored with ENV var, not with config?

-->

The mandatory Craft CMS security key has to be shared among all environments. We recommend to use the security key of your fortrabbit App. Go to the App's ENV vars settings in the Dashboard and copy the content of the `security_key` variable - it's the last line in the textarea. Here is the direct link:

* [dashboard.fortrabbit.com/apps/{{app-name}}/vars](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)

Paste that long string into the value of the `SECURITY_KEY` within your local `.env` file. Here is an example what the specific line looks like:

```dotenv
SECURITY_KEY={{PASTE-KEY-FROM-DASHBOARD-HERE}}
```

The ENV var might already contain a value. Just replace it with the one from the fortrabbit Dashboard. You can also go the other way around and paste your local existing security key to the Dashboard. You can also assign it manually in the `.env` file or trigger a terminal command to set it. 


## Next steps



