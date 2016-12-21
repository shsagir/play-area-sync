---

template:      article
reviewed:      2016-12-20
naviTitle:     General
title:         Deploy code to fortrabbit with Git
lead:          This is the famous 5 minute tutorial to get started with fortrabbit. See how fast and easy you can get your code up and running.
stack:         all
group:         Install_guides


keywords:
    - tutorial
    - guide
    - git-deployment
    - Git
    - terminal
    - shell
    - bash

---

## Get ready

Let's have a quick check-up before we get started here:

1. You have already [signed up to fortrabbit](//dashboard.fortrabbit.com/signup)
2. You should have created a plain vanilla [App](app) (free trial)
3. You should have a [PHP development environment](/local-development) or at least [Git](/git) running on your local machine



## Deploying

The following code gets executed on your local machine in your terminal application. The lines starting with a `$` are the ones you execute. The following code examples include the Git clone URL from your App: **{{app-name}}**.

```bash
# 1. Clone the repo — Initialize Git by cloning the empty repo from fortrabbit
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
# Cloning into '{{app-name}}'...
# warning: You appear to have cloned an empty repository

# 2. Change directory to your new local project
$ cd {{app-name}}

# 3. Create an index php file with a bold messsage on the fly
$ echo '<?php echo "PHPower to the PHPeople";' > index.php

# 4. Add the change in the working directory to the staging area
$ git add index.php

# 5. Commit the staged snapshot to the project history
$ git commit -am 'initial commit'

# 6. Just push to deploy
$ git push -u origin master
# Counting objects: 3, done.
# Writing objects: 100% (3/3), 251 bytes | 0 bytes/s, done.
# Total 3 (delta 0), reused 0 (delta 0)

# Commit received, starting build of branch master
#
# –––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
#
# B U I L D
#
#
# Checksum:
#   3ec951f4f4a59a42afa7e6bebaa78477672d3b6a
#
# Deployment file:
#   not found
#
# Pre-script:
#   not found
#   0ms
#
# Composer:
#   not executing (composer.json missing)
#   0ms
#
# Post-script:
#   not found
#   0ms
#
#
#
# R E L E A S E
#
#
# Packaging:
#   6ms
#
# Revision:
#   1446568383255266100.3ec951f4f4a59a42afa7e6bebaa78477672d3b6a
#
# Size:
#   182B
#
# Uploading:
#   134ms
#
# Build & release done in 217ms, now queued for final distribution.
#
# –––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
#
# To {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
#  * [new branch]      master -> master
# Branch master set up to track remote branch master from origin.
```
**Got an error?** Please see [access troubleshooting](/access-methods#toc-troubleshooting) and [Git troubleshooting](/git).
**Did it work?** Cool! That's it. You now visit your App URL in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

### Further readings

This was just a simple example. Don't miss our install guides for [Laravel](install-laravel), [WordPress](install-wordpress), [Symfony](install-symfony), [Craft CMS](install-craft), [Drupal](install-drupal) …
