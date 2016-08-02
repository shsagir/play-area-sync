---

template:         article
reviewed:         2016-07-27
title:            Install Drupal 8
naviTitle:        Drupal
lead:             Drupal is one of the best known open source PHP CMS. Learn here how to use it with fortrabbit.


websiteLink:      https://www.drupal.org/?utm_source=fortrabbit
websiteLinkText:  drupal.org
category:         CMS
image:            drupal8-mark.png
version:          8.1 (dev)

seeAlsoLinks:
    - app

keywords:
    - drupal
    - install

tags:
    - beginner
    - php
    - install

---

## Get ready

We assume you've already created a New App with fortrabbit. On your local machine you should have installed: [Git](/git), Composer and PHP of course.


### Set the Apps root path

If you haven't already — in the Dashboard: [Set the root path](/app#toc-set-a-custom-root-path) of your App's domains to **web**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>


## Install

You'll need a local Composer based [Drupal 8 installation](https://github.com/drupal-composer/drupal-project). You can either use an existing one (see [below](#toc-existing-installation)) or initialize a new one like so:

### New installation

Execute this in the terminal:

```bash
# Create a new Drupal project locally with Composer:
$ composer create-project drupal-composer/drupal-project:8.x-dev --stability dev --no-interaction {{app-name}}
```

<!-- TODO: link to local development setup help article, hint or link for now -->

Once that is done, visit your local installation in the browser and proceed with the installation wizard. Once that is also done you can continue with "Existing installation". Alternatively you can execute the installation from the terminal:

```bash
$ cd {{app-name}}
$ php vendor/bin/drupal site:install --root=web
```

### Existing installation

Create a new file `web/sites/default/settings_prod.php` with the following contents:

```php
// environment detection: Only triggered on fortrabbit
if (!isset($_SERVER['APP_SECRETS'])) {
    return;
}

$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);

// configure database
$databases['default']['default'] = [
    'database'  => $secrets['MYSQL']['DATABASE'],
    'username'  => $secrets['MYSQL']['USER'],
    'password'  => $secrets['MYSQL']['PASSWORD'],
    'prefix'    => '',
    'host'      => $secrets['MYSQL']['HOST'],
    'port'      => $secrets['MYSQL']['PORT'],
    'namespace' => 'Drupal\\Core\\Database\\Driver\\mysql',
    'driver'    => 'mysql',
];
```

Now open up `web/sites/default/settings.php`. At the bottom of the file include the just created production file:

```php
// other code …
include __DIR__ . '/settings_prod.php';
```


Next, create the [deployment file](/deployment-file-v2) `fortrabbit.yml`:

```yaml
sustained:
    - vendor
    - web/core
    - web/modules/contrib
    - web/themes/contrib
    - web/profiles/contrib
```

<!-- TODO: correct the following sentence (unclear, wrong english) -->
Make sure that the `web/core` folder, which contains the Drupal core itself, and the `web/modules/config` folder, which contains later installed modules.


#### Object Storage

fortrabbit Apps have an [ephemeral storage](/quirks#toc-ephemeral-storage). If you require a persistent storage, for user uploads or any other runtime data your App creates, you can use our [Object Storage Component](/object-storage). Once you have booked the Component in the Dashboard the credentials will become available via the [App secrets](/secrets).

Open up `web/sites/default/settings_prod.php` again and add at the bottom:

``` php
// enable object storage
$schemes = [
    'object-storage' => [
        'driver' => 's3',
        'config' => [
            'key'      => $secrets['OBJECT_STORAGE']['KEY'],
            'secret'   => $secrets['OBJECT_STORAGE']['SECRET'],
            'region'   => $secrets['OBJECT_STORAGE']['REGION'],
            'bucket'   => $secrets['OBJECT_STORAGE']['BUCKET'],
            'options'  => [],
            'protocol' => 'https',
            'prefix'   => '',
            'cname'    => $secrets['OBJECT_STORAGE']['HOST'],
            'endpoint' => 'https://' . $secrets['OBJECT_STORAGE']['SERVER'],
        ],
        'cache'     => true
        'serve_js'  => true,
        'serve_css' => true
    ],
];
$settings['flysystem'] = $schemes;

// use /tmp folder
$config['system.file']['path.temporary'] = '/tmp';
```

#### Flysystem

To use the Object Storage, you need file abstraction. For this you install and enable the [Flysystem modules](https://www.drupal.org/project/flysystem) by executing this in the terminal:

```bash
# Install Flysystem
$ composer require drupal/flysystem dev-8.x-1.x
$ composer require drupal/flysystem_s3 dev-8.x-1.x

# Enable (drupal console must be called from the `web/` sub folder)
$ php vendor/bin/drupal module:install flysystem --root=web
$ php vendor/bin/drupal module:install flysystem_s3 --root=web
```



#### MySQL

Now is a good time to export your local database and import it into your App. First you'll need to open up a tunnel to your App's MySQL database — [see here](/mysql#toc-mysql-via-terminal). Then you can execute this in the terminal:

```bash
# Fetch your fortrabbit database credentials
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL

# export local database
$ mysqldump -u{{your-local-user}} -p {{your-local-db}} > drupal.sql

# import into fortrabbit database
# you will be prompted for your fortrabbit database password
$ mysql -u{{app-name}} -h127.0.0.1 -P13306 -p {{app-name}} < drupal.sql
```

#### MySQL access from local

Please see the [MySQL article](mysql#toc-access-mysql-from-local) on other ways to access the database remotely from your computer.


### Configure Git

You usually don't need to touch the `.gitignore` file. This is how it should like:

```nohighlight
# Ignore directories generated by Composer
drush/contrib
vendor
web/core
web/modules/contrib
web/themes/contrib
web/profiles/contrib

# Ignore Drupal's file directory
web/sites/*/files

# Ignore files generated by PhpStorm
.idea
```

#### Initialize Git and deploy

Back in the terminal:

```bash
# Setup Git (if you haven't already)
$ git init .

# Add your App's Git remote repo
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# Add all files
$ git add -A

# Commit files
$ git commit -m 'Initial'

# Push to deploy
$ git push -u fortrabbit master
```

Once the deployment has finished (takes much longer on the first time as all Composer packages need to be installed) you can visit the App URL in your browser:

* [https://{{app-name}}.frb.io](https://{{app-name}}.frb.io)


#### Modify upload destinations

Once everything is pushed and online login to your App [https://{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) and modify the "Field Settings" of all structure elements which require Upload. For example::

* Login as admin
* Go to "Structure"
* Click on "Content Types"
* In the "Article" row click on "Manage fields"
* In the "Image" row click on "Edit"
* Open the "Field settings" tab
* Set "Upload destination" to "Flysystem: object-storage"

Rinse and repeat for all structure elements, for which you want to upload files.
