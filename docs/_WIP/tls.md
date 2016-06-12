---

template:      article
reviewed:      2016-02-13
dontList:      true
title:         Using TLS with fortrabbit
naviTitle:     TLS
lead:          "How to make use of HTTPS on fortrabbit. Learn about the three options."
group:         Components

otherVersionLinks:
    - ssl-old-app

keywords:
    - ssl
    - domain
    - subdomain
    - CA
    - Certificate Authority
    - port 443
    - port 80

seeAlsoLinks:
    - tls-custom
    - about-domains

tags:
    - DNS

---

## Problem

The interwebs is full of criminals trying to read your communication.


## Solution

All requests should be routed over `HTTPS` not only `HTTP`.


### About HTTPS, TLS & SSL

`HTTPS` is **H**yper **T**ext **T**ransfer **P**rotocol over Transport Layer **S**ecurity. It can used between browser and server. **TLS** is the successor to the (still better known) **SSL** (Secure Sockets Layer). Current browsers show a green lock in the address bar when the connection is over https and the certificate could be verified.


## TLS options on fortrabbit

As part of [security efforts](/security) our aim is to give you secure, easy-to-use and affordable options:


### Piggyback TLS

You can already reach your [App URL](app#toc-app-url) under HTTPS provided by us like so: `https://your-app.frb.io`. Use this for testing and when a domain is not important.

### TLS free

This is the default free and zero-config option which makes use of the external [Let's Encrypt](https://letsencrypt.org/) service. There is nothing to setup or configure. SSL certificates will be created installed and renewed automatically for each [custom domain](/about-domains) you are adding to fortrabbit.

* [TLS free introduction blog post](https://blog.fortrabbit.com/tls-free-launched)

### TLS custom

Bring your own certificate for even more security and advanced options for your custom domains. See the [TLS custom help article](/tls-custom) for why and how.



## General usage

The following applies to all three TLS options on fortrabbit:


### Redirect all requests to HTTPS

Create or modify an `.htaccess` file in your root path folder like so:

```plain
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Port} !=443
RewriteRule (.*) https://%{HTTP_HOST}/$1 [R=301,L]
```

The reverse (redirect all traffic to HTTP) would be:

```plain
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Port} !=80
RewriteRule (.*) http://%{HTTP_HOST}/$1 [R=301,L]
```

### Force HTTPS for future visits with HSTS

In your `.htaccess` file you can add this line (in addition to the rewrite rule above):

```plain
Header always set Strict-Transport-Security "max-age=31536000"
```

This will make your browser remember to always use the secured version of your App. It makes use of the "HTTP Strict Transport Security" policy and improves security by eliminating the risks of man-in-the-middle TLS-protocol-downgrade attacks. Be careful when using it, it's cached in your browser.

### It's not working with Internet Explorer 8

Sorry, IE8 is not supported any more. All our TLS implementations are based on SNI.