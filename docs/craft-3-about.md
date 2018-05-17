---

template:         article
reviewed:         2018-05-16
title:            About Craft on fortrabbit
naviTitle:        About Craft on fortrabbit
lead:             Craft 3 is the CMS you and your clients will love. We love it too. Our aim is to help you — the developer — to successfully develop and deploy Craft here. This is your entry point. 
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---


## Choose your workflow

There are many ways to work with Craft CMS — depending on project needs and skills.

### Seniors

Sophisticated developers, experienced in backend development and ready to work with the Terminal, [Git](/git) and [Composer](/composer) will benefit the most from our advanced workflows. This is were our 💜 is.

#### Choose your Stack

There are two hosting stacks here on fortrabbit, more [here](/stacks). In a nutshell: The [Universal Stack](/app-uni) offers additional backwards compatibility, is easier to use, a bit more affordable, but limited in scaling. The [Professional Stack](/app-pro) on the other hand is based on a 12-factor design, with [ephemeral storage](/app-pro#toc-ephemeral-storage) and horizontal scaling. It therefore requires some different workflows. Instead of [syncing your assets by rsync](/craft-3-assets-uni) on the file system, you will [upload those to the Object Storage](/craft-3-assets-pro). When you are justing testing or your project is small, use the Universal Stack. If your project has some traffic potentially and there serious business behind it, use the Professional Stack. Use the Tabs above to switch the documentation between the Stacks.

#### Senior workflow

1. [Be ready](/get-ready), have your App and local dev setup, with Git and Composer
2. Have an existing Craft 3 project running or [install a new on with Composer](craft-3-install-local#toc-1a-download-craft-with-composer)
3. [Setup Craft](/craft-3-setup), set ENV vars, export/import database
4. [Deploy Craft with Git](/craft-3-deploy-git)
5. Manage Craft assets by [rsync for Uni](/craft-3-assets-uni) or by [Object Storage upload for Pro](/craft-3-assets-pro) 
6. Further tune Craft to get most out of it


### Juniors

Beginners and web developers with a focus on front-end are likely more comfortable using the legacy SFTP workflow. Use this workflow when you are coming from WordPress and not yet familiar to use the Terminal, Git and Composer. This is your route:

#### Junior workflow

1. [Be ready](/get-ready), have your App and local dev setup
2. Have an existing Craft 3 project running or [download Craft](craft-3-install-local#toc-1b-download-the-craft-zip-file)
3. [Setup Craft](/craft-3-setup), set ENV vars, export/import database
4. [Upload Craft with SFTP](/craft-3-upload-sftp)
5. Further tune Craft to get most out of it




