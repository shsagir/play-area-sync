---

template:         article
reviewed:         2018-06-05
title:            About Craft on fortrabbit
naviTitle:        About Craft on fortrabbit
order:            1
lead:             Craft 3 is the CMS you and your clients will love. We love it too. Our aim is to help you — the developer — to successfully develop and deploy Craft here. This is your entry point. 
group:            craft
stack:            all

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
  - Professional
  - Universal

---


## Choose your workflow

There are many ways to work with Craft CMS — depending on project needs and skills.

### 1. Modern workflow

Sophisticated developers, experienced in backend development and ready to work with the Terminal, [Git](/git) and [Composer](/composer) will benefit the most from our advanced workflows. This is were our 💜 is.

#### 1.1 Choose your Stack

There are two hosting stacks here on fortrabbit. You can choose with each new App your create:

1. The [Universal Stack](/app-uni) feels very similar to a single VPS or shared hosting. It is easy to use, affordable, but limited in scaling. Use it for testing + small projects.
2. The [Professional Stack](/app-pro) on the other hand is designed to scale horizontally. All components are redundant to assure high uptime. There is only Git deployment, [ephemeral storage](/app-pro#toc-ephemeral-storage) and assets are stored on a remote file system like the [Object Storage](/craft-3-assets-pro). Use it for serious business + potentially large site with traffic.

Read more about the Stacks [here](/stacks).


#### 1.2 Modern workflow steps

<!-- TODO ??????? merge 1+2, skip local) -->
1. [Be ready](/get-ready), fortrabbit App + local dev env
2. Have an existing Craft 3 project running or [install a new on with Composer](craft-3-install-local#toc-1a-download-craft-with-composer) locally
2. [Prepare environments](/craft-3-setup#environments)<!-- TODO dead link -->
3. [Migrate database](/craft-3-setup#database)
4. [Deploy Craft with Git](/craft-3-deploy-git)
5. Manage Craft assets by [rsync for Uni](/craft-3-assets-uni) or by [Object Storage upload for Pro](/craft-3-assets-pro) 


### 2. Legacy workflow

Juniors and web developers with a focus on front-end are likely more comfortable using the legacy SFTP workflow. Use this workflow when you are coming from WordPress and not yet familiar to use the Terminal, Git and Composer. This is your route:

#### 2.1 Legacy workflow steps

<!-- TODO ??????? merge 1+2, skip local) -->
1. [Be ready](/get-ready), fortrabbit App + local dev env
2. Have an existing Craft 3 project running or [download Craft](craft-3-install-local#toc-1b-download-the-craft-zip-file)
2. [Prepare environments](/craft-3-setup#environments) <!-- TODO dead link -->
3. [Migrate database](/craft-3-setup#database)
4. [Upload Craft with SFTP](/craft-3-upload-sftp)
