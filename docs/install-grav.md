---

template:         article
reviewed:         2016-08-22
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular free file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.1.3


tags:
    - php
    - install

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine. You also need a local Grav installation. You can either use an existing or a new one. This guide explains you how to start from scratch:

## Install

Open a local terminal session and execute:

```bash
# 1. Use Composer to install Grav in a folder named like your App
$ composer create-project getgrav/grav {{app-name}}

# 2. Move into the folder
$ cd {{app-name}}

# 3. Initialize Git
$ git init .

# 4. Add fortrabbit as a Git remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 5. Add all files to Git
$ git add -A

# 6. Commit changes
$ git commit -m 'Initial'

# 7. Push to deploy
$ git push -u fortrabbit master
```

Done. Your Grav site is now online and you can visit it in your browser!  
[https://{{app-name}}.frb.io](https://{{app-name}}.frb.io)


Apart from the above shown Composer installation you can also [download a zip package](https://learn.getgrav.org/basics/installation#option-1-install-from-zip-package) or [clone the Git repo](https://learn.getgrav.org/basics/installation#option-3-install-from-github). Advanced users might also consider to have separated repos for the Grav core, Plugins, Themes and the actual content in pages.

## Tuning

### Install theme

Grav supports themes and plugins, which both can be installed with the `bin/gpm` tool. Assuming you followed the [zip package based installation](http://learn.getgrav.org/basics/installation#option-1-install-from-zip-package), as shown above, installing plugins or themes is easy: Find the [theme you like](http://getgrav.org/downloads/themes), then install it as usual. Following an example. Run locally:

```bash
$ bin/gpm install [[soraarticle]]
```

Open `user/config/system.yaml` and set `pages/theme` to `soraarticle`. Now commit and push:

```bash
$ git add -A
$ git push -m 'New theme'
$ git push
```

Done.

### Install plugin

Find the [plugin you need](http://getgrav.org/downloads/plugins) then install it as usual (we use download here). Following an an example. Run locally:

```bash
$ php bin/gpm install [[simplesearch]]
```

Then configure the plugin, if needed (usually in `user/plugin/<pluginname>/<pluginname>.yaml`), commit and push:

```bash
$ git add -A
$ git push -m 'New plugin'
$ git push
```

Done.


### Grav Admin VS ephemeral storage

The popular [Grav Admin plugin](https://learn.getgrav.org/admin-panel/introduction) provides a way to manage a site in a guided web interface similar to the "wp-admin". This is great, when you want your client to manage her/his Grav site her/himself. Unfortunately it does not play well with ephemeral storage. Like with other PaaS — the local file system storage of the App will be wiped on each deploy — think 12-factor-app. So all local changes made in the Grav Admin panel will be gone.

Currently you should: use Grav on fortrabbit for your own private projects where you: write locally in your text editor, handle the pages in Git.
