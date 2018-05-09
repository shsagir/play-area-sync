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

## No configuration required

It is assumed that you have chosen Craft in software chooser while you have created the App on fortrabbit. In this case all the required settings (ENV vars and root path) to make Craft run on fortrabbit are already set.


## Import the database

Now, unless your database is completely empty, you want to import your local database up to the one on fortrabbit. Please see our [mysql export & import guide](/mysql#toc-export-amp-import) on how to do that.


- - -

Now, that should be it. Finally you should be able to visit your App in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)