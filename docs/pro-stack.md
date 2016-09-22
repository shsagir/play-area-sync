---

template:      article
reviewed:      2016-09-15
title:         Working with the Professional Stack
naviTitle:     Professional Stack
lead:          Learn how practical use.
group:         platform

---

<!-- TODO: harmonize with stacks article -->


## File system access

Old Apps have a **persistent storage**: you have access via SSH and the App can permanently write on the file system. You can upload files and they will stay there.

New Apps have an **ephemeral storage**: all files will be replaced on each Git deployment. There is no place for runtime data. In other words: your App should not write any relevant data (user uploads) you might want to reuse later on the local file system.

You'll need some efforts from your side to have uploads working (again).

We provide an additional Component to store uploads, static assets and any kind of runtime data offshore. It's called [Object Storage](/object-storage) and comes with some more benefits. It's mostly compatible to AWS S3 and therefore, so you'll find easy to integrate it to your CMS and framework. File-system abstraction is the key. Please see our [Object Storage article](/object-storage) for more.


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

We have also written a comprehensive [blog post](http://blog.fortrabbit.com/new-apps-are-here) about the features, benefits, limits and backgrounds of the New Apps.