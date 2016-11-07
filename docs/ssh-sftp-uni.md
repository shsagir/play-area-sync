---

template:      article
reviewed:      2016-11-07
title:         SSH/SFTP
naviTitle:     SSH/SFTP
lead:          Learn about the classical way to deploy and access your App on fortrabbit.

stack:         uni
oldLink:       ssh-sftp-old
group:         platform

keywords:
    - beginner
    - deployment

---

## Problem

Albeit [deploying with Git](git-deployment) has many advantages, sometimes it's more of a burden then it helps. Especially when working with CMS, which support plugin installation, version upgrades and other functions, which make it hard to use version control independently.

## Solution

All Universal Apps come with SSH and SFTP access out-of-the-box.

## Authentication

fortrabbit supports username + password and public key authentication. The latter is recommend, but, especially when using Windows, sometimes password is the only faeasible option. Please read on in a dedicated article on how to utilize and setup [public key](access-methods#toc-ssh-key-authentication) or plain old [password](access-methods#toc-password-authentication) authentication.

## Access SFTP

There are various GUIs out there, which make your life easier. We recommend [Cyberduck](https://cyberduck.io/) (Mac, Windows).

<!-- TODO: Describe configuration of Cyberduck connection -->

## File synchronization

Most SFTP clients feature a file synchronization mode. You can choose your local folder and sync it to the remote folder on fortrabbit. All files will be compared and only changed ones will be uploaded. This works in the other direction as well, of course. Many modern editors or IDEs also feature synchronization tools or are extendible by plugins to that end.

### Syncing code with rsync

The command line tool `rsync` grants a fast and reliable way to upload your code changes. As the name implies, `rsync` is made to synchronize (two) data sets. Following an example showcasing the synchronization of a WordPress plugin from local to fortrabbit:

```shell
# change to the folder above your plugin folder locally
$ cd local-app-folder/wp-content/plugins

# execute synchronization
$ rsync -az --delete custom-plugin/ {{ssh-user}}@deploy.{{region}}.frbit.com:~/wp-content/plugins/custom-plugin/
```

The above command assures that the remote folder `custom-plugin` contains exactly what your local folder of the same name contains. The exemplified `--delete` flag will remove all remove files, which do not exist locally anymore. You can safely omit it, if that you just want to update changed files, but do not remove any file.


## Access SSH

It's as simple as that:

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
```

If you want to execute PHP scripts, including `artisan` and it's like, make sure to specifcy the PHP interpreter explicity:

```bash
# will work
$ php artisan some:command
$ php some-script.php

# will _not_ work:
$ ./artisan some:command
$ ./some-script.php
```
