---

template:     article
reviewed:     2016-11-30
title:        Using Cron Jobs
naviTitle:    Cron Jobs
lead:         Use time scheduled cron jobs to execute long running operations in the background. They run at defined times, independent of visits to the web application.
group:        platform
stack:        uni

---



## Use cases

**Database maintenance**: say the web application cumulates data which needs to be transformed and/or wiped periodically. A Cron Job allows you to make sure the `app/console db:cleanup` - or whatever - script executes hourly, daily, weekly or whenever your want.

**Cache clearing**: say the web application has a news site, which homepage must be rebuilt every ten minutes or so. With a Cron Job you can schedule a cleanup of the homepage every one, ten, thirty or whatever minutes required.


## Availabilty

Cron Jobs are only availble for certain App plans. Please see the [plans & pricing page](https://www.fortrabbit.com/pricing) for more.

## Usage

You can configure your App's cron job in the Dashboard > {{app-name}} > Settings > Cron Jobs. To start a new Cron Job you'll need the set the following parameters:

### Name

A unique name, so the job can be identified later on in the logs or statistics.

### Command

The PHP command which shall be executed, eg `app/console db:cleanup` or `path/to/my-script.php`

### Interval

The interval at which you want to execute the job.

1. every minute
2. every 10 minutes
2. every 30 minutes
3. every hour
4. every day
5. every week
6. every month

The interval timing is guaranteed, the exact time of execution is randomized. For example: 30 minutes will run at every 13th minute and at the 43rd minute again. All daily, weekly and monthly jobs run between 00:00 and 10:00 UTC. Weekly intervals will run on Monday.

### Status

This optional, you can 


## Alternatives to Cron Jobs

Sometimes you might just want to run a small not compute intensive script. So the above described solutions might be a bit over-sized. Maybe just use an external cron job service for this.
