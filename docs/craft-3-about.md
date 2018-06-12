---

template:         article
reviewed:         2018-06-11
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

## Choose your Stack

There are [two hosting stacks](/stacks) here on fortrabbit. You can choose with each new [App](/app) your create:

1. The [Universal Stack](/app-uni) has VPS or shared hosting feeling. It's easy to use, affordable, but limited in scaling. Use it for testing + small projects.
2. The [Professional Stack](/app-pro) is designed to scale horizontally. All components are redundant to assure high uptime. There is only Git deployment, [ephemeral storage](/app-pro#toc-ephemeral-storage) and assets are stored on a remote file system like the [Object Storage](/craft-3-assets-pro). Use it for serious business + potentially large sites.


## Choose your workflow

There are many ways to work with Craft CMS — depending on project needs and skills.

### Modern workflow

Sophisticated developers, experienced in backend development and ready to work with the Terminal, [Git](/git) and [Composer](/composer) will benefit the most from our advanced workflows. This is were our 💜 is.


1. **[Be ready](/get-ready)**: fortrabbit App + local dev env
2. **[Craft running locally](craft-3-install-local)**: install new with Composer or existing project
3. **[Setup Craft](/craft-3-setup)**: Prepare environments, sync database
4. **[Deploy Craft with Git](/craft-3-deploy-git)**
5. **Manage assets**: [rsync for Uni](/craft-3-assets-uni), [Object Storage for Pro](/craft-3-assets-pro) 

### Craft Copy

**Take a shortcut!** We have published this little handy open-source command line tool to speed up common deployment tasks around Craft CMS on fortrabbit. It connects your local Craft CMS installation with an App on fortrabbit and then enables you to sync database and assets in a speedy and convenient way. It guides you through the process of setting everything up, it even has a table to show you which parts are still missing. Give it a try! Here is an appetizer:

```bash
# Install and initialize
$ composer require fortrabbit/craft-copy
$ ./craft install/plugin
$ ./craft copy/setup

# Sync database (local ⇄ fortrabbit )
$ php craft copy/db/up
```

Please head on to the GitHub page for more usage examples:

* [github.com/fortrabbit/craft-copy](https://github.com/fortrabbit/craft-copy)


### Legacy workflow

Juniors and web developers with a focus on front-end are likely more comfortable using the legacy SFTP workflow. Use this workflow when you are coming from WordPress and not yet familiar to use the Terminal, Git and Composer. This is your route:

1. **[Be ready](/get-ready)**: fortrabbit App + local dev env
2. **[Craft running locally](craft-3-install-local)**: install new with Composer or existing project
3. **[Setup Craft](/craft-3-setup)**: Prepare environments, sync database
4. [Upload Craft with SFTP](/craft-3-upload-sftp)
