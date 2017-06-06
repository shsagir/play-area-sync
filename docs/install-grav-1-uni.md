---

template:         article
reviewed:         2017-06-06
title:            Install Grav
naviTitle:        Grav
lead:             Grav is a popular, free, file based CMS based on Twig & Markdown. Learn here how to install and tune Grav on fortrabbit.
group:            Install_guides

websiteLink:      https://getgrav.org?utm_source=fortrabbit
websiteLinkText:  getgrav.org
category:         CMS
image:            grav-symbol.png
version:          1.2.4
stack:            uni
proLink:          install-grav-1-pro

---

## Get ready

We assume you've already created an [App](app) and chose WordPress in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard).


## Quick start

Start by downloading the "latest Grav Core + Admin archive" from the [Grav website](https://getgrav.org/downloads) from the Grav website and unpack it locally. It will extract into a folder named `grav-admin`. Now copy the **contents** of the local `grav-admin` folder (not the folder itself) via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup. When that is done, your Grav installation is completed and you can visited it in the browser:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< Grav installation_
* [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) _< Grav admin_


## Advanced setup and migration

**Don't stop with a plain vanilla installation. Make it yours!** Check out the following topics if you have an existing Grav installation or if you would like to setup Grav so that you can run in a local development environment as well as in your fortrabbit App:

### Install plugins & themes

All the regular ways are supported: Use the Grav admin, use the command line or just unpack the plugins and themes to the appropriate directories.

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


<!-- TODO: write something on how to keep GRAV up date with local dev … link SYNC section of SFTP article -->
