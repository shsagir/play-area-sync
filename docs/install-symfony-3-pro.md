---

template:         article
reviewed:         2017-08-28
title:            Install Symfony
naviTitle:        Symfony
lead:             Symfony has been around for some while — but it doesn't look old. Learn how to install and tune Symfony 2 or 3 on fortrabbit.

group:            Install_guides
stack:            pro
uniLink:          install-symfony-3-uni

websiteLink:      http://symfony.com/?utm_source=fortrabbit
websiteLinkText:  symfony.com
category:         framework
image:            symfony-mark.png
version:          3.1, 3.2, 3.3

---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


### Set the Apps root path


Also, if you haven't already (the stack chooser does that for you) — in the fortrabbit [Dashboard](/dashboard): [Set the root path](/app#toc-root-path) of your App's domains to **web**. This applies to all domains, either the App URL or your external domains.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)
</div>


### Set ENV vars

We assume you want to use the environment `prod` - fortrabbit is for production. So setup the ENV as in the [environment variable](/env-vars) in the Dashboard:

```
SYMFONY_ENV=prod
```

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>



## Install

For a new Symfony installation execute the commands following in your local terminal:

```bash
# 1. Use Composer to create a local Symfony project named like your App
$ composer create-project symfony/framework-standard-edition {{app-name}} "3.1.*"

# 2. Change into the folder
$ cd {{app-name}}

# 3. Initialize a local Git repo
$ git init .

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push changes to fortrabbit
$ git push -u fortrabbit master
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool! This first push can take a bit, since all the Composer packages need to be installed. When the push is done you can visit your App URL in the browser and see the Symfony welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Tune

Until now this is a vanilla Symfony. It needs some more tinkering to make it yours.

### MySQL

Use [App secrets](secrets) to attain database credentials. Modify the `app/config/parameters_prod.php` in your editor like so:

```php
// on fortrabbit: construct credentials from App secrets
$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);

$container->setParameter('database_driver', 'pdo_mysql');
$container->setParameter('database_host', $secrets['MYSQL']['HOST']);
$container->setParameter('database_name', $secrets['MYSQL']['DATABASE']);
$container->setParameter('database_user', $secrets['MYSQL']['USER']);
$container->setParameter('database_password', $secrets['MYSQL']['PASSWORD']);
```

Now make sure you have the above config included, in `app/config_prod.yml:

```yaml
imports:
    - { resource: config.yml }
    - { resource: parameters_prod.php }
```



### Use app_dev.php

If you want to use the development environment, you must modify `web/app_dev.php`. A simple example would be to replace the block, responding with a 403 like so:

```
if (isset($_SERVER['APP_NAME']) && $_SERVER['APP_NAME'] === '{{app-name}}') {
    // allow
} elseif (isset($_SERVER['HTTP_CLIENT_IP'])
    || isset($_SERVER['HTTP_X_FORWARDED_FOR'])
    || !(in_array(@$_SERVER['REMOTE_ADDR'], array('127.0.0.1', 'fe80::1', '::1')) || php_sapi_name() === 'cli-server')
) {
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
}
```

This way you can easily decide per App whether you want to allow the dev mode or not.


### Logging

Per default Symfony logs to a file. Modify the `app/config/config_prod.yml` file to redirect it to PHP's `error_log()`:

``` yaml
monolog:
    # ..
    handlers:
        # ..
        nested:
            type:  error_log
            # ..
```

You can safely remove `path` from the `nested` block as well. You can now use the regular [log access](logging-pro) to view the stream.

### Migrate & other database commands

You can [execute remote commands via SSH](/remote-ssh-execution-pro), for example:

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php bin/console doctrine:migrations:generate
```

### Object Storage

fortrabbit Apps have [ephemeral storage](quirks#toc-ephemeral-storage). If you require a permanent storage, for user uploads or any other runtime data your App creates, you can use our [Object Storage Component](/object-storage). Once you have booked the Component in the Dashboard the credentials will automatically become available via the [App secrets](/secrets).

* [Gaufrette Symfony bundle](https://github.com/KnpLabs/KnpGaufretteBundle)
* [Flysystem Symfony bundle](https://github.com/1up-lab/OneupFlysystemBundle)

Both are well documented. In essence, you should configure your filesystem abstraction so that you use the cloud storage adapter in your prod environment (on fortrabbit) and locally, well, a local adapter.


### Memcache

If you are using a PHP production plan, then your App is distributed over multiple nodes. This means you need a network capable caching: [Memcache](memcache-pro).

Open `app/config/parameters_prod.php` again and add:

```php
// Read secrets, unless you do alrady in this file:
//$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);

$container->setParameter('session_memcache_expire', getenv('SESSION_MEMCACHE_EXPIRE') ?: 86400);
$container->setParameter('session_memcache_prefix', getenv('SESSION_MEMCACHE_PREFIX') ?: 'ez_');
$container->setParameter('session_memcache_host_1', $secrets['MEMCACHE']['HOST1']);
$container->setParameter('session_memcache_port_1', $secrets['MEMCACHE']['PORT1']);
if (isset($secrets['MEMCACHE']['HOST2'])) {
    $container->setParameter('session_memcache_host_2', $secrets['MEMCACHE']['HOST2']);
    $container->setParameter('session_memcache_port_2', $secrets['MEMCACHE']['PORT2']);
}
```

Now open up `app/config/config_prod.yml` and make use of the parameters:

```
framework:
    session:
        handler_id: session.handler.memcached

services:
    session.memcached:
        class: Memcached
        arguments: [ %session_memcache_prefix% ] 
        calls:
            - [ addServer, [ %session_memcache_host_1%, %session_memcache_port_1% ]]
            # if you are using a Memcache Production plan (with two nodes):
            #- [ addServer, [ %session_memcache_host_2%, %session_memcache_port_2% ]]

    session.handler.memcached:
        class:     Symfony\Component\HttpFoundation\Session\Storage\Handler\MemcachedSessionHandler
        arguments: ["@session.memcached", { prefix: %session_memcache_prefix%, expiretime: %session_memcache_expire% }]
```

### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but you can use the `SwiftmailerBundle` and configure it in your `app/config/config.yml` file. Make sure you set the [charset](encoding) to UTF-8:

```php
Swift_Preferences::getInstance()->setCharset('UTF-8');
```
