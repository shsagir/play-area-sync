---

template:         article
reviewed:         2016-10-04
title:            Install Drupal 8
naviTitle:        Drupal 8
lead:             Drupal 8 is one of the best known open source PHP CMS. Learn here how to use it with fortrabbit.
group:            Install_guides
stack:            hobby
proLink:          install-drupal-8-pro

websiteLink:      https://www.drupal.org/?utm_source=fortrabbit
websiteLinkText:  drupal.org
category:         CMS
image:            drupal8-mark.png
version:          8.1

keywords:
    - drupal
    - install

---

## Get ready

We assume you've already created a New App with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.


## Install Drupal 8 with SFTP

The most straight forward way to install Drupal 8 is by downloading it to your local machine and then upload it to your fortrabbit App:

1. Download [the latest](https://www.drupal.org/8) archive
2. Unpack the archive locally
3. Open an SFTP client
4. Connect to your fortrabbit App (access credentials)
3. Upload the **contents** of the local `drupal-8.1.xxx` to the remote `htdocs` folder

### Configure in the browser

Now visit [{{app-name}}.frb.io](https://{{app-name}}.frb.io) in the browser and commence with the guided web installation.

**Note**: If Drupal complains about missing OPcache: You can safely ignore this warning and continue anyway.


In the database form make sure the "advanced options" are opened, then enter:

* **Database type**: `MySQL`
* **Database name**: `{{app-name}}`
* **Database username**: `{{app-name}}`
* **Database password**: `{{your-database-password}}`
* **Host**: `{{app-name}}.mysql.{{region}}.frbit.com`
* **Port number**: keep `3306`
* **Table name prefix**: keep empty (or adjust to your preferences)

Following enter your site name, admin user, e-mail and password.

### Last touch up

After that you should be done open the file `htdocs/sites/default/files/.htaccess` via SFTP or SSH and

* Find the line reading `Options -Indexes -ExecCGI -Includes -MultiViews` (near the top)
* Remove `-ExecCGI` and `-Includes` so that it reads `Options -Indexes -MultiViews`

**Note**: If you edit the file from SSH and cannot safe it: execute `chmod u+w sites/default/files/.htaccess` before modifying the file and `chmod u-w sites/default/files/.htaccess` when you are done.

### Done

You are done an can visit your now Drupal installation in the browser: [{{app-name}}.frb.io](https://{{app-name}}.frb.io)
