---

template:      article
reviewed:      2016-09-15
dontList:      false
title:         Using TLS with fortrabbit
naviTitle:     TLS
lead:          "How to make use of HTTPS on fortrabbit. Learn about the three options."
group:         platform
stack:         all

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

**Problem**: The interwebs is full of criminals trying to read your communication.
**Solution**: Serve requests over `HTTPS` not just `HTTP`.


## About HTTPS, TLS & SSL

`HTTPS` is **H**yper **T**ext **T**ransfer **P**rotocol over Transport Layer **S**ecurity. It is used to secure the data transport between a client (browser) and a server. **TLS** is the successor to the (still better known) **SSL** (Secure Sockets Layer). Current browsers show a green lock in the address bar if the connection is over HTTPS, the certificate could be verified and if the certificate is still valid.


## TLS options on fortrabbit

As part of [security efforts](/security) our aim is to give you secure, easy-to-use and affordable options:


### Piggyback TLS

You can already access your [App URL](app#toc-app-url) using HTTPS which is provided by us like so. For example: `https://{{app-name}}.frb.io`. Use this URL for testing or if a custom domain is not important to you.

### TLS free

This is the default free and zero-config option which makes use of the external [Let's Encrypt](https://letsencrypt.org/) service. There is nothing to setup or configure. Certificates will be created installed and renewed automatically for each [custom domain](/about-domains) you are adding to fortrabbit.

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

This will make your browser remember to always use the secured version of your App. It makes use of the "[HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)" policy and improves security by eliminating the risks of man-in-the-middle TLS-protocol-downgrade attacks. Be careful: setting this header will tell the browser never (or that is: until max-age) to use `http://` again. So if you later on decide to serve (parts of) your site using no encryption, all those clients (browsers) which saw the header will not comply and keep using `https://`.

### It's not working with Internet Explorer 8 or older

Sorry, IE8 is not supported any more. All our TLS implementations are based on SNI.
