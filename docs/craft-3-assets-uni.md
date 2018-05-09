---

template:         article
reviewed:         2018-05-09
title:            Manage Craft assets with rsync
naviTitle:        Manage Craft assets with rsync
lead:             Learn how to deploy Craft CMS runtime data to the Object Storage with fortrabbit Universal Apps using rsync.
group:            craft
stack:            pro
workInProgress:   yes

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5
uniLink:          craft-3-deploy-with-git-uni

otherVersions:
    2.6 : install-craft-2-pro

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready) and have [Craft running on your local machine](/craft-3-install-local). This guide is for advanced users on the advanced Uni Stack already using [Git deployment to deploy Craft](/craft-3-deploy-git).

## Managing assets

Assets in Craft are the files that are managed by Craft. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to the App are [not represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyways. Assets uploaded to an Pro App will get destroyed the next you time deploy anyways, see [here](/app-pro#ephemeral-storage) for more and why.s

So when you deploy using Git here, the assets must be managed and deployed independently. [SFTP](/sftp-uni#toc-accessing-sftp) is one way to to do it. We recommend giving `rsync` on the command line a try. It's easier than you think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.
