---

template:         article
reviewed:         2018-05-09
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS to fortrabbit using Git for the code base and rsync for the runtime data. 
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

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready) and have [Craft running on your local machine](/install-craft-local). This guide is for advanced users, making use of [Git](/git) and [Composer](/composer), it applies to [Professional](/app-pro) and [Universal Apps](/app-uni) on fortrabbit. There is a more simple guide to install Craft using SFTP [over here](/craft-3-upload-sftp).

## Deploy the code base with Git

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


## Import the database

Now, unless your database is completely empty, you want to import your local database up to the one on fortrabbit. Please see our [mysql export & import guide](/mysql#toc-using-the-terminal) on how to do that quickly.

Continue with our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.