---

template:   article
reviewed:   2016-07-22
title:      About logs on fortrabbit
naviTitle:  Logging
lead:       Accessing live logs of your App is essential for developing. Here is how you can do it on fortrabbit.
group:      deployment

seeAlsoLinks:
    - remote-ssh-execution


keywords:
    - Logging
    - Logs

tags:
    - beginner

---

You are developing your App and see the white screen of death. You are getting a 5xx error and don't know why. You write debug logs and need them to trace a problem.


## Log file access

Use the SSH logging command in the terminal to get a live streams of all the logs for your Apps:

```bash
# All sources tailed together:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail

# Only Apache access logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_access
# incoming requests with response status, time-stamp, additional headers & first line of request

# Only Apache error log:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:apache_error
# helpful to debug `.htaccess` files and alike

# Only PHP error logs:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_php_error
# contains whatever your App writes on `error_log()`

# Only web standard error output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:web_stderr
# contains everything written by your App to `STDERR`


# Only Cron Job or Nonstop Job output:
$ ssh {{ssh-user}}@log.{{region}}.frbit.com tail source:worker
# Worker must be enabled
```

**Hint**: Use the `mono` flag to force monochrome output, if your console displays the colors incorrectly: `ssh {{ssh-user}}@log.{{region}}.frbit.com tail {{app-name}} mono`.

**Hint 2**: You can use multiple `source:name` parameters at once.
