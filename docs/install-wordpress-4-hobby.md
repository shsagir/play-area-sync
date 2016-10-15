---

template:      article
reviewed:      2016-10-15
title:         Install WordPress 4
naviTitle:     WordPress
group:         Install_guides
stack:         hobby
lead:          Learn how to install and tune the popular blogging and CMS engine WordPress 4 on fortrabbit.
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

You should have already created an [App](app). You can do so in the [fortrabbit Dashboard](/dashboard).

<!-- TODO:

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

-->


## Quick start

### 1. Download & unpack

Download the [latest.zip](https://wordpress.org/latest.zip) from the WordPress website and unpack the archive locally. You will see a `wordpress folder.

### 2. Connect to your fortrabbit App via SFTP

Open an SFTP client ([Cyberduck](https://cyberduck.io/), WinSCP …) and connect to your fortrabbit App via SFTP (not FTP) like so:

* **Server**: `deploy.{{region}}.frbit.com`
* **User Name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}

When successfull, you will see the yet empty `htdocs` folder of your fortrabbit App. If you can't connect: check the [access methods article](/access-methods) to troubleshoot.


### 3. Upload

Now mode all the contents (not the folder itself) of your local `wordpress` folder to your remote fortrabbit App. This can take some time as there a lot of files.


### 4. Use the web installer

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup like so:

#### MySQL

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{app-database-password}}` _[< how to recover](/mysql-hobby#toc-obtain-the-mysql-password)_
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **Table Prefix**: keep `wp_` or your choice

#### Site title & admin user

<!-- TODO: explain this, App-Name not helpful in long run, see below for routing domains  -->
<!-- TODO: Note: e-mails will never arrive? — the web installer tries to send an e-mail! -->

* **Site Title**: — you can change this later on
* **Username**: set a an admin user name
* **Password**: use the suggested secure password or choose a custom one
* **Your Email**: set an e-mail for the admin


### 5. Login to wp-admin

After that you will be directed to login to the WordPress admin. Use the Username and the Password you have set in the previous step to login here now. 

### 6. Done

Got it? You can start WordPressing. Your WordPress is now life under:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) < WordPress installation
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/admin) < WordPress admin

- - -

## Tune

Don't stop with a plain vanilla installation. Make it yours!

### Add your own domain

Your App URL `{{app-name}}.frb.io` is the first address your WordPress can be reached. Later on, when you go live, you will add your own custom external domains. Here is the basic setup:

1. Register and point the domain to the fortrabbit App < see [domain article](/domains)
2. Tell the fortrabbit App that requests for a new domain will come in
3. Once, the domain is routed, tell WordPress to use the new domain as well:

In the WordPress admin change the Site URL from your App URL to that new domain. Find this setting in wp-admin under Settings > General: "WordPress Address (URL)" and "Site Address (URL)". Change this to your new domain. More advanced help regarding domains and the vars `home_url` and `site_url` can be found in the Wordpress help here:

* [Changing The Site URL](https://codex.wordpress.org/Changing_The_Site_URL)


### Install themes & plugins

This is pretty standard operations. You can download and update themes directly from the WordPress admin. Or you can create your own, test them locally, then upload them to your remote themes folder. The same applies to plugins.


### Keep WordPress secure

Urgent security advice: WordPress is popular with hackers. You are responsible to keep the software you install up-to-date — see our [security guidelines](/security). The good news is that WordPress has automatic background updates and they are enabled by default. Please check this article:

* [WordPress help: Configuring Automatic Background Updates](https://codex.wordpress.org/Configuring_Automatic_Background_Updates)

and take care that your WordPress core, plugins and even the themes are always up-to-date.


### Send e-mails

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for the `wp_mail()` function.



- - -

## WordPress development

So far we saw how to setup WordPress in the cloud. But WordPress development means more than installing a WordPress, it means developing a theme and configuring WordPress. This is best done locally. 

You want to work with a single WordPress codebase that will function in two different environments: locally and on fortrabbit. The transistion between the two environments should be seamless. 

The following section shows you how to set up WordPress in a way

The first thing to get started, is to make sure you have a [PHP development environment](/local-development) running on your local machine.


#### MySQL configuration via ENV vars

The main issue you run into with Wordpress is having to connect to two different databases. 

We recommend using environment variables in place of hard-coding connection credentials. Your fortrabbit App comes with automatically generated environment variables for MySQL.

TODO: wp-config Example here (…)


#### Domain configuration

… TODO: how set up local WordPress so it shows up under "mydomain.dev"


#### Deploying changes

… TODO: Sync? resync? SFTP sync programm? how?


#### MySQL export & import

… TODO: how to upload the database …



## Migrating an excisting WordPress

Have a WordPress hosted somewhere else? This here is how to move your WordPress from one host to another.

– TODO: Database export/import / File transfer




## Advanced topics


### Run WordPress in a sub folder

There are two reasons to install wordpress not in the `htdocs` but in a sub directory:

1. WordPress is just the blog-part of the website: `mydomain.com/blog`
2. You want to run multiple WordPress in one App. [Please don't.](/apps)

You can achieve the first option by putting WordPress in a folder and by changing the "Site Address URL" parameter (see above).


* [Giving WordPress Its Own Directory](https://codex.wordpress.org/Giving_WordPress_Its_Own_Directory)



### Install WordPress with SSH

This is an alternative much quicker and more advanced way to install WordPress via shell commands. Issue the following in your local terminal:

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

Now please proceed with the web installer as shown above.


### Install WordPress with Git

WordPress itself has not arrived in the new PHP age in terms of using the latest technologies and paradigms. So — we do not recommend to use the standard WordPress with Git and Composer. But there is a super-cool WordPress boilerplate called Bedrock. Please see our WordPress install guide on how to set it up — it best works for our Professional stack but can also be used on the Hobby stack.


### Database configuration with wp-config

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

