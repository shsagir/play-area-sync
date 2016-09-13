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

WordPress itself has not really arrived in the new PHP age in terms of using the latest technologies and paradigms. [Bedrock](https://roots.io/bedrock/) allows you to install WordPress easily in a modern infrastructure, such as fortrabbit.

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

# 5. Cleanup
$ rm -r wordpress latest.tar.gz
```

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the installation. In the database dialog enter:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{your-database-password}}`
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Database Prefix**: keep `wp_` or your choice

Following enter your Site name, admin user, email and password.

Done

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
