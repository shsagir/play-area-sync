---

template:      article
reviewed:      2016-05-16
title:         All about Domains & DNS
lead:          How to configure and route any domains to your fortrabbit App.
naviTitle:     Domains
group:         Kitchen_sink

otherVersionLinks:
    - domains-old-app

tags:
    - beginner

keywords:
    - TLD
    - Top Level Domain
    - registration
    - ordering
    - TLS

seeAlsoLinks:
    - app
    - directory-structure
    - tls
    - iwantmyname

---

Each fortrabbit [App](/app) has its own, unique [App URL](/app#toc-app-url). Additionally you can route any external top-level-domain to your App. Your goal is to have your App running under your own domain. This is the high level of what's gonna happen:

<!-- TODO: rewrite on launch of www-izer domain handling  -->

1. register `www.mydomain.com` with the App in the fortrabbit Dashboard
2. route `www.mydomain.com` via CNAME to `your-app.frb.io` using your domain provider
3. forward all requests for the naked domain `mydomain.com` to `www.mydomain.com` with your domain provider or even a third party service

And here are the details:


## Choosing a domain provider

fortrabbit is not offering direct domain registration and management. In classical hosting domain registration & e-mail hosting locked-in the user. Today you can find specialized domain services. There are various offerings, so you should consider upfront what you'll need:

* Additional IMAP/POP3 e-mail hosting
* Additional SSL certificate ordering
* Advanced DNS configurations
* Exotic TLD-endings
* Support for forwards with ALIAS or ANAME records — see [below](#toc-forwarding-a-naked-domain) 




## Route a custom domain

Many different and advanced configurations are possible, see below. Here is the dead simple setup:

#### 1. Domain provider

Point `www.yourproject.com` using `CNAME` to your fortrabbit App URL `my-app.frb.io` (use your own App URL here).

```plain
HOSTNAME      TYPE       VALUE
-------------------------------------------------
www           CNAME      your-chosen-name.frb.io.
```



#### 2. fortrabbit Dashboard

Enter the domain `www.yourproject.com` as a new external custom domain in the domain settings of the App in the [Dashboard](/dashboard). 

Additionally you might set a [custom root path](/app#toc-set-a-custom-root-path) and [change the default domain](/app#toc-change-the-default-domain) or book the [TLS Component](/tls).


## Advanced routing alternatives

The world of DNS is one of its own. Let's dive into it. Understand the backgrounds, explore advanced and alternative settings.


### www and other non-naked subdomains

Back in the days the www. prefix indicated that this is an address to type into the browser. Nowadays the www. prefix indicates a "cloud-enabled" application which can be moved in seconds to another server location. Test it: all big players run on a www. subdomain. The name of the subdomain prefix is not so important, but `www` is the convention for (marketing) entry points. We use `help.fortrabbit.com` for the page you are currently reading and `blog.fortrabbit.com` to publish our thinkings.

The trick is that you can route subdomains using `CNAME` records. With this you tell your DNS provider to send resolve to a `hostname` instead of an `IP`. The great advantage is that the IP address behind the hostname target can change later on — without your intervention.

#### CNAME for naked domains

It is theoretically possible to create a CNAME record for a naked domain. **Just don't do it.** It's not allowed because [the DNS specification says so](http://www.ietf.org/rfc/rfc1035.txt).

The problem is that `CNAME` records do not behave like the other records (`A`, `MX`, `TXT`, …). They are *greedy*. This means: They *overwrite* all other records.

An example: assume you have a domain `domain.tld` and want to receive mails for it. So you create an `MX` record. Say it points to `mail.domain.tld`. If you now would create a `CNAME` record directly for `domain.tld` pointing to `otherdomain.tld`, every lookup of any record for `domain.tld` will be made on `otherdomain.tld` instead (aside from the `CNAME` record itself).


#### Forwarding a naked domain

Ok, let's say that you have your primary domain routed via CNAME to the www domain. Now what should happen when users enter the address without www prefix in the browser? It should redirect to the www version automatically. And here is how to that:

In **fortrabbit Dashboard** you should add the naked domain `yourproject.com` as well as primary domain `www.yourproject.com`.

At your **external domain provider**: forward naked domain `yourproject.com` to subdomain `www.yourproject.com` using ALIAS or ANAME method.

Some domain providers also support a simple HTTP redirect. This basically means: they provide a web server for you and redirect all incoming requests to `http://domain.tld/` to `http://www.domain.tld/`. Alternative names for this feature are "Domain forwarding", "Web forwarding", "Domain masking" and the like.

**Domain forwarding docs of some domain providers**

* [GoDaddy domain forwarding documentation](https://support.godaddy.com/help/article/422/manually-forwarding-or-masking-your-domain-name)
* [Gandi domain forwarding documentation](https://wiki.gandi.net/en/domains/management/domain-as-website/forwarding)
* [1&1 domain forwarding documentation](http://help.1and1.com/domains-c36931/manage-domains-c79822/domain-destination-c38672redirectforward-your-domain-a594868.html)

When your external domain service does not support such forwarding methods, you can make use of a free service like [RootDirect](https://www.rootredirect.com/) or [wwwizer](http://wwwizer.com/) for naked domain redirects. It's pretty simple, as they do not even require any registration or setup, you only need to point your naked domain to their IP and they will redirect to your `www.` address. 



### Time delays

Most DNS records have a TTL (Time To Live) of 24 hours. This means that all name servers will only look every 24 hours if a domain target has been changed. In some cases it might even take longer. That is actually a good thing as it helps to reduce round trips. Some providers let you set down the TTL, this can be useful when moving domains.


### Further readings

* [iwantmyname: ALIAS-type DNS records for CNAME](https://iwantmyname.com/blog/2014/05/alias-type-dns-records-for-cname-functionality-on-naked-domains.html)
* [iwantmyname: ALIAS-type DNS records break the internet](https://iwantmyname.com/blog/2014/01/why-alias-type-records-break-the-internet.html)
* [www.yes-www.org](http://www.yes-www.org)
* [no-www.org](http://no-www.org)

## Troubleshooting domains

You can use the Terminal (Bash) to see the current DNS settings of your domain. With the `dig` command you can see if there are any CNAME entries and where they are pointing to. Here we lookup `help.fortrabbit.com` and see a CNAME pointing to the App URL: `help-frbit.frb.io`.

```
$ dig ANY help.fortrabbit.com
;; Truncated, retrying in TCP mode.

; <<>> DiG 9.8.3-P1 <<>> ANY help.fortrabbit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 44565
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;help.fortrabbit.com.       IN  ANY

;; ANSWER SECTION:
help.fortrabbit.com.      600    IN  CNAME   help-frbit.frb.io.
help-frbit.frb.io.        300    IN  CNAME   help-frbit.eu2.frbit.net.
help-frbit.eu2.frbit.net.  20    IN  A       52.48.51.144

;; Query time: 863 msec
;; SERVER: 192.168.178.1#53(192.168.178.1)
;; WHEN: Wed Jun  1 10:25:40 2016
;; MSG SIZE  rcvd: 122
```