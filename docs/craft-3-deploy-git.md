---

template:         article
reviewed:         2018-05-09
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS code base with Git to fortrabbit. 
group:            craft
stack:            all

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.9

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready) and have [Craft running on your local machine](/craft-3-install-local). This guide is for advanced users, making use of [Git](/git) and [Composer](/composer), it can be applied to [Professional](/app-pro) and [Universal Apps](/app-uni) on fortrabbit. 

There is a more basic guide to install Craft using SFTP on Universal Apps [over here](/craft-3-upload-sftp).

## Deploy the Craft code base with Git

Trigger the following commands in your **local** terminal:

```
# 1. Initialize Git (when not already done)
$ git init .

# 2. Add your App's Git remote to your local repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Download the fortrabbit Craft .gitignore file
$ curl -O  https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore

# 4. Add changes to Git
$ git add -A

# 5. Commit changes
$ git commit -m 'Initial commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master
# The first push takes a little longer
# as it runs Composer

# 7. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [Git guide](/git).


## Next steps

[Configure Craft](/craft-3-setup) to complete your setup. After that, import your local database to the one fortrabbit. Head on to our [MySQL export & import guide](/mysql#toc-export-amp-import) or take a shortcut with the [craft-copy CLI](/craft-3-tuning#toc-craft-copy).

Please also make yourself familiar with the options to deploy the Craft `assets` folder separately. There are two dedicated guides here, depending on the [Stack](/craft-3-about#toc-choose-your-stack): [Deploy assets with rsync (Uni)](/craft-3-assets-uni) and [Deploy assets to the Object Storage (Pro)](/craft-3-assets-pro).

Last not least, see our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.