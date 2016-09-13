---

template:      article
reviewed:      2016-09-13
title:         Install WordPress 4 (Hobby)
naviTitle:     WordPress
lead:          TODO
group:         Install_guides
dontList:      true

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          4.6

otherVersionLinks:
    - install-wordpress-4
    - install-wordpress-old-app

externalLinks:
    - https://codex.wordpress.org/Installing_WordPress

tags:
    - php
    - install
    - wordpress

seeAlsoLinks:
    - app

---

## Get ready

We assume you've already created an [Hobby App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Install

WordPress installation is straight forward and simple:

1. Visit the [WordPress](https://wordpress.org) homepage
2. Download [the latest](https://wordpress.org/latest.zip) archive.
3. Unpack the archive locally
4. Upload the contents of the unpacked `wordpress` folder via SFTP to your App

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation. In the database dialog enter:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{your-database-password}}`
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Database Prefix**: keep `wp_` or your choice

Following enter your site name, admin user, email and password.

Done

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
