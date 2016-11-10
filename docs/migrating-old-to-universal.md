---

template:      article
naviTitle:     Migrating Old App to Universal App
title:         Migrating Old App to Universal App
reviewed:      2016-11-10
lead:          "The Old Stack is sunsetting. Here is how you manually migrate an Old Stack App to the new Universal Stack."
group:         platform
stack:         all

---


Depending on the MySQL and web storage size of your App, the whole migration process should take you between a couple of minutes and an hour.

## Preparation

First you need to decide which Universal App plan fit's your needs. Start by checking your current Old App metrics in the Dashboard > Your App > Metrics. Find out:

* How much web storage the App is using
* How much MySQL database storage the App is using
* If any [App collaborators](app-collaboration) have code access

Now make sure the following applies to your App:

1. Your App doesn't use "PHP l" or "PHP xl" (which are multi-node plans) or your App currently uses either of them but doesn't need as much resources
2. Your App doesn't use [Memcache](memcache-old), which is not available for single-node Universal Apps

If you are unsure about (1), then check the App metrics: If your PHP requests are below 1000 per hour on average, you are, most likely, good and can proceed. If either of the above applies an cannot be changed, then we strongly recommend to use a [Professional App plan](TODO) instead.


### Create a new Universal App

Now that you know how much resources you require, you can decide upon the [right plan](//www.fortrabbit.com/pricing): Choose the one which grants you sufficient MySQL, sufficient web storage and sufficient App collaborators as you have found out in the previous step.

With that knowledge, create a new App in the Dashboard > click "Create an App" > follow the instructions and choose at the end the previously decided Universal App plan.

For the purposes of this article, we recommend to chose the newly created Universal App in the App chooser on the right side in this Help. This article will show you copy&paste-able code examples this way.

### Copy settings

Make sure you apply the same configuration to your new Universal App, as you have set for your Old App. Following a short checklist:

* [ ] PHP Settings (especially: extensions)
* [ ] [Environment variables](env-vars), if any
* [ ] [App secets](secrets), if any


## Data migration

Once the preparation is completed, you can start with copying data.

### Migrate MySQL database

Assuming you are using MySQL, you need to [import and export](mysql#toc-export-amp-import) your database from the old App to the new one.

### Migrate web storage

There are two basic scenarios from which you can start:

1. You are using Git-only deployment & you don't utilize runtime data (like user uploads)
2. You are not using Git, or only sometimes, and you have runtime data

If you are unsure, go with (2).

#### Git only

Clone your Old App's repo now, if you haven't a current local clone available:

```bash
$ cd your-project-folder
$ git clone {{old-app-name}}@git.eu1.frbit.com {{old-app-name}}
$ cd {{old-app-name}}
```

Now, since your local code base is already under Git version control, all you need to do is to add your new Universal App's git remote:

```bash
$ cd {{old-app-name}}
$ git remote add fortrabbit2 {{app-name}}@deploy.{{region}}.frbit.com
```

**Note**: The above example names the new remote `fortrabbit2` so it doesn't collide with `fortrabbit`, which we recommend normally - you can use whichever name you prefer.

#### No Git

Download ALL your web data using [SFTP](ssh-sft-old) from the web storage to a local folder.

## Code & config adjustments

How to proceed here depends on how you current Old App is configured:

1. Are you using hard coded configuration, eg MySQL password in a configuration file?
2. Are you using [ENV var](env-vars) based configuration (or no configuration needs)?

### Hard coded configuration

You need change all those hard coded App specific configurations. Depending on your App, you might not have any such hard coded configurations at all. If you are not sure, here is what to look out for:

#### Web root folder

* **OLD**: `/var/www/web/{{old-app-name}}/htdocs`
* **NEW**: `/srv/app/{{app-name}}/htdocs`

#### MySQL configuration

* **OLD**:
  * *Hostname:* `{{old-app-name}}.mysql.eu1.frbit.com`
  * *Port:* `3306`
  * *Database:* `{{old-app-name}}`
  * *Username:* `{{old-app-name}}`
  * *Password:* your Old Apps MySQL password
* **NEW**
  * *Hostname:* `{{app-name}}.mysql.{{region}}.frbit.com`
  * *Port:* `3306`
  * *Database:* `{{app-name}}`
  * *Username:* `{{app-name}}`
  * *Password:* [obtain your new App's MySQL password](mysql#toc-obtain-the-mysql-password)

### ENV var based configuration

Review the [settings](#toc-copy-settings) you copied earlier. Make sure you use your new App's MySQL login details and root path (as shown above) are used. Mind that you can leverage our new [nested environment variables](env-vars#toc-nested-variables). For example, if you are using an environment variable named `DB_HOST` which contains your App's MySQL hostname, you can utilize nested variables like so:

```plain
# reference the fortrabbit provided MYSQL_HOST var
DB_HOST=${MYSQL_HOST}
```

Find out which ENV vars are [available here](env-vars#toc-env-var-types).


## Deploy to new Universal App

Now it's time to deploy your adjusted code base. Again, depending on whether you are using Git only or no Git (see [above](#toc-migrate-web-storage)):

### Git only

Double check your `.gitignore` file. Make sure that it does *NOT* contain `composer.lock` or `composer.json`, if you are working with Composer. When you are confident it looks alright, just push to your new Universal App's remote:

```bash
$ git push -u fortrabbit2 master
```

### No Git

Upload ALL your web data using [SFTP](sftp-uni) from your local folder to thew new Universal App.

## Review

Once the first deployment is done, it's time to review your new App: are any of the configurations from the preparation missing or wrong? Do you have been to loose or to strict with the contents in your git ignore file? And so on. It should basically work at this point. The only thing missing are your custom domains, if you are using any.

## Finish up

Once your new Universal App is working as expected, you can safely migrate all domains from the Old App to the new Universal App. Mind that you also keep the same root path, when doing so.
