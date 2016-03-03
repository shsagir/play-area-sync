---

template:       article
reviewed:       2016-02-22
naviTitle:      External services
title:          Extending fortrabbit with external services
lead:           Learn how to integrate third party services to enhance your fortrabbit Apps.
dontList:       true
dontIndex:      true
group:          Extending_fortrabbit

tags:
    - beginner

keywords:
    - addon
    - add-on
    - integrations
    - integration
    - API
    - CI
    - cloud

---


<!--

TODO: maybe delete this article. there is some good info in here. but currently the plan is to have articles by provider.

-->

### Choosing a data center location

Some services are latency-relevant. We recommend to use a service hosted in the same AWS region as your fortrabbit App. 

## Application helpers

Key-value stores, site search, Redis, Apache Solr, ElasticSearch

* [Openredis](https://openredis.com)
* [Redis-cloud](http://redis-cloud.com)
* [Indexdepot](https://www.indexdepot.com) Search solutions


## Image processing

* [imgix](https://www.imgix.com/)
* [Blitline](https://blitline.com)

## Databases

fortrabbit offers [MySQL](mysql) out of the box. But there are other interesting database types out there. With database abstraction as part of your framework you actually can switch quickly to an alternative. NoSQL databases are quite popular. Commonly used DBaas:

* [ElephantSQL](http://www.elephantsql.com) PostgreSQL Databases
* [Cloudant](https://cloudant.com) CouchDB
* [MongoLab](https://mongolab.com) MongoDB
* [Compose.io](https://www.compose.io) MongoDB, Elasticsearch & RethinkDB


## Transactional mails

OK, you want send your "forgotten passsword link mail" from your App without fuzz. Transactional mails are automated customer relationship messages send by your App. In contrast to newsletters transactional mails are triggered by user action. It's where marketing meets communications.

With fortrabbit you can't use `sendmail` out of the box, so you either use SMTP in combination with a classical mail provider or a specialized provider. They usually offer an API or simply [SMTP](quirks#toc-mailing):

* [Postmark](https://postmarkapp.com)
* [Sendgrid](http://sendgrid.com)
* [Mailjet](https://www.mailjet.com)
* [Mandrill](https://www.mandrill.com)
* [Mailgun](http://www.mailgun.com)

## Cloud storage

OK, this not about iCloud, DropBox or Google Drive here. You don't want to put your large binaries (like for download files) in Git? Good idea! You want to put your generated and compressed static assets like images, javascripts and stylesheets somewhere else? Good idea!

* [Cloud Files](http://www.rackspace.com/cloud/files)
* [DreamObjects](https://www.dreamhost.com//cloud/dreamobjects)
* [Google Cloud Storage](https://developers.google.com/storage)
* [Amazon S3](http://aws.amazon.com/s3) - check out our BLOG[guide to setup S3 as a cloud storage](new-app-cloud-storage-s3)


## Message queuing

OK, you want to make real good use of your [Worker](worker) with fortrabbit? Combine it with a queue service:

* [Iron MQ](http://www.iron.io/mq)
* [CloudAMPQ](http://www.cloudamqp.com)
* [Amazon SQS](http://aws.amazon.com/sqs)


## Uptime monitoring

OK, you want to know if fortrabbit is really up and delivering as promised in the Service Level Agreement? Go ahead, ping your App constantly:

* [PingDom](https://www.pingdom.com)
* [StatusCake](https://www.statuscake.com)
* [UptimeRobot](http://uptimerobot.com)
* [Mist](https://mist.io)


## Performance monitoring

OK, you care about App performance, mostly of your server side scripts? You need to know what is slowing and where your bottle necks are? The first thing is to get some data in.

We have a special [New Relic integration](new-relic).


## Code quality profiling

Apart from open source solutions like Xdebug or Zend Debugger there are commercial services to help you with debugging and finding performance bottlenecks in your code:

* [Blackfire](https://blackfire.io/) ‹ by sensiolabs, [blackfire + fortrabbit](blackfire)
* [Qafoo](http://qafoo.com/)


## Code collaboration

OK, each fortrabbit App has [Git](git) built in. Git itself is a very powerful tool to collaborate on code with others. GitHub is the poster-child Git platform — enhanced Git hosting, making repos explorable and accessible (pull requests, issues, stats, activity log …).  With Git standards alone you can combine fortrabbit and nearly any other code collaboration tool:

* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org)
* [Plan.io](http://plan.io) — hosted Redmine (ticket system) with Git integration

Please also see the [dedicated article about GitHub/Bitbucket integration](bitbucket-github-and-fortrabbit).


## Logging

* [App Enlight](https://appenlight.com)
* [Loggly](http://loggly.com)
* [Sumo Logic](http://www.sumologic.com)
* [Papertrail](https://papertrailapp.com)
* [AirBrake](https://airbrake.io/pages/home)

## Continuous Integration

OK, Git push is all fine. But sometimes you might want to run a test or two before everything gets deployed into production. We care about [Continuous Integration](continuous-integration). You might try one of those CI services:

* [Codeship](https://www.codeship.io)
* [Codeclimate](https://codeclimate.com)
* [Wercker](http://wercker.com)
* [CircleCi](https://circleci.com)
* [Travis CI](https://travis-ci.org)

## Crons

OK, you want weekly status reports, daily cache cleanups or monthly invoice generations? Cron tasks, or short crons, come in handy if you want to execute a task on a specific, reoccurring time.

fortrabbit offers a great [worker & cron](workers) solution, but sometimes this solution might be a bit over-sized to execute a small cron script from time to time. You might use an external service to time your execution of scripts hosted on fortrabbit. Go ahead, but be warned: Cronjob as a Service providers look a mostly antiquated. The good news is that there are free offerings:

* [Easycron](https://www.easycron.com)
* [Mywebcron](http://www.mywebcron.com)
* [Setcronjob](http://www.setcronjob.com)
* [Webcron](http://www.webcron.org)

## SSL certificates

OK, you want to utilize transfer encryption for your custom domain. On the one hand you'll need the fortrabbit [TLS component](tls), on the other hand you need a service willing to sign your certificate, such as:

* [Digicert](https://www.digicert.com)
* [SSLstore](https://www.thesslstore.com)
* [SSL.com](https://www.ssl.com) — US
* [VeriSign](http://www.verisign.com)
* [Comodo](https://ssl.comodo.com)
* [Thawte](http://www.thawte.com/products)
* [GeoTrust](http://www.geotrust.com)
* [Let's encrypt](https://letsencrypt.org/) < a free alternative

AGREE: All those commercial vendors don't look trustfully. Well, that seems to be part of the shady business they are doing.


## Domain registration

OK, you want to register your domain somewhere.

* [iwantmyname](https://iwantmyname.com/)
* [Hover](https://www.hover.com/)
* [Name.com](https://www.name.com/)
* [namecheap](https://www.namecheap.com/)


## DNS as a Service

OK, you have your own domains registered somewhere already. But that service doesn't support CNAME entries or forwards like ALIAS or ANAME? You can use a Domain Name System service to do this:

* [DNS Made Easy](http://www.dnsmadeeasy.com)
* [DNSimple](https://dnsimple.com) — also offering SSL



## CDN

OK, you want your static assets to be near your clients for better performance? Try a Content Delivery Network:

* [Cloudflare](https://www.cloudflare.com) — also offering SSL now
* [MaxCDN](https://www.maxcdn.com)
* [Fastly](https://www.fastly.com/)
* [KeyCDN](https://www.keycdn.com/)


## Payment processing

OK, you are collecting payments from your users, possibly via credit card. Of course you will not store any data in your fortrabbit App. There is payment service providers:

* [Stripe](https://stripe.com)
* [WireCard](http://www.wirecard.com/)
