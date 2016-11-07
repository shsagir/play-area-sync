---

template:         article
reviewed:         2016-08-29
title:            Install Laravel 5
naviTitle:        Laravel
lead:             Laravel is the most PHPopular framework. Learn how to install and tune Laravel 5 on fortrabbit.
group:            Install_guides

websiteLink:      http://laravel.com?utm_source=fortrabbit
websiteLinkText:  laravel.com
category:         framework
image:            laravel-mark.png
version:          5.1
stack:            pro
oldLink:          install-laravel-5-old
uniLink:          install-laravel-5-uni

keywords:
    - php
    - install
    - laravel

---


## Get ready

We assume you've already created a New App with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


### Set the Apps root path

If you haven't already (the stack chooser does that for you) - in the Dashboard: [Set the root path](/app#toc-set-a-custom-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>

### Add the application key

If you haven't already (the stack chooser does that for you) - in the Dashboard: Add an application key as a new as an [environment variable](/env-vars) named `APP_KEY` to your App. You can use this:

```osterei32
APP_KEY=ClickToGenerate
```

<div markdown="1" data-user="known">
[Go to ENV vars for the App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>



## Install

Execute the following in your local terminal to start from scratch with a fresh new Laravel installation on fortrabbit (see [below](#toc-add-an-existing-project) on how to add an existing project):

```bash
# 1. Use Composer to create a local Laravel project named like your App
$ composer create-project laravel/laravel --prefer-dist {{app-name}}
# this installs Laravel locally and will take a while

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
# this will install Laravel on remote and take another while
# the next deployments will be much faster
```

**Got an error?** Please see the [access troubleshooting](/access-methods#toc-troubleshooting). **Did it work?** Cool, when this is done, you can visit your App URL in the browser to see the Laravel welcome screen:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io)




## Tune

Until now this is a vanilla Laravel. Now, make it yours.



### MySQL

Use [App secrets](secrets) to attain database credentials. Replace all contents from `config/database.php` in your editor like so:

```php
<?php
// locally: use standard settings
$mysql = [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    'charset'   => 'utf8',
    'collation' => 'utf8_unicode_ci',
    'prefix'    => '',
    'strict'    => false,
    'engine'    => null,
];


// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $mysql = [
        'driver'    => 'mysql',
        'host'      => $secrets['MYSQL']['HOST'],
        'port'      => $secrets['MYSQL']['PORT'],
        'database'  => $secrets['MYSQL']['DATABASE'],
        'username'  => $secrets['MYSQL']['USER'],
        'password'  => $secrets['MYSQL']['PASSWORD'],
        'charset'   => 'utf8',
        'collation' => 'utf8_unicode_ci',
        'prefix'    => '',
        'strict'    => false,
    ];
}

return [
    'fetch'         => PDO::FETCH_CLASS,
    'default'       => env('DB_CONNECTION', 'mysql'),
    'connections'   => [
        'mysql' => $mysql,
    ],
    'migrations' => 'migrations'
    // possible other code …
];
```

This example contains environment detection, so the App can run on your local machine with your local database, as well as with the one on fortrabbit.

#### MySQL access from local

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.

#### Migrate & other database commands

You can [execute remote commands via SSH](/remote-ssh-execution), for example:

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan migrate
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan migrate:rollack
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com php artisan tinker
```

If `APP_ENV` is set to `production` - which is the default - then Laravel expects `--force` for migrate commands.



### Object Storage

fortrabbit Apps have an [ephemeral storage](/quirks#toc-ephemeral-storage). If you require a persistent storage, for user uploads or any other runtime data your App creates, you can use our [Object Storage Component](/object-storage). Once you have booked the Component in the Dashboard the credentials will become available via the [App secrets](/secrets).

To make your App access the Object Storage, open up `config/filesystems.php` and modify it as following:

<!-- TODO: double check: flysystem must not be enabled here? it will work like that? -->

```php
// construct credentials from App secrets, when running on fortrabbit in production
$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);

return [
    // other code …
    'disks' => [
        // other code …
        's3' => [
            'driver'   => 's3',
            'key'      => $secrets['OBJECT_STORAGE']['KEY'],
            'secret'   => $secrets['OBJECT_STORAGE']['SECRET'],
            'bucket'   => $secrets['OBJECT_STORAGE']['BUCKET'],
            'endpoint' => 'https://'. $secrets['OBJECT_STORAGE']['SERVER'],
            'region'   => $secrets['OBJECT_STORAGE']['REGION']
        ],
        // other code …
    ],
    // other code …
];
```

If you want to use the Object Storage with your fortrabbit App and a local storage with your local development setup then replace the "default" value in `filesystems.php` as well. For example like so:

```php
// other code ….
'default' => env('FS_TYPE', 'local'),
// other code …
```

Now set `FS_TYPE` in your local `.env` file to the value `local` and the [environment variables](/env-vars) in the Dashboard to the value `s3`.

An alternative to our Object Storage Component is Amazon S3 and we have written up a [guide to get your started](https://blog.fortrabbit.com/new-app-cloud-storage-s3).


#### Elixir

You can use Elixir locally - not on fortrabbit as there is no Node on remote. You extend the Elixir with a publish task to export your minified assets to the [Object Storage](object-storage). This is how it works. To start, execute in your terminal:

```bash
# Get your Object Storage credentials
$ ssh {{app-name}}@deploy.{{region}}.frbit.com secrets OBJECT_STORAGE
```

Then extend your existing `gulpfile.js`:

```javascript
// extend import by "gulp" and "awspublish"
var elixir = require('laravel-elixir'),
    awspublish  = require('gulp-awspublish'),
    gulp        = require('gulp');

// create publish task
elixir.extend("publish", function(path) {
    gulp.task('publish', function() {

        var publisher = awspublish.create({
            region: '{{your-object-storage-region}}',
            params: {
                Bucket: '{{app-name}}'
            },
            signatureVersion: 'v2',
            endpoint: 'objects.{{region}}.frbit.com',
            accessKeyId: '{{app-name}}',
            secretAccessKey: '{{your-object-storage-key}}'
        });

        return gulp.src('./public/build/*')
            .pipe(publisher.publish())
            .pipe(publisher.sync('subfolder'))
            .pipe(awspublish.reporter());

    });
});
// other code …
```

Back in your terminal you now can:

```bash
# Install package via NPM
$ npm install --save-dev gulp-awspublish

# Run the publish task
$ gulp publish
```

Please mind that your `gulpfile.js` now contains sensitive data. You can exclude it from Git by adding it to your `.gitignore` file or you can read the credentials from an external json file which then also should be excluded.

Also mind that you need to tell your source code to look for the minified CCS & JS files on the offshore Object Storage.



### Logging

Per default Laravel writes all logs to `storage/log/..`. Since you don't have [direct file access](/quirks#toc-ephemeral-storage), you need to configure Laravel to write to the PHP `error_log` method instead. That's easily done: open `boostrap/app.php` and add the following **just before the** `return $app` statement at the bottom:

```php
$app->configureMonologUsing(function($monolog) {
    // chose the error_log handler
    $handler = new \Monolog\Handler\ErrorLogHandler();
    // time will already be logged -> change default format
    $handler->setFormatter(new \Monolog\Formatter\LineFormatter('%channel%.%level_name%: %message% %context% %extra%'));
    $monolog->pushHandler($handler);
});
```

You can now use our regular [log access](logging) to view the stream.


#### Setting time zone in Laravel

As Eloquent uses `PDO`, you can use the `PDO::MYSQL_ATTR_INIT_COMMAND` option. Extend your `mysql` configuration array in `app/config/database.php` or your specific environment `database.php` file:

```php
return [
    // other code …
    'mysql' => [
        // other code …
        'options'   => [
            \PDO::MYSQL_ATTR_INIT_COMMAND => 'SET time_zone = \'+02:00\''
        ]
    ],
    // other code …
];
```


### Memcache

Make sure you enabled the [Memcache](memcache) Component. Then you can use the [App Secrets](app-secrets) to attain your credentials. Modify the `memcached` connection in your `config/cache.php` like so:

```php
// locally: use standard settings
$servers = [[
    'host' => env('MEMCACHED_HOST', '127.0.0.1'),
    'port' => env('MEMCACHED_PORT', 11211),
    'weight' => 100,
]];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $servers = [[
        'host' => $secrets['MEMCACHE']['HOST1'],
        'port' => $secrets['MEMCACHE']['PORT1'],
        'weight' => 100
    ]];
    if ($secrets['MEMCACHE']['COUNT'] > 1) {
        $servers []= [
            'host' => $secrets['MEMCACHE']['HOST2'],
            'port' => $secrets['MEMCACHE']['PORT2'],
            'weight' => 100
        ];
    }
}

// other code …

return [
    // other code …
    'stores' => [
        // other code …
        'memcached' => [
            'driver'  => 'memcached',
            'servers' => $servers
        ],
        // other code …
    ],
    // other code …
];
```

In addition, set the `CACHE_DRIVER` [environment variable](env-vars) so that you can use `memcached` in your production App on fortrabbit and `apc` or `array` on your local machine, via `.env` file.


### Redis

Redis can be used in Laravel as a cache or a queue or both. First integrate with [Redis Cloud](redis-cloud) then configure the redis database connection in `config/database.php`:

```php
// locally: use standards
$redis = [
    'host'     => env('REDIS_HOST', 'localhost'),
    'password' => env('REDIS_PASSWORD', null),
    'port'     => env('REDIS_PORT', 6379),
    'database' => 0,
];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $redis = [
        'host'     => $secrets['CUSTOM']['REDIS_HOST'],
        'port'     => $secrets['CUSTOM']['REDIS_PORT'],
        'password' => $secrets['CUSTOM']['REDIS_PASSWORD']
    ];
}

return [
    // other code …
    'redis' => [
        'cluster' => false,
        'default' => $redis
    ],
    // other code …
];
```

If you plan on using Redis as a cache, then open `config/cache.php` and set `default` to `redis` (or set the `CACHE_DRIVER` [environment variable](env-vars) to `redis` in the Dashboard). For [queue](#toc-queue) usage see below.

### Queue

Laravel supports multiple queue drivers. One which can be used with fortrabbit out of the box is `database`, which simple uses your database connection as a queue. That's great for small use-cases and tinkering, but if your App handles very many queue messages you should consider [Redis](#toc-redis).

Once you've decided the queue you want to use just open `config/queue.php` and set `default` to either `redis`, `database`, `iron` or `amqp` - or even better: set the `QUEUE_DRIVER` [environment variable](env-vars) accordingly in the Dashboard.


#### CloudAMQP

CloudAMQP  can be used in Laravel as a queue driver. First integrate with [CloudAMQP](cloudamqp), install the composer package `fhteam/laravel-amqp` then configure the queue connection in `config/queue.php`:

```php
$amqp = [];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $amqp = [
        'driver'         => 'amqp',
        'host'           => $secrets['CUSTOM']['CLOUD_AMQP_HOST'],
        'port'           => $secrets['CUSTOM']['CLOUD_AMQP_PORT'],
        'user'           => $secrets['CUSTOM']['CLOUD_AMQP_USER'],
        'password'       => $secrets['CUSTOM']['CLOUD_AMQP_PASSWORD'],
        'vhost'          => $secrets['CUSTOM']['CLOUD_AMQP_VHOST'],
        'queue'          => null,
        'channel_id'     => null,
        'exchange_name'  => null,
        'exchange_type'  => null,
        'exchange_flags' => null,

         //Durable queue (survives server crash)
        'queue_flags'=> ['durable' => true, 'routing_key' => null],

        //Persistent messages (survives server crash)
        'message_properties' => ['delivery_mode' => 2],
    ];
}

return [
    // other code …
    'connections' => [
        // other code …
        'amqp' => $amqp,
        // other code …
    ],
    // other code …
];
```

Make sure you have added `Forumhouse\LaravelAmqp\ServiceProvider\LaravelAmqpServiceProvider::class` to `providers` in `config/app.php`.

Lastly set the `QUEUE_DRIVER` [environment variable](env-vars) in the Dashboard to `amqp`.

#### IronMQ

IronMQ can be used in Laravel as a queue driver. First integrate with [IronMQ](ironmq) then configure the queue connection in `config/queue.php`:

```php
// locally: use standards
$iron = [
    'driver'  => 'iron',
    'host'    => env('IRON_MQ_HOST'),
    'token'   => env('IRON_MQ_TOKEN'),
    'project' => env('IRON_MQ_PROJECT_ID'),
    'queue'   => env('IRON_MQ_QUEUE'),
    'encrypt' => true,
];

// on fortrabbit: construct credentials from App secrets
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    $iron = [
        'driver'  => 'iron',
        'host'    => $secrets['CUSTOM']['IRON_MQ_HOST'],
        'token'   => $secrets['CUSTOM']['IRON_MQ_TOKEN'],
        'project' => $secrets['CUSTOM']['IRON_MQ_PROJECT_ID'],
        'queue'   => $secrets['CUSTOM']['IRON_MQ_QUEUE'],
        'encrypt' => true,
    ];
}

return [
    // other code …
    'connections' => [
        // other code …
        'iron' => $iron,
        // other code …
    ],
    // other code …
];
```

**Note for Laravel 5.2:** The `iron`-block does not exist and needs to be created. Also the `laravelcollective/iron-queue` composer package must be installed manually (mind adding `Collective\IronQueue\IronQueueServiceProvider::class` into `providers` in `app.php`).

Lastly set the `QUEUE_DRIVER` [environment variable](env-vars) in the Dashboard to `iron`.


#### Using envoy

Easy. Here is an `Envoy.blade.php` example:

```php
@servers(['fr' => '{{ssh-user}}@deploy.{{region}}.frbit.com'])

@task('ls', ['on' => 'fr'])
    ls -lha
@endtask

@task('migrate', ['on' => 'fr'])
    php artisan migrate
@endtask
```

Then execute locally:

```bash
$ envoy run ls
$ envoy run migrate
```


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but Laravel provides a API over the popular SwiftMailer library. The mail configuration file is `app/config/mail.php`, and contains options allowing you to change your SMTP host, port, and credentials, as well as set a global form address for all messages delivered by the library.


## Add an existing project

You can also push your existing Laravel installation to fortrabbit. When you already using Git, you can add fortrabbit as an additional remote, like described [above](#toc-install) under point 6. When moving from another host to fortrabbit, please also read our [migration guide](/migrating) as well.
