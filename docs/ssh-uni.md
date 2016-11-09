---

template:      article
reviewed:      2016-11-07
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


[Git deployment](git-deployment) is fancy, but sometimes you also want a little more control. That's where the Secure shell comes in. SSH is the big brother of [SFTP](sftp-uni).

## Accessing SSH

Execute the following in your terminal:

```bash
# Login to your App by SSH like so:
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
# You might be asked for your Account password then
```

When it worked, you will see a welcome screen. 

<!-- TODO: dump example of SSH screen (this is marketing) -->

You are now logged in to your App by SSH. See [below](#toc-troubleshooting-authentication) when you got an error.

## Things to do with SSH

Now this is what you can do:

<!-- TODO: some more examples here please!  "wget and unpack example" with links to CMSs, what about logs? -->


### Using CLIs

We pre-installed various CLIs for you:

<!-- TODO: what about Artisan CLI? -->

* [WP-CLI](http://wp-cli.org/): eg `wp theme ...`
* [Drush](http://www.drush.org/en/master/): eg `drush topic`
* [Drupal Console](https://www.drupal.org/project/console): eg `drupal check`


### Executing PHP

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


### Using Composer on remote

<!-- TODO: write some more about Composer here and the alternative way to trigger Composer via Git … -->

You can also use `composer install` directly via SSH. 

<!-- TODO: 

## Limits

write something about limits and modules that are not installed or that one can not install software and link to specs article for execution time and otther possible limits 

-->


## Troubleshooting authentication

Got an error when trying to login? fortrabbit supports username + password and public key authentication. The latter is recommend, but, especially when using Windows, sometimes password is the only feasible option. Please go on here:

* [See the access methods article](access-methods)