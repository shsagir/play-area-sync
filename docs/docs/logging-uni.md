---

template:   article
reviewed:   2019-11-08
title:      About logs on fortrabbit
naviTitle:  Logging
lead:       Accessing logs of your App is essential for developing. Here is how you can do it on fortrabbit.
group:      deployment
stack:      uni

proLink:    logging-pro

keywords:
    - Logging
    - Logs

---

You are developing your App and see the "white screen of death". You are getting a 5xx error and don't know why. You write debug logs and need them to trace a problem.

## Log file access

You can access your log files from [SSH](ssh-uni) or [SFTP](sftp-uni). They are stored in the folder `logs`, right above the `htdocs` folder. The `log` folder contains up to 8 files, by type of log and time:

```bash
$ ls -1 ../logs

# Apache access logs (current and historic)
apache_access.log
apache_access.log.1.gz

# Optional cron logs (current and historic)
cron.log
cron.log.1.gz

# PHP error logs (`error_log`)
php_error.log
php_error.log.1.gz

# PHP STDERR output
stderr.log
stderr.log.1.gz
```

The `.log` file contains the latest logs, the `.gz` (gzipped) file contains older logs for each type.

### Log files in your CMS/framework

Mind that your CMS or framework might store logs in a different location. The basic concepts here will still apply, but the location and file name of the logs might be different. You might also can turn on more verbose logging output. See specific guides here:

* [Craft CMS logging](/craft-3-tune#toc-logging)
* [Laravel logging](/install-laravel-5-uni#toc-logging)


## Live log access

Use the SSH logging [SSH remote command](remote-ssh-execution) in the terminal to get a live streams of all the logs for your Apps:

```bash
# All sources tailed together:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail

# Only Apache access logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access
# incoming requests with response status, time-stamp, additional headers & first line of request

# Only Apache error log:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_error
# helpful to debug `.htaccess` files and alike

# Only Object Storage access log:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:objects_access
# See requests when and from whom

# Only PHP error logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_php_error
# contains whatever your App writes on `error_log()`

# Only web standard error output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr
# contains everything written by your App to `STDERR`


# Only Cron Job or Nonstop Job output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:worker
# Worker must be enabled

# --------------- PRO TIPS ---------------

# Use the mono flag to force monochrome output, if your console displays colors incorrectly:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail {{app-name}} mono

# Use multiple source:name parameters at once:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr source:web_php_error
```
