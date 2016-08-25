---

template:         article
reviewed:         2016-08-24
title:            Install Phalcon 3
naviTitle:        Phalcon
lead:             Looking for sPHPeed? Phalcon is a web framework delivered as C extension providing high performance and low resource consumption. Here you learn how to best getting started with Phalcon 3 on fortrabbit.
dontList:         true
group:            Install_guides

websiteLink:      https://phalconphp.com/en/?utm_source=fortrabbit
websiteLinkText:  phalconphp.com
category:         framework
image:            phalcon-mark.png
version:          3.0.1

otherVersionLinks:
    - install-phalcon-2


otherVersionLinks:
    - install-phalcon-old-app

keywords:
    - phalconphp
    - framework
    - php extension
    - c extension
    - hhvm


tags:
    - php
    - install


---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

### Set Phalcon root path

Phalcon uses `public` as doc root, you need change that in the fortrabbit Dashboard under your Apps Domains settings.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>

### Enable Phalcon extension

As Phalcon is a C Extension, you need to enable it in the Dashboard under your Apps PHP Extensions.

## Install

Execute the following in your local terminal to start from scratch with a fresh new Phalcon installation on fortrabbit:

```bash
# 1. Change into your local phalcon folder
$ cd {{app-name}}

# 2. Initialize local Git repo
$ git init .

# 3. Make sure to keep the cache folder in Git
$ touch cache/.gitkeep

# 4. Add all files
$ git add -A

# 5. Commit files for the first time
$ git commit -m 'Initial'

# 6. Add fortrabbit as a remote
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 7. Push to fortrabbit
$ git push -u fortrabbit master
```

When it is done you can visit your App URL in the browser to see the Phalcon welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)


## Tune

Until now this is a vanilla Phalcon. Now, make it yours.

### MySQL

Use App [secrets](secrets) to attain database credentials. Open configuration file `app/config/config.php` and modify it directly after the `defined` statements like so:

```php
// ^^^ defined statements

// local MySQL credentials
$database = [
    'adapter'  => 'Mysql',
    'host'     => 'localhost',
    'username' => 'your-local-db-user',
    'password' => 'your-local-db-password',
    'dbname'   => 'your-local-db-name',
    'charset'  => 'utf8'
];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $database = [
        'adapter'  => 'Mysql',
        'host'     => $secrets['MYSQL']['HOST'],
        'username' => $secrets['MYSQL']['USER'],
        'password' => $secrets['MYSQL']['PASSWORD'],
        'dbname'   => $secrets['MYSQL']['DATABASE'],
        'charset'  => 'utf8',
    ];
}

return new \Phalcon\Config([
    'database'    => $database,
    'application' => [
        // do not change these contents
    ]
]);
```

#### MySQL access from local

Please see the [MySQL article](/mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.

### Memcache

Make sure you enabled the Memcache Component. Then you can use the [App Secrets](secrets) to attain your credentials.

#### Sessions

Open `app/config/services.php` and find the block starting with `$di->setShared('session', ...`. Replace it with the following:

```php
/**
 * Stash memcache servers into DI, so that they can be easily accessed by cache related services
 */
$di->setShared('memcache-servers', function() {
    if (isset($_SERVER['APP_SECRETS'])) {
        $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
        if (isset($secrets['MEMCACHE'])) {
            $servers = [];
            foreach (range(1, $secrets['MEMCACHE']['COUNT']) as $num) {
                $servers[]= [
                    'host' => $secrets['MEMCACHE']["HOST$num"],
                    'port' => $secrets['MEMCACHE']["PORT$num"],
                ];
            }
            return $servers;
        }
    }
    return [];
});

/**
 * Start the session the first time some component request the session service
 */
$di->setShared('session', function () {

    // on fortrabbit: construct credentials from App secrets
    if ($servers = $this->get('memcache-servers')) {
        $session = new \Phalcon\Session\Adapter\Libmemcached([
            'uniqueId'   => 'my-private-app',
            'servers'    => $servers,
            'lifetime'   => 3600,
            'client'     => [
                \Memcached::OPT_HASH      => \Memcached::HASH_MD5,
                \Memcached::OPT_PREFIX_KEY => 'prefix.',
            ],
            'prefix'     => 'my_'
        ]);
    }

    // locally: use the default session adapter
     else {
        $session = new SessionAdapter();
    }

    $session->start();

    return $session;
});
```

#### Backend Cache

Open up `app/config/services.php` again and add two new services:

```php
/**
 * Create the frontend cache the first time some component request the frontend cache service
 */
$di->setShared('frontend-cache', function() {
    return new \Phalcon\Cache\Frontend\Data([
        "lifetime" => 172800
    ]);
});

/**
 * Create the backend cache the first time some component request the backend cache service
 */
$di->setShared('backend-cache', function () {
    $frontendCache = $this->get('frontend-cache');

    // on fortrabbit: construct memcache credentials from App secrets
    if ($servers = $this->get('memcache-servers')) {
        $cache = new \Phalcon\Cache\Backend\Libmemcached($frontendCache, [
            'uniqueId'   => 'my-private-app',
            'client'     => [
                \Memcached::OPT_HASH      => \Memcached::HASH_MD5,
                \Memcached::OPT_PREFIX_KEY => 'prefix.',
            ],
        ]);
    }

    // locally: use the file cache adapter
     else {
        $cache = new \Phalcon\Cache\Backend\File($frontendCache, [
            'cacheDir' => '../app/cache/'
        ]);
    }

    return $cache;
});
```

### Logging

To use the fortrabbit [log access](/logging) the App needs to write all logs either to `STDERR` or use `error_log`. Phalcon supports the former. Open `app/config/services.php` and add a new logging service:

```php
/**
 * Create the logger facility the first time some component request the logger service
 */
$di->setShared('logger', function() {
    return new \Phalcon\Logger\Adapter\Stream("php://stderr");
});
```
