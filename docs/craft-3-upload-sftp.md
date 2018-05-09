---

template:         article
reviewed:         2018-05-07
title:            Upload Craft CMS with SFTP 
naviTitle:        Upload Craft with SFTP
lead:             Are you more "web designer" and less a "web developer"? Learn how to upload Craft in a classical way using SFTP. 
group:            craft
stack:            uni

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

Make sure you to have completed all steps in the [get ready guide](/get-ready) and have a [Craft installed and running locally](/craft-3-install-locally). This guide here follows the easiest path to get Craft running on fortrabbit, we also have a better but more advanced workflow to [deploy Craft with Git](/craft-3-deploy-with-git-uni) here.

## Upload Craft with SFTP

This workflow is so simple and common that it doesn't actually needs much explanations, everybody and his dog knows [how to use SFTP](/sftp). Just grab your personal SFTP login credentials from the Dashboard. Use any SFTP client. Upload all contents of your local Craft folder into the `htdocs` folder of your fortrabbit App. Don't forget the hidden `.htaccess` file. Leave the other hidden `.env` file — which is only for your local development — at home.

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [SFTP guide](/sftp).


## Next steps

Your Craft fortrabbit App should already connect to the fortrabbit database, thanks to the [Software Preset](/app#toc-software-preset). So, next, you may want to import your local database to the one fortrabbit. Head on to our [MySQL export & import guide](/mysql#toc-export-amp-import).

After that, please make yourself familiar with the options to deploy the Craft `assets` folder separately. There are two dedicated guides here, depending on the Stack: [Deploy assets with rsync (Uni)](/craft-3-assets-uni) and [Deploy assets to the Object Storage (Pro)](/craft-3-assets-pro).

Also see our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.

