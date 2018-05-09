---

template:         article
reviewed:         2017-01-20
title:            Install WordPress 4
naviTitle:        WordPress
lead:             WordPHPress is PHPowering much of the web. Learn here how to install and tune the popular blogging and CMS engine WordPress 4 on fortrabbit.
group:            Install_guides

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          4.6
stack:            pro
uniLink:          install-wordpress-4-uni
deprecated:       yes

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

### Set the root path

If you haven't already (the [Software Preset](app#toc-software-preset) does that for you) — in the Dashboard: [Set the root path](/app#toc-root-path) of your App's domains to **web**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### Set WordPress authentication unique keys and salts

If you haven't already (the [Software Preset](app#toc-software-preset) does that for you) — in the Dashboard, add the following [App secrets](secrets):

```osterei32
AUTH_KEY=LongRandomString
SECURE_AUTH_KEY=LongRandomString
LOGGED_IN_KEY=LongRandomString
NONCE_KEY=LongRandomString
AUTH_SALT=LongRandomString
SECURE_AUTH_SALT=LongRandomString
LOGGED_IN_SALT=LongRandomString
NONCE_SALT=LongRandomString
```

<div markdown="1" data-user="known">
[Add secrets for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/secrets/new)
</div>

### Set the domain

In the Dashboard, set the WordPress URLs and environment as [ENV vars](env-vars):

```
WP_ENV=production
WP_HOME=http://{{app-name}}.frb.io
WP_SITEURL=http://{{app-name}}.frb.io/wp
```

<div markdown="1" data-user="known">
[Go to my ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

We are using the App URL in this example. You can use your own domain here of course – just take care to also [route the domain](domains) then. The folder `wp` in the WP_SITEURL is Bedrock-specific. You can also use the encrypted `https` protocol.




## Install

WordPress itself has not really arrived in the new PHP age in terms of using the latest technologies and paradigms. [Bedrock](https://roots.io/bedrock/) allows you to install WordPress easily in a modern infrastructure, such as fortrabbit.

### Create WordPress locally with Bedrock

Issue the following in your local terminal:

```bash
# 1. Create a new App based on bedrock locally
$ git clone https://github.com/roots/bedrock {{app-name}}

# 2. Add your App's remote, so you can push later on
$ cd {{app-name}}
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 3. Install all required composer packages
$ composer install
```



### MySQL

In your editor, open up the file `config/application.php`

```php
// insert at the top of the file after "<?php"
// this will parse the the secrets.json file, when on fortrabbit
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
}

// other code …

/**
 * DB settings
 */
if (isset($_SERVER['APP_SECRETS'])) {
  define('DB_NAME', $secrets['MYSQL']['DATABASE']);
  define('DB_USER', $secrets['MYSQL']['USER']);
  define('DB_PASSWORD', $secrets['MYSQL']['PASSWORD']);
  define('DB_HOST', $secrets['MYSQL']['HOST']);
} else {
  define('DB_NAME', env('DB_NAME'));
  define('DB_USER', env('DB_USER'));
  define('DB_PASSWORD', env('DB_PASSWORD'));
  define('DB_HOST', env('DB_HOST') ?: 'localhost');
}
define('DB_CHARSET', 'utf8mb4');
define('DB_COLLATE', '');
$table_prefix = env('DB_PREFIX') ?: 'wp_';

// other code…
```

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.


### Authentication keys & salts

Again your editor, further down in the file `config/application.php`

```php
// other code…

/**
 * Authentication Unique Keys and Salts
 */
if (isset($_SERVER['APP_SECRETS'])) {
  define('AUTH_KEY', $secrets['CUSTOM']['AUTH_KEY']);
  define('SECURE_AUTH_KEY', $secrets['CUSTOM']['SECURE_AUTH_KEY']);
  define('LOGGED_IN_KEY', $secrets['CUSTOM']['LOGGED_IN_KEY']);
  define('NONCE_KEY', $secrets['CUSTOM']['NONCE_KEY']);
  define('AUTH_SALT', $secrets['CUSTOM']['AUTH_SALT']);
  define('SECURE_AUTH_SALT', $secrets['CUSTOM']['SECURE_AUTH_SALT']);
  define('LOGGED_IN_SALT', $secrets['CUSTOM']['LOGGED_IN_SALT']);
  define('NONCE_SALT', $secrets['CUSTOM']['NONCE_SALT']);
} else {
  define('AUTH_KEY', env('AUTH_KEY'));
  define('SECURE_AUTH_KEY', env('SECURE_AUTH_KEY'));
  define('LOGGED_IN_KEY', env('LOGGED_IN_KEY'));
  define('NONCE_KEY', env('NONCE_KEY'));
  define('AUTH_SALT', env('AUTH_SALT'));
  define('SECURE_AUTH_SALT', env('SECURE_AUTH_SALT'));
  define('LOGGED_IN_SALT', env('LOGGED_IN_SALT'));
  define('NONCE_SALT', env('NONCE_SALT'));
}

// other code …
```

### Deploy with Git

After you have [initialized and configured WordPress](#toc-create-wordpress-locally-with-bedrock) you can run these commands in the terminal to deploy:

``` bash
# 4. Add all changes to Git
$ git add -A

# 5. Commit changes
$ git commit -am "Initial"

# 6. Push to deploy
$ git push -u fortrabbit master
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, finally you can visit your App in the browser and follow the WordPress setup:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)

### Permalinks

WordPress creates an `.htaccess` file during setup on the fly. With [ephemeral storage](quirks#toc-ephemeral-storage) this will be destroyed during each deployment. We'll make the `.htaccess` permanent. To do so, open the hidden `.gitignore` file on top level and remove the .htaccess file from being ignored from Git by deleting or commenting the line like so:

```
# other code …

# WordPress
web/wp
#web/.htaccess
# the line above is no longer in use

# other code …
```

Next, create an `.htaccess` file with following contents and also place it on top level.

```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

Then again in the terminal, deploy the changes:

```
# 7. Add .htaccess & modified .gitignore to Git
$ git add -A

# 8. Commit changes
$ git commit -am "Added htaccess"

# 9. Push to deploy htaccess
$ git push
```

## Tuning

### Installing plugins

[Bedrock allows](https://roots.io/bedrock/docs/composer/#plugins) two ways to install plugins:  Use Composer to install plugins from [WordPress Packagist](http://wpackagist.org/); Just download and unpack the plugins into `web/app/plugins`

#### With Composer

This is the recommend way, since it already provides a simple upgrade mechanism (Composer, of course). Following is an example on how to install Akismet:

```bash
$ composer require wpackagist-plugin/akismet
$ git commit -am "With Akismet"
$ git push
```

Now you should enable the Plugin via the WordPress admin. Not all plugins are, as of yet, available via Wordpress Packagist, but the number is growing. Use their [search](http://wpackagist.org/) to check if the plugin you require is available.

#### Without Composer

If the plugin is not (yet) available via WordPress Packagist, you can still use the "old way" to install plugins. Following an example to install the WooCommerce plugin:

```bash
$ cd {{app-name}}/web/app/plugins
$ wget http://downloads.wordpress.org/plugin/woocommerce.zip
$ unzip woocommerce.zip
$ rm woocommerce.zip
```

Now make sure that the WooCommerce plugin directory is excluded from the `.gitignore` (aka: un-ignored) by opening `{{app-name}}/.gitignore` and adding a new line immediately after `!web/app/plugins/.gitkeep`:

```
# other code …
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/woocommerce
# other code …
```

Now you can add everything to Git and push it online:

```bash
$ cd {{app-name}}
$ git add -A
$ git commit -m "With WooCommerce"
$ git push
```

Once that's done you can head over your WordPress admin and activate the plugin as usual.

### Installing themes

Bedrock recommends to install themes by unpacking them into the `web/app/themes` folder and to use Composer only in [rare cases](https://roots.io/bedrock/docs/composer/#themes).

### Object storage

Since WordPress is a CMS living on editorial provided content you most likely need a persistent storage — but fortrabbit App have [ephemeral storage](/quirks#toc-ephemeral-storage). That's not a breaker: Use our [Object Storage](/object-storage). Once you have booked the Component in the Dashboard the credentials will automatically become available via the [App secrets](/secrets).

Now you need to install two plugins. Best do it with Composer in your local terminal:

```bash
$ composer require wpackagist-plugin/amazon-web-services
$ composer require deliciousbrains/wp-amazon-s3-and-cloudfront
```

Next create the file `web/app/plugins/amazon-s3-fortrabbit.php` with the following contents:

```
<?php
/*
Plugin Name: Amazon S3 fortrabbit
*/

$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
define('DBI_AWS_ACCESS_KEY_ID', $secrets['OBJECT_STORAGE']['KEY']);
define('DBI_AWS_SECRET_ACCESS_KEY', $secrets['OBJECT_STORAGE']['SECRET']);

add_filter('aws_get_client_args', function($args) use ($secrets) {
    $args['endpoint'] = 'https://'. $secrets['OBJECT_STORAGE']['SERVER'];
    return $args;
});
```

To make sure the plugin becomes part of the Git repo you need to open the `.gitignore` file and edit it like so:

```
# other code …
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/amazon-s3-fortrabbit.php
# other code …
```

Now commit everything and push it online:

```bash
$ git commit -am "With Object Storage"
$ git push
```

Once the deployment is done, you can head over to the WordPress Admin, activate all three plugins ("Amazon S3 fortrabbit", "Amazon Web Services" and "WP Offload S3 (Lite)") and then navigate to AWS > "S3 and CloudFront". First choose your bucket (there will be only one with the same name as your App), then enable "CloudFront or Custom Domain" and enter `{{app-name}}.objects.frb.io` as a custom domain. Save and done!




### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
