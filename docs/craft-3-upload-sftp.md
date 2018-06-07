---

template:         article
reviewed:         2018-05-12
title:            Upload Craft CMS with SFTP 
naviTitle:        Upload Craft with SFTP
lead:             Are you more "web designer" and less a "web developer"? Learn how to upload Craft in a classical way using SFTP. 
group:            craft
stack:            uni

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

Make sure to have completed the other steps from the [Junior workflow](craft-3-about#toc-juniors) before. This guide here follows the easiest path to get Craft up and running on fortrabbit, we also have a better but more advanced workflow to [deploy Craft with Git](/craft-3-deploy-with-git-uni) here.

## Upload Craft with SFTP

This workflow is simple and common. It doesn't need much explanation. Everybody and his dog knows [how to use SFTP](/sftp). Check the [downloading an archive file manually](https://docs.craftcms.com/v3/installation.html) workflow from the official Craft docs as your detailed reference. 

On the fortrabbit side: Just grab your personal SFTP login credentials from the Dashboard. Use any SFTP client. Upload all contents of your local Craft folder into the `htdocs` folder of your fortrabbit App. And you are good to go.

## Troubleshooting

Doesn't work as expected? Keep calm and read on:

### Connection error

**Got an error while connecting with SFTP?** Doing this for the first time on fortrabbit? Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [SFTP guide](/sftp) to get started and all setup.

### File permissions

<!--

TODO: 

Either confirm or delete this part. I have added this based on that feedback: 
https://app.intercom.io/a/apps/ntt8mpby/inbox/inbox/search/conversations/16267404732?q=744

Maybe it's OS related? It's also in the offcial docs:
https://docs.craftcms.com/v3/installation.html#step-2-set-the-file-permissions

-->


Make sure that Craft can write the files `composer.json`, `composer.lock`, `config/license.key`, `storage/*` and `vendor/*` on the App. Set the file permissions for those files to `744` with your SFTP client. 

### Hidden .htaccess file

Don't forget to upload hidden `.htaccess` file. This file is required. You can not see that file in your Desktop, unless you set the option to show hidden files. The file browser from your SFTP client most likely will show that file by default. Leave the other hidden `.env` file — which is only for your local development — at home.

## Consider

A downside of the SFTP workflow is, that you have keep both of your Craft environments in sync manually. Our [manage assets with rsync](/craft-3-assets-uni) is optional but still helpful for SFTP users. Also see our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.


## Next steps

Your Craft fortrabbit App should already connect to the fortrabbit database, thanks to the [Software Preset](/app#toc-software-preset). Next, [configure Craft](/craft-3-setup) to complete your setup.