---

template:         article
reviewed:         2018-05-07
title:            Install Craft CMS locally
naviTitle:        Install Craft locally
lead:             Craft is a CMS you and your clients will love. Learn how to install Craft CMS locally, matching your deployment workflow.
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

## Get ready

1. Make sure to have a [local development environment](/local-development) up and running.

## Choose your workflow

Use the detailed [official Craft 3 install guide](https://github.com/craftcms/docs/blob/v3/en/installation.md) as your guideline to install Craft on your local machine first. The way you do that will set the course on how you will [deploy](/deployment-methods) Craft CMS here on fortrabbit. **Choose your workflow now**, a or b?


#### 1a. Download Craft with Composer

This is the recommended - more sophisticated - way. You will use [Git](/git) and [Composer](/comoser#toc-local-composer) in the Terminal. Run this command **on you local machine** to create a Craft 3 project:

```
$ composer create-project craftcms/craft {{app-name}}
```

See an error? Check your [local development](/local-development).

#### 1b. Download the Craft zip file

Are you more "designer" and less "developer"? SFTP also works here. Just download Craft directly from the Craft website: [craftcms.com/latest-v3.zip](https://craftcms.com/latest-v3.zip). Unpack that zip file to get to the actual project files.


### Install Craft CMS locally

Craft 3 uses environment variables to access environment specific settings. With your fortrabbit Craft App, those variables are already set. Configure it to work on your local machine now. You have two options to install Craft:

#### 2a. Terminal setup

```
# 1. go into your local Craft folder 
$ cd {{app-name}}

# 2. run the terminal installer
$ ./craft setup
```

This will ask you some questions, the defaults will work mostly, you can change these settings [later](#toc-mastering-the-env-file).

#### 2b. Browser setup

You can also run the installer in the browser by visiting this address: `http://{{host}}/index.php?p=admin` in your browser. Substitute `{{host}}` with the [host name of your local development environment](/local-development#toc-virtual-hosts). 

- - -

**Wrap up**: By now your Craft CMS should already run locally. You should be able to visit your Craft installation on your local machine and login to the Craft admin panel.
