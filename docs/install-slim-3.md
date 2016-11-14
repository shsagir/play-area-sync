---

template:         article
reviewed:         2016-11-14
title:            Install Slim Framework 3
naviTitle:        Slim Framework
lead:             Slim is a PHP micro framework that helps you write simple web applications and APIs quickly. Learn how to install and tune Slim 3 on fortrabbit.

group:            Install_guides
stack:            all

websiteLink:      http://www.slimframework.com/?utm_source=fortrabbit
websiteLinkText:  slimframework.com
category:         micro framework
image:            slim-logo.png
version:          3.1

---

## Get ready

We assume you've already created a new App. If not: You can do so in the [fortrabbit Dashboard](/dashboard).

Go to the Dashboard and [set the root path](/app#toc-set-a-custom-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>

## Install

Execute locally in your terminal:

```bash
# 1. Use Composer to create a local Symfony project named like your App
$ composer create-project slim/slim-skeleton {{app-name}}

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init .

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push changes to fortrabbit
$ git push -u fortrabbit master
```

That's it. You can see the welcome page in the browser:
[https://{{app-name}}.frb.io/](https://{{app-name}}.frb.io/)
