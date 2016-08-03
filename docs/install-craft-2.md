---

template:         article
reviewed:         2016-08-02
title:            Install Craft CMS 2 on fortrabbit
naviTitle:        Craft CMS
lead:             Craft is a CMS you and your clients love. Learn how to deploy Craft using Git on fortrabbit.
group:            Install_guides

websiteLink:      https://craftcms.com/?utm_source=fortrabbit
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          2.5
group:            Install_guides

keywords:
    - craft
    - craftCMS
    - setup
    - install-guide

tags:
    - advanced
    - php
    - install

---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

### Set the root path

In the fortrabbit Dashboard: [Set the document root](/domains#toc-set-a-custom-root-path) of your App's domains to `public`. This applies to all domains, either the App URL or your external domains.

### Set ENV vars

If you haven't already: login the fortrabbit Dashboard and set the following [environment variables](env-vars):

```
CRAFT_DEBUG=0
CRAFT_CACHE=memcache
CRAFT_UPDATES=0
```

### Set App secrets

Then, still in the fortrabbit Dashboard, head over to the [App secrets](secrets) and add a validation key (long random string):

```osterei32
CRAFT_KEY=LongRandomString
```


## Install

If you are just testing then best use the [HappyLager Demo](https://github.com/pixelandtonic/HappyLager) and follow their [install guide](https://github.com/pixelandtonic/HappyLager#installation) to run it locally first. If you have a running (production) installation then you need to export its data and set up a local, working "clone" with which you can proceed.


## Configure the fortrabbit environment

Craft's native multi-environment configuration (also see our [multi-staging article](multi-staging)) options allow you to define configuration options based on the domain name. This is great, but there is a potential security flaw (when using Git based deployments) you should be aware of: You're hard-coding the configuration details of your production environment into your code, which means you will have sensitive information in your Git version history. Let's solve this.

Open up `craft/config/general.php` and change it like so:

```php
// Only triggered on fortrabbit
$validationKey = false;
if ($file = getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents($file), true);
    $validationKey = $secrets['CUSTOM']['CRAFT_KEY'];
}

return [
    'allowAutoUpdates'  => getenv('CRAFT_UPDATES') ?: true,
    'cacheMethod'       => getenv('CRAFT_CACHE') ?: 'file',
    'devMode'           => getenv('CRAFT_DEBUG') ?: false,
    'siteUrl'           => 'http://'. $_SERVER['HTTP_HOST'],
    'validationKey'     => $validationKey,
    'environmentVariables' => [
        'assetsBaseUrl'  => '/assets',
        'assetsBasePath' => './assets',
    ]
];
```

## MySQL

Your App needs database access - when working local and on remote. We use environment detection to store both access informations. On fortrabbit the actual credentials are getting parsed from the [App secrets](secrets). Open `craft/config/db.php` in your editor and modify it like the following:

```php
// Connnect to MySQL on fortrabbit
if ($file = getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents($file), true);
    return [
        'server'      => $secrets['MYSQL']['HOST'],
        'user'        => $secrets['MYSQL']['USER'],
        'password'    => $secrets['MYSQL']['PASSWORD'],
        'database'    => $secrets['MYSQL']['DATABASE'],
        'port'        => $secrets['MYSQL']['PORT'],
        'tablePrefix' => 'craft',
    ];
}

// Connect to MySQL from local development environment
return [
    'server'      => '127.0.0.1',
    'user'        => 'your-local-db-user',
    'password'    => 'your-local-db-password',
    'database'    => 'your-local-db-name',
    'tablePrefix' => 'craft',
];
```

### MySQL access from local

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on how to access the database remotely from your computer.



## Object Storage

Since fortrabbit does not support a [persistent storage](quirks#toc-ephemeral-storage) you want to use the [Object Storage](object-storage) to save your uploads and static assets.

We've prepared a Craft plugin that acts as a drop-in replacement for the Amazon S3 asset source. [Download the plugin](https://github.com/fortrabbit/craft-s3-fortrabbit) from GitHub and **follow the setup instructions in the [README.md](https://github.com/fortrabbit/craft-s3-fortrabbit/blob/master/README.md)**. Once that's done, you can create your Object Storage asset source:

1. In the Craft admin: Go to Settings > Assets
2. Click on `New asset source` to create a new `AssetSource`
3. Give it a name like `Object Storage`
4. Set the Type to `Amazon S3`
4. Enter the Access Key ID and Secret Access Key
5. Click on `Refresh`
5. Select a `Bucket`
6. Enter a `Subfolder` (use the App name here)
7. Click `Save` in the upper right corner

You now need to move all the "local assets" to the newly created `Object Storage` asset source:

1. Go to Assets
2. Drag & drop assets to the `Object Storage` asset source (left side)

**Note**: Rinse and repeat with all your local asset sources! To make use of the cloud storage support of Craft you need a "Pro" license.

Now that is done you can safely remove the empty, local asset sources:

1. Got to Settings > Assets
2. Click the remove button for every `Local Folder` type asset source


### Cache & sessions

If you are just testing out Craft: make sure you use tinkering PHP plan with fortrabbit. With those you can set the cache variable (above) to `file`, `db` or `apc` during your tinkering, for example:

```
CRAFT_CACHE=db
```

All fortrabbit production PHP plans run on multiple Nodes and require a cache which is accessible jointly - same goes for sessions, of course. This disqualifies `apc`, `db` and `file` and leaves you with `memcache` as options.

So make sure you have booked the [Memcache component](memcache) and then create the file `craft/config/memcache.php` with the following content:

```php
if ($file = getenv('APP_SECRETS')) {
    $secrets = json_decode(file_get_contents($file), true);
    if (isset($secrets['MEMCACHE'])) {
        $memcache = $secrets['MEMCACHE'];

        $handlers = [];
        $servers  = [];
        foreach (range(1, $memcache['COUNT']) as $num) {
            $handlers []= $memcache['HOST'. $num]. ':'. $memcache['PORT'. $num];
            $servers []= [
                'host'          => $memcache['HOST'. $num],
                'port'          => $memcache['PORT'. $num],
                'retryInterval' => 2,
                'status'        => true,
                'timeout'       => 2,
                'weight'        => 1,
            ];
        }

        // session config
        ini_set('session.save_handler', 'memcached');
        ini_set('session.save_path', implode(',', $handlers));
        if ($memcache['COUNT'] == 2) {
            ini_set('memcached.sess_number_of_replicas', 1);
            ini_set('memcached.sess_consistent_hash', 1);
            ini_set('memcached.sess_binary', 1);
        }

        // craft config
        return [
            'servers'      => $servers,
            'useMemcached' => true,
        ];
    }
}
```

### Database migration

In case you have a database you want to export/import: Database migration is straight forward: export the database of the local installation and import it to your fortrabbit App. You can use a [GUI](mysql#toc-mysql-guis) or the shell. From the shell you start out by [opening up a tunnel](mysql#toc-shell-tunnel-mysql) and then use `mysqldump` to export and `mysql` to import:

```bash
# on your local machine

# export from local
$ mysqldump database-name > database.sql

# import to fortrabbit using the tunnel
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < database.sql
```


## Deploy with Git

Now that you have the configuration done, let's get the code up. If your local Craft installation is not under Git version control already, then you do it now:

```
# Initialize Git
$ git init .

# Add your App's Git remote to your local repo:
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
```

Now open and modify the `.gitignore` file:

```bash
# Exclude misc project files
*.idea/*
*.log
*.DS_Store
*Thumbs.db
*.sass-cache/
node_modules
*config.codekit
*.sql

# Exclude uploads, runtime data and temp files, but keep the layout assets
/craft/storage/runtime/
/craft/storage/cache/
/craft/storage/logs/
/craft/storage/compiled_templates/
/public/assets/*
!/public/assets/css/*
!/public/assets/js/*
!/public/assets/fonts/*

# Exclude Composer's vendor folder
/vendor/
```

You're almost done with the basic setup. Last step is to deploy your code:

```
$ git add -A
$ git commit -m 'Init'
$ git push -u fortrabbit master
```

**Done! Your Craft App is online!**

## Tuning


### Logging & debugging

Per default Craft writes all the logs to a file, which won't do. So we wrote a simple plugin, which writes all logs to STDERR instead. Then the logs are accessible via our standard [logging](logging). The plugin can be found here: [github.com/ostark/craft-stderr-logger](https://github.com/ostark/craft-stderr-logger)


### Sending mail

You can not use [sendmail](quirks#toc-mailing) on fortrabbit but Craft provides multiple alternative options. In the Craft admin: just go to Settings > Email, change the `Protocol` to `SMTP` or `Gmail` and enter the credentials of your mail service.
