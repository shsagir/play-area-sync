---

template:      article
reviewed:      2016-10-24
title:         Install WordPress 4
naviTitle:     WordPress
group:         Install_guides
stack:         universal
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

This is the fast way to start with a fresh installation, please look below for [migrating WordPress](migration).

### 1. Download & unpack

Download the [latest.zip](https://wordpress.org/latest.zip) from the WordPress website and unpack the archive locally. It will extract into the folder `wordpress`.


### 2. Connect to your fortrabbit App via SFTP

<!--  TBD: maybe just link to the SFTP connection article here? TMI -->

Open an SFTP client ([Cyberduck](https://cyberduck.io/), WinSCP …) and connect to your fortrabbit App via SFTP (not FTP) like so:

* **Server**: `deploy.{{region}}.frbit.com`
* **User Name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When successful, you will find yourself in the empty `htdocs` folder of your App. If you can't connect: check the [access methods article](/access-methods) for troubleshooting.


### 3. Upload

Now copy the contents (not the folder itself) from your local `wordpress` folder to your remote App. This can take some time as there are a lot of files, usually around 7 minutes. 

PSSST: You can also use [wget with SSH](#toc-install-wordpress-with-ssh) here to speed up this to 30 seconds.


### 4. Use the web installer

When the upload is finished, visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in your browser and commence with the guided web installation to finish the setup:

#### MySQL

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: [Lookup in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql)
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_
* **Table Prefix**: keep `wp_` or your choice

#### Site title & admin user

* **Site Title**: What your site is about, you can this change this later
* **Username**: set a user name for the admin
* **Password**: use the suggested password or choose a custom one
* **Your Email**: set an e-mail for the admin


### 5. Login to wp-admin

After this you will be redirected to the WordPress admin login form. Use the previously chosen Username and Password to login.

### 6. Done

Got it? Your WordPress is now live under:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< WordPress installation_
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/admin) _< WordPress admin_


## Migration

Don't stop with a plain vanilla installation. Make it yours! Please check out the following topics if you have an existing WordPress installation or if you would like to setup WordPress so that you can run in a local development environment as well as in your fortrabbit App:

### Configuring environment configuration

We use [environment detection](local-development#toc-environment-detection) to realize different behaviours based on the location: locally or on remote. Most importantly: the local WordPress should connect to the local database. To do so, we add different configurations for local development and remote production and add a switch so that WordPress can detect where it is currently running.

Check if the file `wp-config.php` exists on top level in your local WordPress installation. If it doesn't: create it by making a copy of `wp-config-sample.php` and name it `wp-config.php`. Now open `wp-config.php` in an editor and modify it using [environment variables](/env-vars) as following:

```php
/** The base configuration for WordPress */
/**                    grab the ENV var              or use local credentials  */

define('DB_NAME',      getenv('MYSQL_DATABASE')      ?: 'your-local-db-name');
define('DB_USER',      getenv('MYSQL_USER')          ?: 'your-local-db-user');
define('DB_PASSWORD',  getenv('MYSQL_PASSWORD')      ?: 'your-local-db-password');
define('DB_HOST',      getenv('MYSQL_HOST')          ?: 'localhost');
define('DB_CHARSET',  'utf8');
define('DB_COLLATE',  '');

/** Authentication Unique Keys and Salts. */
/**                        grab the ENV var           or  use local credentials  */

define('AUTH_KEY',         getenv('AUTH_KEY')         ?: 'unique local phrase');
define('SECURE_AUTH_KEY',  getenv('SECURE_AUTH_KEY')  ?: 'unique local phrase');
define('LOGGED_IN_KEY',    getenv('LOGGED_IN_KEY')    ?: 'unique local phrase');
define('NONCE_KEY',        getenv('NONCE_KEY')        ?: 'unique local phrase');
define('AUTH_SALT',        getenv('AUTH_SALT')        ?: 'unique local phrase');
define('SECURE_AUTH_SALT', getenv('SECURE_AUTH_SALT') ?: 'unique local phrase');
define('LOGGED_IN_SALT',   getenv('LOGGED_IN_SALT')   ?: 'unique local phrase');
define('NONCE_SALT',       getenv('NONCE_SALT')       ?: 'unique local phrase');

/** WordPress Database Table prefix. */
$table_prefix  = 'wp_';

/** For developers: WordPress debugging mode. */
define('WP_DEBUG', getenv('WP_DEBUG') ? true : false);

/* That's all, stop editing! Happy blogging. */

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

<!--

TBD: that's tooooo redundant and clear at this place, also this here is not step by step, more like loose topics.


### Upload WordPress to your fortrabbit App

Open an SFTP client ([Cyberduck](https://cyberduck.io/), WinSCP …) and connect to your fortrabbit App via SFTP (not FTP) like so:

* **Server**: `deploy.{{region}}.frbit.com`
* **User Name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When successful, you will find yourself in the empty `htdocs` folder of your App. If you can't connect: check the [access methods article](/access-methods) for troubleshooting.

-->

### Migrating the database

WordPress consists of it's files, the code and the uploads and of course the [MySQL database](/mysql-uni), were most contents are stored. There are various use cases to import and export the database:

* export the database from your old webhosting
* export your local database to import it to the fortrabbit database
* download the remote database on fortrabbit to bring your local installation up-to-date

#### Database migration with a GUI

TODO, TODO, TODO …

#### Database migration in the terminal

<!-- TODO: which direction is this? up and down or just down, what about the other way? -->

Create a dump of your existing database, eg using `mysqldump` from the command line:

```shell
$ mysqldump -uyour-local-db-user -pyour-local-db-password your-local-db-name > dump.sql
```

Now either open up your [MySQL GUI](/mysql-universal#toc-mysql-via-gui), connect your App's database and import the just created dump or open up a tunnel from the shell then import the dump from another shell terminal:

```shell
# open a tunnel
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com

# in a new terminal: import the dump
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < dump.sql
```

**Note**: You will be asked to enter your [App's database password, which you can find in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql).

<!--

TBD: again redundant

### Test

Your are good to go. Your WordPress is now live under:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< WordPress installation_
* [{{app-name}}.frb.io/wp-admin](https://{{app-name}}.frb.io/admin) _< WordPress admin_

-->

### Developing WordPress

Continuous development of a WordPress site has different requirements than first time setup. Speed and certainty of deployed code are of utmost importance.

You can use SFTP to upload your code modifications simple enough and does not need further explanation: Just upload your changed files. The advantage is that it's easy as pie. The disadvantage is that it's slow and one can get confused about which changes are online and and which are not.

Instead of manually uploading code files changes one by one, you can also: 

#### Syncing code with SFTP

Most SFTP clients are featuring a file synchronization mode. You can choose your local folder and sync that up or down against the remote one (on fortrabbit). All files get compared and newer ones get uploaded or downloaded, depending on the direction you choose.

This SFTP syncing might also be possible with your code editor of choice: Coda and Dreamweaver are supporting this out of the box, for Sublime Text you can find plugins.

SFTP syncing is not perfect and not fast, but it works well in most cases.

#### Syncing code with RSync

The command line tool `rsync` grants a fast and reliable way to upload your code changes. As the name implies, `rsync` is made to synchronize (two) data sets and that is exactly what it does. Following an example showcasing development on a custom plugin in the `wp-content/plugins` folder:

```shell
$ rsync -az --delete custom-plugin/ {{app-name}}@deploy.{{region}}.frbit.com:~/wp-content/plugins/custom-plugin/
```

The above command must be executed from within the `plugins` folder of your local WordPress installation. It assures that the remote folder `custom-plugin` contains exactly what your local folder of the same name contains. Mind that you can omit the `--delete` flag, which then makes sure no files in the remote folder will be deleted - even if they do not exist (anymore).

#### Deployng WordPress with Git

WordPress itself has not arrived in the new PHP age in terms of using the latest technologies and paradigms. So — we do not recommend to use the standard WordPress with Git and Composer. But there is a super-cool WordPress boilerplate called Bedrock. Please see our [WordPress install guide for the Profressional stack](/install-wordpress-4-pro) on how to set it up — it best works with the Professional Stack, but can also be used on the Universal Stack.


### Add a custom domain

Your App URL `{{app-name}}.frb.io` is the first address your WordPress can be reached. Later on, when you go live, you will add your own custom external domains. Here is the basic setup:

1. Register and route the domain to the fortrabbit App < see [domain article](/domains)
2. [Connect the domain to your fortrabbit App](/domains#toc-connect-your-domain-to-fortrabbit), so that requests for a new domain will be delegated to your App
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

Now proceed with the web installer as shown above.

