---

template:      article
reviewed:      2016-09-15
title:         Install WordPress 4 (Hobby)
naviTitle:     WordPress
lead:          WordPHPress is PHPowering much of the web. Learn here how to install and tune the popular blogging and CMS engine WordPress 4 on fortrabbit.
group:         Install_guides
dontList:      false
stack:         hobby

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          4.6

otherVersionLinks:
    - install-wordpress-professional

tags:
    - php
    - install
    - wordpress

seeAlsoLinks:
    - app
---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Install with SFTP

WordPress installation is straight forward and simple:

1. Download [the latest](https://wordpress.org/latest.zip) archive
2. Unpack the archive locally
3. Upload the contents of the unpacked `wordpress` folder via SFTP to your App

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation. In the database form enter:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{your-database-password}}`
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Database Prefix**: keep `wp_` or your choice

Following enter your site name, admin user, e-mail and password.


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:



---

## Install with SSH

This is 

### Download and unpack latest wordpress

Issue the following in your local terminal:

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

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the installation. In the database dialog enter:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{your-database-password}}`
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Database Prefix**: keep `wp_` or your choice

Following enter your Site name, admin user, email and password.

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
