---

template:      article
reviewed:      2016-05-30
title:         Remote SSH commands
naviTitle:     SSH
lead:          How to execute remote SSH commands in your App.
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


## Problem

With fortrabbit you [deploy with Git](/deployment). But sometimes SSH access to your App can be helpful as well. fortrabbit Apps have [ephemeral storage](quirks#toc-ephemeral-storage) and a horizontal scalable infrastructure. This allows great performance and grants high availability. One trade of is that full SSH access is not feasible.

## Solution

Remote SSH execution: run single commands remotely using the SSH remote exec protocol.

### Application scenarios

* Use CLI tools for tasks like database migration (see below)
* List files on remote, see what actually has been deployed
* Debug your [workers](/workers) and crons
* Debug your post/pre deploy scripts
* Integrate third party services (like Envoyer)
* Execute PHP scripts on remote
* …


## Usage

All you need to do is: prefix the command you want to execute remotely with the SSH login command. For example:

```
$ ssh your-app@deploy.eu2.frbit.com php htdocs/script.php
# \                               / \                   /
#  --------------|---------------    ---------|---------
#                v                            v
#      the ssh login command          the remote command
```

**Note**: In the example you can see that the `htdocs` path is placed before the `script.php` file. Your App has the "home" folder `/srv/app/your-app` from where all commands will be executed. The "home" folder contains the sub folder `htdocs` which in turn contains your App's code, which is deployed via Git. So if the file `script.php` is top-level in your local code directory, then it will be deployed into the `htdocs` sub folder of your App. Read more about the directory structure [here](/directory-structure).

So you see: the main difference of the SSH remote exec to a "full SSH environment" is that you can only execute one command at once and that you specify the command to be executed with the SSH login command line.

### Authentication

<!-- TODO: rewrite on password-username launch -->

For remote SSH execution you identify using your public SSH keys. If you get a connection error, please check if your SSH key setup is correct and if in doubt refer to our [SSH key troubleshooting](/ssh-keys) guide.


### Available commands

* ls
* …


### Using CLI tools

Many modern web development frameworks and CMS come with a programmable command line interfaces (CLI), Drush and Artisan for example. These CLIs allow you to automate often occurring tasks, help you to develop your App via code generators and provide useful helpers to set up new installations. Executing commands, like database migration, can hugely simplify development.


### Limitations

**Nothing that leaves a permanent result on disk**: Remember the [ephemeral storage](quirks#toc-ephemeral-storage). Everything you'll write to the local file storage will be wiped on change of settings or deployment.

**No deployment via SSH/sFTP**: For the same reason as above: Don't use SSH to deploy code, use Git instead.

**No code manipulation**: Code changes made via remote SSH execution don't have any effect in your running App! This means: You can run the migrate command, which changes the contents of your database and thereby has effect on your App. If your remote command instead modifies or removes a file, it will only be modified or removed from the "deployment Container" in which the SSH command was executed, not in the "Web Container", which is used to serve your App to the web. All code changes need to be made locally and then [committed and pushed via Git](/deployment).

**Concurrent usage**: To guarantee code consistency no deployments can be made while a remote SSH command is executing. For the same reasons parallel deployments are not allowed.

**Limited execution time**: The maximal execution time for remote SSH commands is [limited](https://www.fortrabbit.com/specs#limits).

* **No background execution**: Commands cannot be detached to the background. That means: When the command execution is finished and you are back on your local shell, then the remote command execution has terminated as well.

### Commands that are not available

* mkdir
* …


## Advanced usage

Still reading, go ahead, dive deeper!

### Chaining

You can execute multiple commands at once by quoting everything and separating the commands with a semicolon:

```
# execute three scripts
$ ssh your-app@deploy.eu2.frbit.com "php htdocs/foo.php ; php htdocs/bar.php ; php htdocs/baz.php"
```

### Aliases

On Mac and Linux: You can define local "aliases", which reduce the amount typing when executing a remote SSH command by a lot. For example:

```
# create the alias
$ alias artisan-my-app="ssh your-app@deploy.eu2.frbit.com php htdocs/artisan"

# use the alias
$ artisan-my-app migrate
# this actuall calls "ssh your-app@deploy.eu2.frbit.com php htdocs/artisan migrate"
```

To persist those aliases between shell sessions you need to add them to your shell profile file. Usually that is either `~/.profile`, `~/.zshrc` or `~/.bashrc`.
