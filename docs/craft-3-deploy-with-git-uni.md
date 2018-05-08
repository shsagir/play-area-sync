---

template:         article
reviewed:         2018-05-07
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS to fortrabbit using Git for the code base and rsync for the runtime data. 
group:            craft
stack:            uni

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.5
proLink:          craft-3-deploy-with-git-pro

otherVersions:
    2.6 : install-craft-2-uni

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready) and have [Craft running on your local machine](/install-craft-locally). This guide is for advanced users: [Git](/git) and [Composer](/composer) are used from the terminal. There is a more simple guide to install Craft using SFTP [over here](/craft-3-upload-with-sftp).

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
$ git commit -m 'My first commit'

# 6. Initial push and upstream
$ git push -u fortrabbit master
# The first push takes a little longer
# as it runs Composer

# 7. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting) and our [Git guide](/git).


## Upload assets with rsync

Assets in Craft are the files that are managed by Craft. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to the App are [not represented in Git](https://help.fortrabbit.com/deployment-methods-uni#toc-git-works-only-one-way) anyways.

So when you deploy using Git here, the assets must be managed and deployed independently. [SFTP](/sftp-uni#toc-accessing-sftp) is one way to to do it. We recommend giving `rsync` on the command line a try. It's easier than you think: 

```
# Sync local assets with remote
$ rsync -av ./web/assets/ {{app-name}}@deploy.{{region}}.frbit.com:~/web/assets/
```

Our [rsync tutorial](https://blog.fortrabbit.com/deploying-code-with-rsync) covers many useful rsync options, like excludes, `--dry-run` and `--delete`.

## Import the database

Now, unless your database is completely empty, you want to import your local database up to the one on fortrabbit. Please see our [mysql export & import guide](/mysql#toc-using-the-terminal) on how to do that quickly.

- - -

That should be it for now. Now you have a local Craft for quick development. You can push new code changes up and sync the assets with rsync. Visit your fortrabbit Craft App in your browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

Continue with our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.