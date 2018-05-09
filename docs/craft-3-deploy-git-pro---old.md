---

template:         article
reviewed:         2018-05-09
title:            Deploy Craft CMS with Git 
naviTitle:        Deploy Craft with Git
lead:             Learn how to deploy Craft CMS to fortrabbit using Git for the code base and upload the runtime data to the Object Storage. 
group:            craft
stack:            pro
workInProgress:   yes
dontList:         yes

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

For best results here, make sure you have completed all steps from the [get ready guide](/get-ready) and have [Craft running on your local machine](/install-craft-locally). This guide is for advanced users on the advanced [Pro Stack](/app-pro).

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


## Managing assets

Assets in Craft are the files that are managed by Craft. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to an Pro App will get destroyed the next you time deploy anyways, see [here](/app-pro#ephemeral-storage) for more and why.

### Upload assets to the Object Storage

So what you want, is to swap the assets folder on the file system with external files stored on the fortrabbit [Object Storage](/object-storage). For this we have developed a special Craft plugin to connect the fortrabbit Craft App with the Object Storage. All uploads will transferred to the Object Storage directly, all URLs will point to the Object Storage. See the install guide on GitHub for usage:

* [github.com/fortrabbit/craft-object-storage](https://github.com/fortrabbit/craft-object-storage)



## Import the database

Now, unless your database is completely empty, you want to import your local database up to the one on fortrabbit. Please see our [mysql export & import guide](/mysql#toc-using-the-terminal) on how to do that quickly.

- - -

That should be it for now. Now you have a local Craft for quick development. You can push new code changes up and sync the assets with rsync. Visit your fortrabbit Craft App in your browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

Continue with our [Craft tuning guide](/craft-3-tuning) to truly master Craft on fortrabbit.