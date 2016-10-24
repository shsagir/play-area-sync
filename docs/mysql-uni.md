---

template:      article
reviewed:      2016-10-10
title:         All about MySQL
naviTitle:     MySQL
lead:          Each App comes with a pre-installed MySQL database. Learn how to access & configure the common database on fortrabbit.
group:         platform

stack:         universal
oldLink:       mysql-old
proLink:       mysql-pro

keywords:
     - localhost
     - mysqldump
     - dump
     - mysql
     - database
     - innodb
     - myisam
     - phpmyadmin
     - heidisql
     - workbench
     - sequel-pro
     - nosql

---


## Access MySQL from your App

Usually there is a configuration file which is used from the App to connect to the database. This is what you need to fill in there:

* **Database Name**: `{{app-name}}`
* **Username**: `{{app-name}}`
* **Password**: `{{app-database-password}}` _[< how to recover](/#toc-obtain-the-mysql-password)_
* **Database Host**: `{{app-name}}.mysql.{{region}}.frbit.com` _< not localhost_

See our specific examples for: [Laravel](install-laravel-uni#toc-mysql), [Symfony](install-symfony-uni#toc-mysql), [WordPress](install-wordpress-uni#toc-mysql), [Craft CMS](install-craft-uni#toc-mysql).


## Access MySQL from local

Sometimes you want to run a certain query on your live database. Or you want to dump your database. So you need to access the MySQL database on fortrabbit remotely. For security reasons you can not connect to the MySQL database from "outside" directly. But you can open a [SSH tunnel](http://en.wikipedia.org/wiki/Tunneling_protocol) and then connect to the MySQL database thru this tunnel.


#### MySQL access

* **MySQL host**: `{{app-name}}.mysql.{{region}}.frbit.com` < not localhost
* **MySQL database**: `{{app-name}}`
* **MySQL user**: `{{app-name}}`
* **MySQL password**: [Lookup in the Dashboard](https://dashboard.fortrabbit.com/apps/{{app-name}}#mysql)
* **MySQL port**: `3306` < usually not required

<!-- TODO: make password link work in Dashboard -->


#### SSH tunnel for MySQL access

* **SSH tunnel server**: `tunnel.{{region}}.frbit.com`
* **SSH tunnel user**: `{{ssh-user}}`
* **SSH password**: `{{ssh-password}}`
* **SSH port**: `22` < usually not required


### Obtain the MySQL password

You can lookup the MySQL in the Dashboard with your App. To pickup 

<!-- TODO: in Dashboard, the tabs need to react on the anchor tag -->

* https://dashboard.fortrabbit.com/apps/{{app-name}}





### Resetting the MySQL password

Instead of reclaiming you can also set a new MySQL password. Please mind that this can have some consequences: 

* Your App will can't connect any more — you'll need to update the Apps config then
* Your coworkers won't be able to connect afterwards




### MySQL via GUI

Sometimes it's handy to manage your remote MySQL database with a graphical interface. We recommend [MySQL Workbench](http://www.mysql.com/products/workbench/) (Mac/Linux/Windows). There is also [Navicat](http://www.navicat.com/products/navicat-for-mysql) (also multi-platform), [HeidiSQL](http://www.heidisql.com/) for Windows and [Sequel Pro](http://www.sequelpro.com/) for Mac. And a [host of others](https://www.google.com/search?q=mysql%20gui).

All clients have connection presets that help you to establish the SSH tunnel and the MySQL connection in one convenient step. In the connection info you will insert all the SSH access information and the MySQL connection information. 

The MySQL hostname will not be `127.0.0.1` or `localhost` — it's the remote server:  
`{{app-name}}.mysql.{{region}}.frbit.com`.

#### phpMyAdmin

Please don't try to install phpMyAdmin on your fortrabbit App - for security and practical reasons. But you can also manage the remote MySQL with your **local phpMyAdmin installation**. Add an additional server configuration to your local phpMyAdmin `config.inc.php` file like so:

```
$cfg['Servers'][$i]['verbose']       = '{{app-name}}';
$cfg['Servers'][$i]['host']          = '127.0.0.1';
$cfg['Servers'][$i]['port']          = '13306'; // like specified in the tunnel command (see below)
$cfg['Servers'][$i]['connect_type']  = 'tcp';
$cfg['Servers'][$i]['extension']     = 'mysqli';
$cfg['Servers'][$i]['compress']      = FALSE;
$cfg['Servers'][$i]['auth_type']     = 'cookie';
$i++;
```

Then open a [tunnel](#toc-mysql-via-terminal), then visit your local phpMyAdmin in the browser. You now can select your fortrabbit App. You will be asked for the MySQL user "**{{app-name}}**" and [password](#toc-obtain-the-mysql-password). 




### MySQL via terminal

If you are familiar with the shell then this is no biggie. Issue this in your terminal:

```bash
# open a tunnel on local port 13306 < arbitrary, choose between 10000-65000
$ ssh -N -L 13306:{{app-name}}.mysql.{{region}}.frbit.com:3306 {{ssh-user}}@tunnel.{{region}}.frbit.com
```

**This command will not reply with any message on success! If nothing shows up: you did right!** This behavior is how SSH clients are implemented and sadly we cannot issue any positive response message.

Once the tunnel is up, you can connect to the remote MySQL database with the `mysql` console client. Open a **new window terminal window** and issue:

```bash
# connect to the database < use 127.0.0.1, not localhost
$ mysql -u{{app-name}} -h127.0.0.1 -P13306 -p {{app-name}}
```

In the next step you will be asked for your [MySQL password](#toc-obtain-the-mysql-password).


##  Export & import

A common task is to move your MySQL data around, e.g. if you are migrating to fortrabbit or you are about to set up a staging environment. GUIs have easy to use export/import wizards, in the terminal you can do this like so:

### mysqldump & mysql

Using `mysqldump` and `mysql` is the standard approach to migrate a database between two MySQL servers. First of start by exporting your data from your old database (example):

```bash
# on your local machine or on the old server
$ mysqldump {{database-name}} > database.sql
```

Now [create a tunnel](#toc-mysql-via-terminal) to your fortrabbit App's MySQL database and import everything:

```bash
$ mysql -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}} < database.sql
```

### LOAD DATA

You can export and import a large, single table with the following example:

```bash
# on your local machine or on the old server
$ echo 'SELECT * FROM tablename;' | mysql database-name > tablename.sql

# import everything via a tunnel to yourfortrabbit MySQL database
$ mysql --local-infile=1 -h127.0.0.1 -P13306 -u{{app-name}} -p {{app-name}}

# on the mysql shell
$ mysql> LOAD DATA LOCAL INFILE '/path/to/tablename.sql' INTO TABLE tablename;
```

## Local MySQL

This article describes how to deal with the fortrabbit remote MySQL database. You might a local one as well. Please see our [local development article](/local-development).

- - -

## Advanced usage

### Different time zone

MySQL has [time zone support](http://dev.mysql.com/doc/refman/5.5/en/time-zone-support.html) Our nodes default to the standard time zone "UTC". If you want to change this time zone, you can do so on a "per connection" basis.

There are two approaches to tackle this issue: handle the time zone on application level or handle the time zone on database level. Each has its merits and which one is better strongly depends on the use case. This article shows you how to set the time zone in the database.

#### Setting time zone in plain (My)SQL

The syntax to change the time zone is:

```sql
-- set time zone to +3 hours
SET time_zone = '+03:00';

-- set time zone to -7 hours
SET time_zone = '-07:00';
```

You can query the current time zone like so:

```sql
SELECT @@session.time_zone;
```

#### Setting time zone with PDO

`PDO` offers configurable options when setting up the connection. One of them allows you to issue commands right after initialization (connection).

```php
$secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);

$pdo = new PDO(
    'mysql:host='. $secrets['MYSQL']['HOST']. ';dbname='. $secrets['MYSQL']['DATABASE'],
    $secrets['MYSQL']['USER'],
    $secrets['MYSQL']['PASSWORD'],
    array(
        PDO::MYSQL_ATTR_INIT_COMMAND => 'SET time_zone = \'+02:00\''
    )
);
```
