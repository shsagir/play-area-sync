---

template:      article
reviewed:      2016-11-10
title:         SSH
naviTitle:     SSH
lead:          Learn what you can do on the command line with fortrabbit Apps.

stack:         uni
oldLink:       ssh-sftp-old
proLink:       remote-ssh-execution-pro
group:         deployment

keywords:
    - beginner
    - deployment

---

Using our [Git deployment](git-deployment) is great, but sometimes need a little more control. That's where the Secure shell comes in. SSH is the big brother of [SFTP](sftp-uni) and allows you to directly interact with your App's code.


## Accessing SSH

Execute the following in your terminal:

```bash
# Login to your App by SSH like so:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
# Unless you are using public key: You will be asked for your Account password
```

When it worked, you will see small a welcome screen:

```
–––––––––––––––––––––––  ∙ƒ  –––––––––––––––––––––––
```

You are now logged in to your App by SSH. See [below](#toc-troubleshooting-authentication) when you got an error.

## Things to do with SSH

Following a few simple examples, just to give you some ideas:

```bash
# Download & unpack wordpress
$ curl https://wordpress.org/latest.tar.gz | tar zx

# Stream your Apache access logs
$ tail -f ../logs/apache_access.log

# Search for 50x responses in apache access
$ grep -E ' 50[0-9] ' ../logs/apache_access.log
```

Also checkout: [WordPress install from SSH](install-wordpress-4-uni#toc-installing-wordpress-with-ssh), [Execute Laravel's artisan](install-laravel-5-uni#toc-migrate-amp-other-artisan-commands).


### Using CLIs

We pre-installed various CLIs for you:

* [WP-CLI](http://wp-cli.org/): eg `wp theme ...`
* [Drush](http://www.drush.org/en/master/): eg `drush topic`
* [Drupal Console](https://www.drupal.org/project/console): eg `drupal check`


### Executing PHP scripts

If you want to execute PHP scripts, including `artisan` and it's like, make sure to specifcy the PHP interpreter explicity:

```bash
# will work
$ php artisan some:command
$ php some-script.php

# will _not_ work:
$ ./artisan some:command
$ ./some-script.php
```

### Syncing code with rsync

The command line tool `rsync` grants a fast and reliable way to upload your code changes. As the name implies, `rsync` is made to synchronize (two) data sets. Following an example showcasing the synchronization of a WordPress plugin from local to fortrabbit:

```shell
# change to the folder above your plugin folder locally
$ cd local-app-folder/wp-content/plugins

# execute synchronization
$ rsync -az --delete custom-plugin/ {{ssh-user}}@deploy.{{region}}.frbit.com:~/wp-content/plugins/custom-plugin/
```

The above command assures that the remote folder `custom-plugin` contains exactly what your local folder of the same name contains. The exemplified `--delete` flag will remove all remove files, which do not exist locally anymore. You can safely omit it, if that you just want to update changed files, but do not remove any file.


### Using Composer

Using [Git deployment](git-deployment) will trigger [Composer](composer) automatically. If you need to run Composer manually, just execute the following when logged in via SSH:

```bash
$ composer install
```

## Limitations

* This is not a root shell, so you can't install or remove software packages
* Mind that you are using the same runtime as your web application: resource intensive operations will drain memory and CPU from the web execution
* See the [specs page](https://fortrabbit.com/specs) for more limits and numbers


## Troubleshooting authentication

Got an error when trying to login? fortrabbit supports username + password and public key authentication. The latter is recommend, but, especially when using Windows, sometimes password is the only feasible option. Please go on here:

* [See the access methods article](access-methods)
