---

template:         article
reviewed:         2016-09-30
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular free file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.1.5
stack:            hobby

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine. You also need a local Grav installation. You can either use an existing or a new one. This guide explains you how to start from scratch:



## Install Grav with SFTP and SSH

[Grav](http://getgrav.org) runs pretty much out of the box. 

1. Visit the **[Grav download page](http://getgrav.org/downloads)**
2. Download the latest "Grav core + admin plugin"
3. Unpack the archive locally (it will extract into the sub-folder `grav-admin`)
4. Open an SFTP client
5. Connect to your fortrabbit App (access credentials)
6. Copy contents of the local `grav-admin` into the remote `htdocs` folder

### Configure in the browser

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation which will ask you to create an administrative user.

<!-- 

## Tuning

### Install theme

TODO: recommend SSH + execute or admin interface - or not mention it?

### Install plugin

TODO: recommend SSH + execute or admin interface - or not mention it

-->