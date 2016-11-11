---

template:         article
reviewed:         2016-11-11
title:            Install Craft CMS 2 on fortrabbit
naviTitle:        Craft CMS
lead:             Craft is a CMS you and your clients love. Learn how to deploy Craft using Git on fortrabbit.
group:            Install_guides
stack:            uni
proLink:          install-craft-2-pro

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

---


## Get ready

We assume you've already created a New App and chose Craft CMS in the stack chooser. If not: You can do so in the [fortrabbit Dashboard](/dashboard). If you haven't chosen Craft CMS stack when creating the App in the Dashboard, please set the following:

### Root path

Go to the Dashboard and [set the root path](/app#toc-set-a-custom-root-path) of your App's domains to **public**.

<div markdown="1" data-user="known">
[Change the root path for App URL of App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/{{app-name}}.frb.io/docroot)
</div>


## Quick start

Start by [downloading the latest Craft release](https://craftcms.com/) from the Craft CMS website and unpack it locally. It will extract into multiple folders, so make sure that you unpack it in an empty parent directory. Now open the just extracted `craft/config/db.php` and change it as following:

```php
return [
	'server'      => getenv('MYSQL_HOST')     ?: 'localhost',
	'database'    => getenv('MYSQL_DATABASE') ?: 'local-db-name',
	'user'        => getenv('MYSQL_USER')     ?: 'local-db-user',
	'password'    => getenv('MYSQL_PASSWORD') ?: 'local-db-password',
	'tablePrefix' => 'craft',
];
```

Before you upload anything: Rename the file `htaccess` in the extracted folder `public` to `.htaccess` (with a `.` in the beginning). If your operating system makes that hard for you (hello Mac), you can first upload and then rename the file directly on fortrabbit via SFTP.

Copy everything you just extracted, including the changed `db.php` file and the renamed `.htaccess` file, via SFTP to the `htdocs` folder of your App. The `htdocs` folder is the one you are automatically in after logging in via SFTP. The SFTP access for your App **{{app-name}}** is:

* **Server**: `deploy.{{region}}.frbit.com`
* **User name**: `{{ssh-user}}`
* **Password**: `{{ssh-password}}`

When the upload is finished, visit [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) in your browser and commence with the guided web installation to finish the setup.

After the guided web setup is done, you will be automatically redirected to the Craft CMS admin login form. Use the previously chosen Username and Password to login. Your Craft CMS is now live under 😋:

* [{{app-name}}.frb.io](https://{{app-name}}.frb.io) _< Craft CMS installation_
* [{{app-name}}.frb.io/admin](https://{{app-name}}.frb.io/admin) _< Craft CMS admin_
