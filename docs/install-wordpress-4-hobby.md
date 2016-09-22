---

template:      article
reviewed:      2016-09-15
title:         Install WordPress 4
naviTitle:     WordPress
lead:          "WordPHPress is PHPowering much of the web. Learn here how to install and tune the popular blogging and CMS engine WordPress 4 on fortrabbit."
group:         Install_guides
stack:         hobby
proLink:       install-wordpress-4-pro
oldLink:       install-wordpress-4-old
deprecated:    0

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          4.6

otherVersionLinks:
    - install-wordpress-4-pro

tags:
    - php
    - install
    - wordpress

keywords:
    - wp-admin

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Install WordPress with SFTP

The most straight forward way to install WordPress is by downloading it to your local machine and then upload it to your fortrabbit App:

1. Download [the latest](https://wordpress.org/latest.zip) archive
2. Unpack the archive locally
3. Open an SFTP client
4. Connect to your fortrabbit App (access credentials)
3. Upload the **contents** of the local `wordpress` to the remote `htdocs` folder

### Configure in the browser

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation. In the database form enter:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{your-database-password}}`
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Database Prefix**: keep `wp_` or your choice

Following enter your site name, admin user, e-mail and password.

After that you should be done for now, you can visit:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) < WordPress installation
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/admin) < WordPress admin

## Tune

Don't stop with a plain vanilla installation. Make it yours!

### Install themes & plugins

This is pretty standard operations. You can download and update themes directly from the WordPress admin. Or you can create your own, test them locally, then upload them to your remote themes folder. The same applies to plugins.

### Send mails

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:

### Secure WordPress

Please mind that it is [your responsibility](/security) to keep your WordPress secure and up-to-date. We highly recommend to enable auto-updates.


### Add your own domain

Your App URL {{app-name}}.frb.io is the first address your WordPress can be reached. Later on, when you go live, you will add your own custom external domains. Please see our [domain article](/domains) on how to register a domain in the Dashboard.

In the WordPress admin you will also Site URL from your App URL to that URL. More advanced help regarding domains in the Wordpress help:

* https://codex.wordpress.org/Changing_The_Site_URL

- - -


## Install WordPress with SSH

This is an alternative quicker and more advanced way to install WordPress. Issue the following in your local terminal:

```bash
# 1. Login to your App via SSH
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com

# 2. Download latest Wordpress
$ wget https://wordpress.org/latest.tar.gz

# 3. Unpack wordpress
$ tar zxf latest.tar.gz

# 4. Move unpackged files to top level
$ mv wordpress/* .

# 5. Remove the downloaded container
$ rm -r wordpress latest.tar.gz
```

Now please proceed as [above](#toc-configure-in-the-browser).


## Install WordPress with Git

WordPress itself has not arrived in the new PHP age in terms of using the latest technologies and paradigms. So — we do not recommend to use the standard WordPress with Git and Composer. But there is a super-cool WordPress boilerplate called Bedrock. Please see our WordPress install guide on how to set it up — it best works for our Professional stack but can also be used on the Hobby stack.