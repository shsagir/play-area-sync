---

template:      article
reviewed:      2016-10-14
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
version:          4.6.1

keywords:
    - wp-admin

---

<!-- TODO: restructure -->


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Install WordPress with SFTP

The most straight forward way to install WordPress is by downloading it to your local machine and then upload it to your fortrabbit App:

1. Download [the latest](https://wordpress.org/latest.zip) archive
2. Unpack the archive locally
3. Open an SFTP client
4. Connect to your fortrabbit App (access credentials)
3. Upload the **contents** of the local `wordpress` to the remote `htdocs` folder

## Configure in the browser

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation:

### MySQL

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{app-database-password}}` _[< how to recover](/mysql-hobby#toc-obtain-the-mysql-password)_
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **Table Prefix**: keep `wp_` or your choice

### Site title & admin user

<!-- TODO: explain this, App-Name not helpful in long run, see below for routing domains  -->
<!-- TODO: Note: e-mails will never arrive? — the web installer tries to send an e-mail! -->

* **Site Title**: — you can change this later on
* **Username**: set a an admin user name
* **Password**: use the suggested password or role your own
* **Your Email**: set an e-mail


## Test it!

After that you should be done for now, you should be redirected to login the WordPress admin. Your WordPress is now life under:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) < WordPress installation
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/admin) < WordPress admin

- - -

## Tune

Don't stop with a plain vanilla installation. Make it yours!

### Install themes & plugins

This is pretty standard operations. You can download and update themes directly from the WordPress admin. Or you can create your own, test them locally, then upload them to your remote themes folder. The same applies to plugins.

### Send e-mails

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for the `wp_mail()` function.


### Secure WordPress

Please mind that it is [your responsibility](/security) to keep your WordPress secure and up-to-date. We highly recommend to enable auto-updates.


### Add your own domain

Your App URL `{{app-name}}.frb.io` is the first address your WordPress can be reached. Later on, when you go live, you will add your own custom external domains. Here is the basic setup:

1. Register and point the domain to the fortrabbit App < see [domain article](/domains)
2. Tell the fortrabbit App that requests for a new domain will come in
3. Tell WordPress to use the new domain as well:

In the WordPress admin (https://{{app-name}}.frb.io/wp-admin) you will change the Site URL from your App URL to that new domain. You'll find this setting in wp-admin under Settings > General: "WordPress Address (URL)" and "Site Address (URL)" should be changed to your new domain. More advanced help regarding domains in the Wordpress help can be found here:

* [Changing The Site URL](https://codex.wordpress.org/Changing_The_Site_URL)

### Run WordPress in a sub folder

There are two reasons to install wordpress not in the `htdocs` but in a sub directory:

1. WordPress is just the blog-part of the website: `mydomain.com/blog`
2. You want to run multiple WordPress in one App. [Please don't.](/apps)

You can achieve the first option by putting WordPress in a folder and by changing the "Site Address URL" parameter (see above).


* [Giving WordPress Its Own Directory](https://codex.wordpress.org/Giving_WordPress_Its_Own_Directory)


### Keep WordPress secure

Urgent security advice: WordPress is popular with hackers. You are responsible to keep the software you install up-to-date — see our [security guidelines](/security). The good news is that WordPress has automatic background updates and they are enabled by default. Please check this article:

* [WordPress help: Configuring Automatic Background Updates](https://codex.wordpress.org/Configuring_Automatic_Background_Updates)

and take care that your WordPress core, plugins and even the themes are always up-to-date.


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

## Database configuration with wp-config

A freshly downloaded WordPress contains a `wp-config-example.php` file on root level. you can make a copy and rename that to `wp-config.php` so that it will be used. This is what the MySQL database part must look like to work with your fortrabbit App:


```
// other code …

// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', '{{app-name}}');

/** MySQL database username */
define('DB_USER', '{{app-name}}');

/** MySQL database password */
define('DB_PASSWORD', '{{your-database-password}}');

/** MySQL hostname */
define('DB_HOST', '{{app-name}}.mysql.{{region}}.frbit.com');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');

// other code …
```



<!-- TODO:


## Local development

BLA BLA BLA … environment detection



## Routing your own domains

BLA BLA … "home_url" "site_url" …

-->