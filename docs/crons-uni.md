---

template:     article
reviewed:     2016-09-15
title:        Crons
naviTitle:    Crons
lead:         Use time scheduled cron jobs to execute long running operations in the background
group:        platform
stack:        uni

---

Cron Jobs are time scheduled PHP executions. They run at defined times, independent of visits to the web application.

## Use cases

**Example: Database maintenance**: say the web application cumulates data which needs to be transformed and/or wiped periodically. A Cron Job allows you to make sure the `app/console db:cleanup` - or whatever - script executes hourly, daily, weekly or whenever your want.

**Example: Cache clearing**: say the web application has a news site, which homepage must be rebuilt every ten minutes or so. With a Cron Job you can schedule a cleanup of the homepage every one, ten, thirty or whatever minutes required.

## Usage

You can configure your App's cron job in the Dashboard > {{app-name}} > Cron Job:

* **Name**: A unique name, so the job can be identified later on in the logs or statistics.
* **Command**: The PHP command which shall be executed, eg `app/console db:cleanup` or `path/to/my-script.php`
* **Interval**: The interval at which you want to execute the job.
* **Status**: You can temporary disable jobs

### Intervals

1. every minute
2. every 10 minutes
2. every 30 minutes
3. every hour
4. every day
5. every week
6. every month

The interval timing is guaranteed, the exact time of execution is randomized. For example: 30 minutes will run at every 13th minute and at the 43rd minute again. All daily, weekly and monthly jobs run between 00:00 and 10:00 UTC. Weekly intervals will run on Monday.


## Alternative to Crons

Sometimes you might just want to run a small not compute intensive script. So the above described solutions might be a bit over-sized. Maybe just use an external cron job service for this.
