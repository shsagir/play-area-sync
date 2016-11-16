---

template:      article
naviTitle:     "Professional Stack"
title:         "About the Professional Stack"
reviewed:      2016-11-11
lead:          'With each App you create, you can choose between two technology stacks. This article helps you to understand the Professional Stack.'
group:         platform
stack:         pro

---

The Professional Stack is made for modern PHP development workflows building modern web applications. It subscribes to the [12-factor paradigm](https://12factor.net/)

### Scaling

Professional Apps are designed to be scalable. Any Professional App consists of multiple Components: PHP, [MySQL](mysql), [Memcache](memcache-pro), [Object Storage](object-storage). Think of Components as micro services. Some Components are integral — meaning: you can't turn them off — while others are optional. Each Component is individually scalable, allowing you to increase the resources where needed instead of having to pay for additional resources you don't need. Some Components are capped to an upper limit, but other Components can be scaled virtually infinitely.


### Target audience

To work with Professional Apps, you need to be familiar with modern patterns and workflows like: file system abstraction, caching, Git, Composer. If that matches your skill set, then the good news is: We offer the perfect infrastructure for you.

### Application types

It's less a question of application type and more a question of underlying frameworks/CMS. Not all are up2date in terms of modern PHP development. Some do not support storage abstraction, others are hard to combine with Git-only deployment. Either way: Most maintained frameworks/CMS are moving towards support of 12-factor environments, so that developers using them can leverage truly scalable infrastructures as well. To name a few of those framework/CMS which are easily usable:

* Laravel (since 5.0)
* Symfony (since 2.3)
* Drupal (since 8.2)
* Craft (since 2.5)

### Ephemeral storage

Ephemeral storage is a concept of the [12-factor model](https://12factor.net/), which says: Whenever you deploy new code changes, all (since your last deploy)  generated runtime data will be deleted. The best example for runtime data are user uploads.

This behavior sounds harsh on first reading, but makes sense if you dig deeper: Allowing to scale your App to high degrees implies that the App's resources must be horizontally scaled. Horizontal scaling, in contrast to vertical scaling, is the technique of adding more resources (more servers), instead of using bigger resources (bigger servers).

To allow an App to scale horizontally means that it runs on multiple servers on the same time. Those servers do not share a storage and all solutions, which would allow sharing storage, would need to build on a network storage, which is infamously slow by nature. To still allow persistent runtime data, your App needs to use an cloud storage, like our [Object Storage](object-storage) or [S3 from AWS](https://aws.amazon.com/s3/) or equivalent.

You still can use the regular file system, eg for temporarily storing uploads, but upon every deploy your App will only consist of your deployed code.

> The filesystem … can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database. The twelve-factor app never assumes that anything cached … on disk will be available on a future request or job … a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.


#### Logs

As a result of the horizontal scalable resources and the ephemeral storage, [logs can only be accessed live](logging-pro). If you need historic logs, we recommend [Logentries](logentries).

### High availability

Most plans are available as Development and Production. All Production plans, and those which are not grouped into both, are highly available.















<!--

TODO: this is content from an "pro-stack" article, merge it's content with the above


## File system access

[Old Apps](app-old) and [Universal Apps](app-uni) have a **persistent storage**: you have access via SSH and the App can permanently write on the file system. You can upload files and they will stay there.

New Apps have an **ephemeral storage**: all files will be replaced on each Git deployment. There is no place for runtime data. In other words: your App should not write any relevant data (user uploads) you might want to reuse later on the local file system.

You'll need some efforts from your side to have uploads working (again).

We provide an additional Component to store uploads, static assets and any kind of runtime data offshore. It's called [Object Storage](/object-storage) and comes with some more benefits. It's mostly compatible to AWS S3, so you'll find easy to integrate it to your CMS and framework. File-system abstraction is the key. Please see our [Object Storage article](/object-storage) for more.


## Only Git, no SSH/SFTP

The Old Apps offered [Git](git) and [SSH](ssh-sftp-old) deployment. With the New Apps you only deploy with Git. That means you have no longer direct file access to the actual "webspace" of your App.



## Atomic deployment

The Old Apps Git offer a synchronization deployment mode: the Git repo contents gets rsynced file by file. Per default this is non-destructive, new files and changes would replace old files, but no files will be deleted (overwrite but not delete).

The New Apps deployment is [atomic](http://blog.fortrabbit.com/new-apps-are-here). That means that a completely new release package will substitute the old one on every deploy. We have made a [behind the scenes video](deployment-architecture-video) showcasing what is happening in the background. The atomic deployment also features a more robust build process: your code will only be released if all scripts (composer install, optional pre- and post-deploy scripts) succeed.

Everything will feel familiar. You will just notice a different, more detailed Git deploy log.


## Composer is standard

With Old Apps you had to trigger the dependency manager [Composer](composer). Now with the New Apps Composer is a standard part of the deployment. As long as your repository contains a `composer.lock` and `composer.json` file (on top level), a `composer install` will be executed on every `git push`. The first Composer installation can take some time (depending on the amount of packages you are using). Any subsequent deployment will re-use the vendor folder and is thereby fast.


## New deployment file version

You can fine-tune your build with additional scripts and variables by creating a file called `fortrabbit.yml` on top-level of your repository. Old Apps are using the [deployment file version 1](deployment-file-v1-old), New Apps are using [deployment file version 2](deployment-file-v2).



## No Git submodules

Git submodules are not supported by New Apps. We recommend to use Git subtrees instead. See [this post from Atlassian](http://blogs.atlassian.com/2013/05/alternatives-to-git-submodule-git-subtree/).


## HTTP-Auth without htaccess directive

New Apps don't require you to write a [.htaccess directive](http-auth) any more.


## App repository reset

Only for New Apps: to start with a complete new Git history, you can now reset your repository. This can be done with the `reset` command like so:

```bash
ssh {{ssh-user}}@deploy.{{region}}.frbit.com reset
```

The reset operation is non-destructive, meaning: It does not generate a release. Thereby your live App continues to operate without any interruption. The new release will only be build on the next push (of your new code base). A repository reset also removes any sustained directory (the `vendor` folder, so a following [Composer](composer) install requires to download everything once again).


## Private repositories

Old Apps offered a keygen hook to realize access to [private Composer repos](private-composer-repos). With the New Apps you can create a new SSH keypair for your App, using the `keygen` command:

```bash
ssh {{ssh-user}}@deploy.{{region}}.frbit.com keygen
```

This will print out a public key, which you can then install with your private repository (on Github, Bitbucket or wherever).


## Simplified branching for multi-staging setups

Usually, only the remote Git `master` branch will be deployed. With the New Apps you can also create a branch with the same name as your App, which will be preferred over the master branch. That way it is easier to set up a [multi-staging environment](multi-staging).


## New streaming log access

With the Old Apps you can access the logs via SSH. New Apps don't offer direct file access (nor historical logs) but you can tail a stream of live logs.


### Sources

```bash
# Per default all sources are tailed together:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail

# Only Apache access log, all incoming requests with response status, time-stamp, additional headers and the first line of the request:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access

# Only Apache error log, which can be very helpful to debug `.htaccess` files or the like:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_error

# Only PHP error logs, which contain whatever your App writes on `error_log()`:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_php_error

# Only web standard error output, which containins everything written by your App to `STDERR`:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr
```

### Other parameters

```bash
# The `source` parameter can be use multiple times:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access source:apache_error

# Use the `mono` parameter to disable output colors:
ssh {{ssh-user}}@log.{{region}}.frbit.com tail mono
```

## Further readings

We have also written a comprehensive [blog post](http://blog.fortrabbit.com/new-apps-are-here) about the features, benefits, limits and backgrounds of the ~~New~~ Professional Apps.

-->