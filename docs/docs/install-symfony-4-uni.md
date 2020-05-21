---

template:         article
reviewed:         2019-09-11
title:            Install Symfony 4
naviTitle:        Symfony
lead:             Symfony has been around for some while — but it doesn't look old. Learn how to install and tune Symfony 4 on fortrabbit.

group:            Install_guides
stack:            uni
proLink:          install-symfony-4-pro

websiteLink:      https://symfony.com
websiteLinkText:  symfony.com
category:         framework
image:            symfony-mark.png
version:          4.0

otherVersions:
  3 : install-symfony-3-uni


---


## Get ready

We assume you've already created a new App and chose Symfony in the [Software Preset](app#toc-software-preset). If not: You can do so in the [fortrabbit Dashboard](/dashboard). You should also have a [PHP development environment](/local-development) running on your local machine.


### Root path

If you haven't chosen Symfony when creating the App in the Dashboard at first, please set the following: Go to the Dashboard and [set the root path](/app#toc-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>

### ENV vars

Symfony 4 [environments](https://symfony.com/doc/current/configuration/environments.html#executing-an-application-in-different-environments) are controlled by the `APP_ENV` ENV var. You can even use ENV vars in the .yml configurations now. To get you started quickly, we provide you with the following ENV vars by default when you have chosen the Symfony from when creating the App:

```osterei32
APP_ENV=prod
APP_DEBUG=0
APP_SECRET=ClickToGenerate
DATABASE_URL=mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@${MYSQL_HOST}/${MYSQL_DATABASE}
```

Within the Dashboard under your App settings you can modify the ENV vars. Modify `APP_DEBUG` or `APP_ENV` to change the behavior of your application. You can also define your own key-value-pairs and use them in your configuration files.

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>


## Quick start

We assume that you already have [Symfony installed locally](http://symfony.com/download). If your project is not under Git version control yet, follow these steps to be prepared for your first `git push`.

```bash
# 1. Pull the .htaccess file via flex
$ composer req symfony/apache-pack

# 2. Initialize a local Git repo
$ git init .

# 3. Add all files
$ git add -A

# 4. Commit files for the first time
$ git commit -m 'Initial'

# 5. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 6. Initial push and set upstream
$ git push -u fortrabbit master
# Lot's of output

# 7. From there on only
$ git push
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! This first push can take a bit, since all the Composer packages need to be installed. When the first push is done you can visit your App URL in the browser and see the Symfony welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## MySQL

Until now you just deployed some code. If you want to use doctrine and Mysql it requires some more tinkering to make it yours.

### Configuration

The `DATABASE_URL` ENV var stores all DB access information already (see [above](#toc-env-vars)). You just need to use it in `doctrine.yaml` like so:  

```yaml
doctrine:
    dbal:
        # All in one single line (default)
        url: '%env(DATABASE_URL)%'

```

### Doctrine & symfony console

Once doctrine is configured and the changes are deployed, you may want to create the DB schema, run migrations or load fixtures. You can login via [ssh](ssh) in to your App, or instead just fire single commands like so:

```bash
# doctrine
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console doctrine:schema:create'
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console doctrine:fixtures:load'

# cache clear
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com 'php bin/console cache:clear'
```

You can also add this migrate command to your `composer.json` to have it run automatically every time you push changes.

```json
"scripts": {
    "post-install-cmd": [
        "php bin/console doctrine:migrations:migrate",
    ],
}
```

With that in place, any time you deploy your code, database changes will be applied immediately. If no database changes are required, nothing happens, so it is safe to run all the time. Just make sure to test your upgrades and migrations locally first.

## Webpack Encore

We assume you are using Encore to manage your CSS & JS assets and `yarn` is configured in the local environment already.

### Configuration

In your `webpack.config.js` define different locations of the build - `prod` and `dev`:

```js
var Encore = require('@symfony/webpack-encore');
var env = Encore.isProduction() ? 'prod' : 'dev';

Encore
    .setOutputPath('public/build/' + env)
    .setPublicPath('/build/' + env)
    .setManifestKeyPrefix('build')
    // ...
    .enableVersioning(true)
;

module.exports = Encore.getWebpackConfig();
```

Tell the application where to find the `manifest.json` - again for `prod` and `dev`:

```yml
# config/package/prod/framework.yaml
framework:
    assets:
        json_manifest_path: '%kernel.project_dir%/public/build/prod/manifest.json'
```

```yml
# config/package/dev/framework.yaml
framework:
    assets:
        json_manifest_path: '%kernel.project_dir%/public/build/dev/manifest.json'
```

### Deploying assets

Compiled assets should not be under version control. So, instead of committing the build files to Git, you deploy them separately. rsync works great for this and it's easier than you might think:

```bash
# Build production assets locally
$ yarn run encore production

# Deploy the build/prod folder
$ rsync -av ./public/build/prod/ {{app-name}}@deploy.{{region}}.frbit.com:~/public/build/prod/
```


## Advanced configurations

Still reading? Let's go on:

### Logging

You can access all log files your App writes on the file system. If you want to use [live logging](logging#toc-live-log-access), then you should configure Symfony to use `error_log`. Modify the `config/packages/prod/framework.yml` file:

``` yml
monolog:
    # ..
    handlers:
        # ..
        nested:
            type:  error_log
            # ..
```

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use the `Swiftmailer`. Configure it in your `config/packages/swiftmailer.yaml` file and provide the `MAILER_URL` ENV var in Dashboard.

``` yml
swiftmailer:
    url: '%env(MAILER_URL)%'
    spool: { type: 'memory' }
```
