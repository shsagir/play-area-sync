---

template:         article
reviewed:         2016-09-30
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular, free, file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.1.5
stack:            uni

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

## Advanced setup and migration

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing Grav installation or if you would like to setup Grav so that you can run in a local development environment as well as in your fortrabbit App:

### Install plugins & themes

All the regular ways are support: Use the Grav admin, use the command line or just unpack the plugins and themes to the appropriate directories.

### Environment configuration

Grav allows you to [configure different settings based on the domain](https://learn.getgrav.org/advanced/environment-config) it is served from. For example, if you are developing your Grav site locally, you probably want the debugging enabled while you want it to be disabled, when serving the App from fortrabbit.

To that end, you can create domain specific configurations. All you need to do is create a new `system.yaml` configuration file for you custom domain. Say the domain you are using is `www.my-grav.tld`, you would create the file `user/www.my-grav.tld/config/system.yaml`, in which you then set:

```yaml
debugger:
  enabled: false
```

An even better approach is set your `user/config/system.yaml` as restrictive as possible while using a special localhost configuration in `user/localhost/config/system.yaml`, which enables debugging and so on:

```yaml
debugger:
  enabled: true
```
