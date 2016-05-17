---

template:   article
title:      "Quirks & constraints"
naviTitle:  Quirks
lead:       "Limits, restrictions, permissions there are some — aren't there always? Heads up so it doesn't cost you hours of debugging in the wrong direction."

otherVersionLinks:
    - quirks-old-app

group:       Kitchen_sink

seeAlsoLinks:
    - app
    - dashboard
    - app
    - encoding
    - http-auth

tags:
    - beginner

---


## Service location

Mind that we are currently only offering service locations in Ireland (AWS EU). For customers from the US this means some data latency as all requests have to be send over the ocean. Please also consider that all prices are in Euro (€). We are based in Berlin, our time zone is: CET.

## Mailing

Sendmail — the Mail Transfer Agent — is not available on fortrabbit.

In PHP Sendmail is usually used with the `mail()` function. Back in the good 'ol days, this would simply call the shell command `sendmail` from the shell to send a mail directly from the web server to the receiving mail server.

In recent days, this is a really bad practice: your web server can send mails, but not receive any - it is not a mail server. So it is not distinguishable from any home PC part of a bot net. Long story short: it is very likely that your mail will not be received.


### Direct SMTP

Instead of `sendmail` you can use a mail script that uses SMTP (Simple Mail Transfer Protocol) with your e-mail provider - usually the one that provides your domain - directly.

Transactional mail services (see [extending fortrabbit](/#extending-fortrabbit)) can actually do the bulk mailing for you. You can either connect to them via "SMTP relay" or by "API". Those services help you to save your Apps resources, are probably more reliable and have some nice extra features like analytics.

In the following we focus on an SMTP implementation. There are countless possibilities how to do this. Most frameworks and CMS give them to you out of the box. If you use a custom script, have a look at [Swift Mailer](http://swiftmailer.org/).

There are special solutions for [WordPress](install-wordpress#toc-smtp), [Laravel](install-laravel#toc-smtp) & [Symfony](install-symfony#toc-smtp).


## Logs

Currently only [live logs](logging) are available.


## Ephemeral storage

Each App has a [limited amount](https://www.fortrabbit.com/specs#limits) of local non-persistent, floating — so called ephemeral storage. It's a bit of a unique concept and might look very strange at first, but it's actually a corner stone of cloud enabled applications. From the [12-factor Apps](http://12factor.net/) manifesto:

> The filesystem … can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database. The twelve-factor app never assumes that anything cached … on disk will be available on a future request or job … a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.


So you better not store any runtime data, like user uploads there. Here is what might can happen: You install [WordPress](install-wordpress), you upload images via the wp-admin. Everything works fine, but then you deploy code again and everything you have uploaded is gone. 

During each [deployment](/deployment) the whole local storage gets whipped and replaced. See also our [deployment video](/deployment-architecture-video) to learn about what happens in the background.

But we have a good solution. With the [Object Storage Component](/object-storage) you can offshore your static assets. That is a very scalable solution that also brings other benefits and speed improvements. You can easily integrate it with a plugin or Composer package and some config, see your framework/CMS guides here as well.


## PHP

### PATH_INFO

Our runtime implements PHP via FastCGI + FPM. To utilize `PATH_INFO` you need to do a small hack-around in your `.htaccess` file:

```
RewriteCond %{REQUEST_URI} \.php/ [NC]
RewriteRule ^(.+)\.php/(.+)$    /$1.php [NC,L,QSA,E=PATH_INFO:/$2]
```

### Basic authentication

If you want to use [HTTP basic authentication](http://en.wikipedia.org/wiki/Basic_access_authentication), you need to forward the `Authorization` header via an `.htaccess` directive and [parse the contents manually](http-auth).

### Authorization header

If you need the `Authorization` header, for OAuth for example, you have to forward the header explicitly via an ENV variable:

```
RewriteCond %{HTTP:Authorization} .
RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
```

### pecl_http

The PHP extension `HTTP` from PECL is enabled by default. The classes `HttpRequest`, `HttpR`sponse` and `HttpMessage` are very handy replacement for curl and alike. The downside: it breaks CakePHP in some cases. You can disable the extension in your Settings -> Extensions tab of your App.

### Locales

Available locales are:

<pre><code class="plain" id="locales"></code></pre>

<script type="text/javascript">
    $(function() {
        console.log("Loading locales");
        $.get('/locales.php', function(res) {
            $('#locales').text(res.sort().join("\n"));
        })
    });
</script>

## Git

### Branch name matters

You can use / create as many [branches](git) as you want and push them to the fortrabbit remote repository. However there are only two branches which will be deployed: the `master` branch and a branch which has the same name as your App. If your App is named `my-app` then a branch named `my-app` will be prefered over the `master` branch.

This is a feature, not a bug: use other branches as "transport" branches to interchange code with other developers / locations without publishing it to your web space. Once your code is ready to deploy, just merge it in the master (or App-name) branch and push it.

### Release package limit

To keep deployment fast for everyone the size of the release package is limited to 200 MB. [Read on](git#toc-release-package-limit).

## MySQL

### Persistent connections vs upgrade

If you scale (up or down) your MySQL, your App's database will be migrated to a new node. Therefore the CNAME target of your Apps MySQL hostname will be changed. The hostname's time to live (TTL) is 60 seconds, which reflects the maximum expected downtime during upgrade.

With persistent connections this can take longer (possibly up to half an hour). Thefore we recommend to disable persistent connections during upgrades and downgrades.



## Firewalling

Outgoing traffic is limited for [security](security) reasons — most ports for making outgoing calls are locked. Only the most [standard ports are white-listed](http://www.fortrabbit.com/specs#firewall). Any access to service Components provided by us directly (Memcached, MySQL, ..) is of course also allowed. Any other traffic is denied - but you can request to open ports for your App. To do so: login to the Dashboard, browse to your App, go to the firewall settings, "request a custom white listing".


## Outgoing IP

Apps don't have a fixed IP address. This is a side effect of "the cloud", as Apps need to be capable of moving fast from one Node to another, due to failover, scaling or load balancing.

As we are currently only in the AWS EU1 (Ireland) region, there is an semi-official [list of ip ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html), which we are using. Depending on the use-case, it is possible to use a [HTTP](https://www.quotaguard.com/pricing#_quotaguardstatic) [proxy](http://www.vpnuk.info/dedicated-ip.html) provider, which offers a static IP.

The context for requests on this is for payment processing, have a look at an external provider.

## What this isn't

Embrace the idea of decoupled services! We don't offer some classical hosting providers standards such as:

* Plesk, cPanel, ISPconfig, any classical hosting control panel
* eMail hosting, E-Mail address configuration
* any WebMail, so no RoundCube or SquirrelMail
* Domain ordering, top level domain registration services
* one-click installers

Our advice to you, the professional developer: consider to abandon your all-in-one old school hosting.