---

template:         article
reviewed:         2018-05-07
title:            Upgrade from Craft 2 to Craft 3
naviTitle:        Upgrade from Craft 2
lead:             What you need to know, when upgrading from a Craft CMS 2 installation to Craft CMS 3 here on fortrabbit. 
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.9

keywords:
  - craft
  - craftCMS2
  - craftCMS3
  - upgrade

---


## Get ready

First of all, please read the official upgrade guide: [docs.craftcms.com/v3/upgrade.html](https://docs.craftcms.com/v3/upgrade.html). Use that as your main source.

## About the changes in Craft 3

Craft 2 was way more simple. So we have recommended to use [an SFTP deployment](/install-craft-2-uni). With Craft 3 many things are different, modern and more powerful. Just the way we like it:

* configuration with `dotenv` files
* new directory structure
* Composer based workflow
* Git support

## Choose your upgrade path

Now you have two options to upgrade from Craft 2 to Craft 3:

### 1. Quick update

The above linked guide offer you a way to upgrade with as little intervention as possible. Go that route if you are looking a quick way to upgrade. Keep your current directory structure and continue the [SFTP workflow](/craft-3-upload-sftp). 

### 2. Full update

So you want to make use Git and Composer and resemble the new Craft 3 directory structure? Then you best: Start a new Craft 3 project from scratch, import your templates, configs and contents. See the [official Craft guide](https://docs.craftcms.com/v3/upgrade.html#if-you-want-your-directory-structure-to-resemble-a-new-craft-3-project) for more on this.

On the fortrabbit side you better start a new App additional App as well. When your local development version is running the new Craft 3 version with all your contents, [deploy it with Git]() and use [rsync](craft-3-deploy-with-git-uni) to your new fortrabbit App. Once it is also running on fortrabbit, remove the [domain](/domains) from the old App running on Craft 2 and add it to the new App running on Craft 3. Once the domain switch is done, delete the now not needed Craft 2 fortrabbit App. Also check out our general [migration guide](/migrating) on how to switch seamlessly and without downtime.