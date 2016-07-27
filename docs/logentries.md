---

template:         article
naviTitle:        Logentries
reviewed:         2016-07-27
title:            Using Logentries with fortrabbit

group:            Logging
section:          Extending_fortrabbit

image:            logentries-logo.svg
websiteLink:      https://logentries.com?utm_source=fortrabbit
websiteLinkText:  logentries.com
category:         logging

tags:
     - advanced

keywords:
     - logging
     - log
     - logs

seeAlsoLinks:
    - about-databases

---


## About LogEntries

Logentries is a software as a service provider for log management and intelligence.


## Pricing

It starts with a free plan with an fair amount of storage and a week retention included. From there you can continue with larger storage sizes and longer retentions.


## Signing Up

Go to the [Logentries sign up page](https://logentries.com/get-started-now/?utm_referrer=https%3A%2F%2Fhelp.fortrabbit.com&utm_source=fortrabbit) and enter your personal information to sign up for an account.


## Booking

Once you are logged in click on the "+ Add New" button in the upper left corner. Choose "Add a Log". In the next dialog click on PHP in the "Libraries" section. Now name your new Log (source) appropriately. For example `frbit-your-app-name` then click on "Create Log Token" below. Make sure you save the token somewhere - you'll need it later on. Then finish the setup by clicking "Finish & View Log" below.

## Connecting

Stash the just created token in your App's [secrets](app-secrets). Go to the fortrabbit Dasboard > Your App > Settings > App Secrets and add:

```plain
LOGENTRIES_TOKEN=the-token-you-just-got
```

### 1. Request a firewall white-listing

Logentries needs the ports 10000 and 20000 to be open. By default all outgoing calls to non-standard ports from your fortrabbit App are blocked for [security](security) reasons. But you can request the fortrabbit team to open up any port for you. That doesn't take long and isn't complicated.

Navigate in the fortrabbit [Dashboard](dashboard) to your App > Settings > Firewall whitelist and request new custom firewall rules - one for each of the ports. Write nothing under the optional IP field. As descriptions we suggest "Logentries" for port 10000 and "Logentries (TLS)" for port 20000 - or the like. Once your request has been approved, which usually takes not very long, you are ready to use Logentries with your App!

## Using Logentries

There are various PHP libraries which you can use to send your logs to Logentries. Here are the two most interesting contenders:

* The [official library](https://github.com/logentries/le_php), which should work with any PHP application
* The [Monolog adapter](https://github.com/logentries/logentries-monolog-handler), which works perfectly with any App using Monolog already
