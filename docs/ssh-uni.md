---

template:      article
reviewed:      2016-11-07
title:         SSH
naviTitle:     SSH
lead:          Learn what you can do on the command line with fortrabbit Apps.

stack:         uni
oldLink:       ssh-sftp-old
proLink:       remote-ssh-execution-pro
group:         platform

keywords:
    - beginner
    - deployment

---


[Git deployment](git-deployment) is fancy, but sometimes you also want a little more control. That's where the Secure shell comes in. SSH is the big brother of [SFTP](sftp-uni).

## Access SSH

Execute the following in your terminal:

```bash
# Login to your App by SSH like so:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
# You might be asked for your Account password in the next step
# Please see below for
```

## Things to do with SSH

## Executing PHP

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


### Pre-installed CLIs

We pre-installed various CLI tools for you:

* [Composer](https://getcomposer.org/): eg `composer install`
* [WP-CLI](http://wp-cli.org/): eg `wp theme ...`
* [Drush](http://www.drush.org/en/master/): eg `drush topic`
* [Drupal Console](https://www.drupal.org/project/console): eg `drupal check`


## Troubleshooting authentication

fortrabbit supports username + password and public key authentication. The latter is recommend, but, especially when using Windows, sometimes password is the only feasible option. Please read on in a dedicated article on how to utilize and setup [public key](access-methods#toc-ssh-key-authentication) or plain old [password](access-methods#toc-password-authentication) authentication.