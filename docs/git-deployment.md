---

template:      article
reviewed:      2016-12-20
naviTitle:     Git deployment
title:         Deploy with Git on fortrabbit
lead:          Learn how to get your code up and running with a simple git push.
group:         deployment
stack:         all
oldLink:       git-old

keywords:
    - beginner
    - dashboard
    - deployment

---

## Git ready

We assume that you have: [Git installed](git) locally and know the basics. We further assume that you know about [access methods](/access-methods) here on fortrabbit, so either have your SSH keys installed or your [Dashboard](/dashboard) password handy.

## Usage

Each fortrabbit App comes with its own Git repo. Set this as a Git remote in your local Git working copy. To deploy just push your code to that remotes master branch.

### Simple Git deployment workflow

```bash
# 1. Clone the (empty) app to register the remote origin master
$ git clone {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# 2. Go in the folder
$ cd {{app-name}}

# 3. Do stuff
$ echo '<?php echo "PHPower to the PHPeople";' >index.php

# 4. Push to deploy
$ git add index.php
$ git commit -am 'Intial commit'
$ git push -u origin master
```
after the deployment is done you can worship your work in the browser:
[{{app-name}}.frb.io](https://{{app-name}}.frb.io)

Also see our specific install guides for [Laravel](/install-laravel), [Symfony](/install-symfony), [Craft CMS](/install-craft), [WordPress](/install-wordpress) …


### Adding fortrabbit as a remote

```
# Using Git already? Add fortrabbit as additional remote:
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git
```

### Resetting the remote repo

To start with a complete new Git history, you can now reset your repository. This can be done with the `reset` command like so:

```
# Reset the remote repo (delete remote Git repo & vendor folder):
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com reset
```


The reset operation is non-destructive, meaning: It does not generate a release. Thereby your live App continues to operate without any interruption. The new release will only be build on the next push (of your new code base). A repository reset also removes any sustained directory (the `vendor` folder, so a following [Composer](composer) install requires to download everything once again).


### Git with a GUI or IDE

You can also use a graphical interface like Tower, SourceTree, Gitbox and so on - see the [official list of Git GUIs](https://git-scm.com/downloads/guis) – or an IDE like PhpStorm or Eclipse to manage Git. You'll need these access credentials:

* **SSH clone URL**: {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}
* **SSH password**: {{ssh-password}}

## Advanced usage

Still reading? Dig deeper!


### Git deployment vs Universal Apps

Universal Apps have persistent storage, which you can access via [SSH](ssh-uni) or [SFTP](sftp-uni). It further means, that runtime data, like user uploads, are persistent and *will not be removed* upon Git push.

To make sure nothing is deleted, all git deployments to Universal Apps follow an **overwrite but not delete** strategy which is thoroughly explained in the [deployment methods article](deployment-methods-uni#toc-git-push-overwrite-but-not-deletes).

### Behind the scenes

```nohighlight
┌─────────┐             ┌──────────┐             ┌───────────┐
│         │             │          │             │           │
│   You   ├──Git push───▶ Git repo ├───deploy────▶    App    │
│         │             │          │             │           │
└─────────┘             └──────────┘             └───────────┘
```

Your `git push` updates the Git remote on fortrabbit and triggers the build of a new release package. This new release package will then be distributed to all the Nodes your App runs on. All files on the Nodes will then be replaced by the ones contained in the release package. Check out [this video](deployment-architecture-video) to understand the flow.


### The branch name counts

While you can have as many Git branches you want, only changes in certain branches will be deployed. The `master` branch is the default one.


### Branching for multi-staging setups

Usually, only the remote Git `master` branch will be deployed. You can also create a branch with the same name as your App, which will be preferred over the master branch. That way it is easier to set up a [multi-staging environment](multi-staging-pro).


### Deployment file

Fine tune deployment configurations with the `fortrabbit.yml` deployment file: control the way Composer runs, define pre- & post-deploy scripts and more. You can find a full example [here](deployment-file-v2).

### Composer

[Composer](composer) install will always run after a successful `git push` - as long as both `composer.json` and `composer.lock` are present. Read on [here](composer).

### Private Git/Composer repos

You can also include your own private Composer repository as described [here](private-composer-repos).

### Large files

We don't actively enforce a file limit, but you should not put big binary files (> 2 MB) into your Git repo. It would bloat your repository and slows down deployment.


### Deployment release package

The release package is a gzipped archive which contains: the Apps Git repository + vendor folder + all files generated by pre or post deployment scripts. In short: compressed size of ``~/htdocs``.

To keep deployment fast for everyone the size of the release package is [limited](http://www.fortrabbit.com/specs#limits). You can find out the release package size locally by packaging the local copy of your App, including the `vendor` folder and anything else generated by your build scripts:

```bash
$ tar -zcf my-project.tar.gz {{app-name}}
$ ls -lh {{app-name}}.tar.gz
```

### Integrating with GitHub & Bitbucket

We don't have any fancy GitHub/Bitbucket integrations (yet). But it is easily possible to combine your fortrabbit repo with [GitHub](github) and [Bitbucket](bitbucket).


## Don't use Git submodules

Git submodules are not supported. We recommend to use Git subtrees instead. See [this post from Atlassian](http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/).
