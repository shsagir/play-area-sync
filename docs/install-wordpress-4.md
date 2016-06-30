---

template:         article
reviewed:         2016-06-30
title:            Install WordPress 4
naviTitle:        WordPress
lead:             WordPHPress is PHPowering much of the web. Learn here how to install and tune the popular blogging and CMS engine WordPress 4 on fortrabbit.
group:            Install_guides

websiteLink:      http://wordpress.org/?utm_source=fortrabbit
websiteLinkText:  wordpress.org
category:         CMS
image:            wordpress-mark.png
version:          4.4

otherVersionLinks:
    - install-wordpress-old-app

tags:
    - php
    - install
    - wordpress

seeAlsoLinks:
    - app

---

## Get ready

We assume you've already created an [App](app) with fortrabbit. 


## Install

WordPress itself has not really arrived in the new PHP age in terms of using the latest or even recent technologies and paradigms. But do not fear: [Bedrock](https://roots.io/bedrock/) comes to the rescue. Bedrock is a "WordPress boilerplate with modern development tools, easier configuration, and an improved folder structure" and allows you to install WordPress easily in a modern infrastructure, such as fortrabbit.

Let's get started by installing Bedrock locally:

```bash
# create a new App based on bedrock locally
$ cd ~/Projects
$ git clone https://github.com/roots/bedrock MyApp

# add your App's remote, so you can push later on
$ cd MyApp
$ git remote add fortrabbit git@deploy.eu2.frbit.com:your-app.git

# install all required composer packages
$ composer install
```


### Setup WordPress authentication unique keys and salts

Now head over to the [Dashboard](dashboard), [set the document root](/domains#toc-set-a-custom-root-path) of your App's domains to `web` and then add the following [App secrets](secrets):

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

It is recommended to use different random strings.

### Set the domain

The last configuration in the Dashboard is to set the WordPress URLs and environment as [environment variables](env-vars). With `Dashboard > App > Settings > ENV vars` you enter the following in the textarea and save.

```
WP_ENV=production
WP_HOME=http://your-app.eu2.frb.io
WP_SITEURL=http://your-app.eu2.frb.io/wp
```

Replace `your-app.eu2` with the App URL (see Dashboard). You can also use your own domain here of course – just take care to also [route the domain](about-domains) then. The folder `wp` in the WP_SITEURL is Bedrock-specific. You can also use the encrypted `https` protocol. 


### Database setup 

Now open up the main config file in `config/application.php` and the following at the very top of the file:

```php
$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
```

In the same file, go to the `DB settings` section and replace the `getenv("DB_*")` calls with access to the MySQL secrets:

```php
define('DB_NAME', $secrets['MYSQL']['DATABASE']);
define('DB_USER', $secrets['MYSQL']['USER']);
define('DB_PASSWORD', $secrets['MYSQL']['PASSWORD']);
define('DB_HOST', $secrets['MYSQL']['HOST']);
```

Then do the same for the `Authentication Unique Keys and Salts` section:

```php
define('AUTH_KEY', $secrets['CUSTOM']['AUTH_KEY']);
define('SECURE_AUTH_KEY', $secrets['CUSTOM']['SECURE_AUTH_KEY']);
define('LOGGED_IN_KEY', $secrets['CUSTOM']['LOGGED_IN_KEY']);
define('NONCE_KEY', $secrets['CUSTOM']['NONCE_KEY']);
define('AUTH_SALT', $secrets['CUSTOM']['AUTH_SALT']);
define('SECURE_AUTH_SALT', $secrets['CUSTOM']['SECURE_AUTH_SALT']);
define('LOGGED_IN_SALT', $secrets['CUSTOM']['LOGGED_IN_SALT']);
define('NONCE_SALT', $secrets['CUSTOM']['NONCE_SALT']);
```

Before visiting your site, git push all the changes:

``` bash
$ git commit -am "Initial"
$ git push -u fortrabbit master
```

Finally you can visit your App in the browser and follow the WordPress setup. Done.


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
$ cd ~/Projects/MyApp/web/app/plugins
$ wget http://downloads.wordpress.org/plugin/woocommerce.zip
$ unzip woocommerce.zip
$ rm woocommerce.zip
```

Now make sure that the WooCommerce plugin directory is excluded from the `.gitignore` (aka: un-ignored) by opening `~/Projects/MyApp/.gitignore` and adding a new line immediately after `!web/app/plugins/.gitkeep`:

```
# ...
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/woocommerce
# ...
```

Now you can add everything to Git and push it online:

```bash
$ cd ~/Projects/MyApp
$ git add -A
$ git commit -m "With WooCommerce"
$ git push
```

Once that's done you can head over your WordPress admin and activate the plugin as usual.

### Installing themes

Bedrock recommends to install themes by unpacking them into the `web/app/themes` folder and to use Composer only in [rare cases](https://roots.io/bedrock/docs/composer/#themes).

### Persistent storage

Since WordPress is a CMS living on editorial provided content you most likely need a persistent storage. That's not so hard: you can use our [Object Storage component](/object-storage). Once you have booked the component in the Dashboard the credentials will automatically become available via the [App secrets](/secrets).

Now you need to install two plugins. Best do it with composer:

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
# ...
web/app/plugins/*
!web/app/plugins/.gitkeep
!web/app/plugins/amazon-s3-fortrabbit.php
# ...
```

Now commit everything and push it online:

```bash
$ git commit -am "With Object Storage"
$ git push
```

Once the deployment is done, you can head over to the WordPress Admin, activate all three plugins ("Amazon S3 fortrabbit", "Amazon Web Services" and "WP Offload S3 (Lite)") and then navigate to AWS > "S3 and CloudFront". First choose your bucket (there will be only one with the same name as your App), then enable "CloudFront or Custom Domain" and enter `your-app.objects.frb.io` as a custom domain. Save and done!






### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use a SMTP plugin like [WP SMTP](http://wordpress.org/plugins/wp-smtp/) or [MAIL SMTP](http://wordpress.org/plugins/wp-mail-smtp/) to enable SMTP support for your `wp_mail()` function:
