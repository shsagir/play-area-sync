---

template:      article
reviewed:      2016-05-30
title:         Remote SSH commands
naviTitle:     SSH
lead:          Executing remote SSH commands in your App.
group:         Kitchen_sink

seeAlsoLinks:
    - deployment
    - ssh-keys
    - deployment-architecture-video

otherVersionLinks:
    - ssh-sftp-old-app

tags:
    - git
    - advanced

---

Many modern web development frameworks come with a programmable command line interfaces (CLI). These CLIs allow you to automate often occurring tasks, help you to developer your App via code generators and provide useful helpers to set up new installations.

## Problem

Especially when working with a framework, executing some commands, like database migration, can hugely simplify development. fortrabbit Apps are based on a horizontal scalable infrastructure. This allows great performance and grants high availability. One trade of is that SSH access is not easily feasible.


## Solution

Instead of a full SSH access, which has implications and requirements conflicting with a horizontal scalable infrastructure, you can execute single commands remotely using the SSH remote exec protocol.


## Usage

All you need to do is prefix the command you want to execute remotely with the SSH login command. For example:

```
$ ssh your-app@deploy.eu2.frbit.com php htdocs/script.php
# \                               / \                   /
#  --------------|---------------    ---------|---------
#                v                            v
#      the ssh login command          the remote command
```

**Note**: In the example you can see that the `htdocs` path is placed before the `script.php` file. Your App has the "home" folder `/srv/app/your-app` from where all commands will be executed. The "home" folder contains the sub folder `htdocs` which in turn contains your App's code, which is deployed via Git. So if the file `script.php` is top-level in your local code directory, then it will be deployed into the `htdocs` sub folder of your App. Read more about the [directory structure](/directory-structure).

So the main difference of the SSH remote exec to a "full SSH environment" is that you can only execute one command at once and that you specify the command to be executed with the SSH login command line.


### Limitations

* **No code manipulation**: Code changes made via remote SSH execution don't have any effect in your running App! This means: You can run the migrate command, which changes the contents of your database and thereby has effect on your App. If your remote command instead modifies or removes a file, it will only be modified or removed from the "deployment Container" in which the SSH command was executed, not in the "Web Container", which is used to serve your App to the web. All code changes need to be made locally and then [committed and pushed via Git](/deployment).
* **Concurrent usage**: To guarantee code consistency no deployments can be made while a remote SSH command is executing. For the same reasons parallel deployments are not allowed.
* **Limited execution time**: The maximal execution time for remote SSH commands is [limited](/specs).
* **No background execution**: Commands cannot be detached to the background. That means: When the command execution is finished and you are back on your local shell, then the remote command execution has terminated as well.

### Advanced

#### Chaining

You can execute multiple commands at once by quoting everything and separating the commands with a semicolon:

```
# execute three scripts
$ ssh your-app@deploy.eu2.frbit.com "php htdocs/foo.php ; php htdocs/bar.php ; php htdocs/baz.php"
```

#### Aliases

On Mac and Linux: You can define local "aliases", which reduce the amount typing when executing a remote SSH command by a lot. For example:

```
# create the alias
$ alias artisan-my-app="ssh your-app@deploy.eu2.frbit.com php htdocs/artisan"

# use the alias
$ artisan-my-app migrate
# this actuall calls "ssh your-app@deploy.eu2.frbit.com php htdocs/artisan migrate"
```

To persist those aliases between shell sessions you need to add them to your shell profile file. Usually that is either `~/.profile`, `~/.zshrc` or `~/.bashrc`.
