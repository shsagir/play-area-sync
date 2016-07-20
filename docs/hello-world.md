---

template:      article
reviewed:      2016-02-16
naviTitle:     Hello world
title:         Deploy some code to fortrabbit
lead:          This is the famous 5 minute tutorial to get started with fortrabbit. See how fast and easy you can get your code up and running.

group:         Install_guides


keywords:
    - tutorial
    - guide
    - git-deployment
    - Git
    - terminal
    - shell
    - bash

seeAlsoLinks:
    - dashboard
    - app
    - app-design

tags:
    - beginner
    - install

---

## Get ready

Let's have a quick check-up before we get started here:

1. You have already [signed up to fortrabbit](https//dashboard.fortrabbit.com/signup)
2. You have your public [SSH key](ssh-keys) installed with your fortrabbit Account
3. You should have created a plain vanilla [App](app) (free trial)
4. You are a familiar using the shell and Git



## Deploying

The following code gets executed on your local machine. The lines starting with a `$` are the ones you execute. Replace the Git clone URL with the one from your actual App. The following code examples include the Git clone URL, you'll need the one from your actual App. You get the Git clone URL in the Dashboard > Your App > Git.

```bash
# Clone the repo from fortrabbit
# Initialize Git by cloning the empty repo from fortrabbit
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
Cloning into '{{app-name}}'...
warning: You appear to have cloned an empty repository

# Get in position
# Change directory to your new local project
$ cd {{app-name}}

# Make a change
# Create an index php file with a bold messsage on the fly
$ echo '<?php echo "PHPower to the PHPeople";' > index.php

# Thanks for the add
# Add the change in the working directory to the staging area
$ git add index.php

# You did it, commit it
# Commit the staged snapshot to the project history
$ git commit -am 'initial commit'

# Le grand final
# Just push to deploy
$ git push -u origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 251 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)

Commit received, starting build of branch master

–––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––

B U I L D


Checksum:
  3ec951f4f4a59a42afa7e6bebaa78477672d3b6a

Deployment file:
  not found

Pre-script:
  not found
  0ms

Composer:
  not executing (composer.json missing)
  0ms

Post-script:
  not found
  0ms



R E L E A S E


Packaging:
  6ms

Revision:
  1446568383255266100.3ec951f4f4a59a42afa7e6bebaa78477672d3b6a

Size:
  182B

Uploading:
  134ms

Build & release done in 217ms, now queued for final distribution.

–––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––

To {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```
