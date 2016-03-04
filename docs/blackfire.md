---

template:         article
reviewed:         2016-02-20
title:            Using Blackfire with fortrabbit
naviTitle:        Blackfire
group:            Extending_fortrabbit
group:            Extending_fortrabbit

image:            blackfire-orig.png
websiteLink:      https://blackfire.io?utm_source=fortrabbit
websiteLinkText:  blackfire.io
dataCenters:      n/a


keywords:
    - profiler
    - performance

tags:
  - php
  - advanced
  - profiling
  - performance

seeAlsoLinks:
  - new-relic

---

## About Blackfire

Blackfire is a relatively new profiling service that can deeply look in your PHP web application. It helps you analyze your PHP code. It's made by [SensioLabs](https://sensiolabs.com/) — Créateur de Symfony.



## Integration

To use Blackfire with your fortrabbit App, you only need to paste the Agent credentials from Blackfire into the fortrabbit [Dashboard](/dashboard). Here is a detailed step by step guide:


## Over at Blackfire

* Sign up or log in to [Blackfire](https://blackfire.io).
* Go to your Account () > My Credentials > [My Server Credentials](https://blackfire.io/account/credentials#server)
* Copy the credentials: Server ID and Server Token

## At fortrabbit

* Log in to the dashboard
* Navigate to your App > PHP > Debugging
* Enable Blackfire
* Paste the Blackfire Server ID and Server Token
* Save the PHP Settings

## At your browser

* Install the [Chrome Extension](https://blackfire.io/docs/integrations/chrome)
* Browse to your App
* Hit the Blackfire button to start profiling


## Further readings

* [fortrabbit blog post on Blackfire](http://blog.fortrabbit.com/blackfire-profiler-on-fortrabbit)
