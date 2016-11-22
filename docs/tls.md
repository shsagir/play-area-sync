---

template:      article
reviewed:      2016-09-15
dontList:      false
title:         Using TLS with fortrabbit
naviTitle:     TLS
lead:          "How to make use of HTTPS on fortrabbit. Learn about the three options."
group:         platform
stack:         all
oldLink:       ssl-old

keywords:
    - ssl
    - domain
    - subdomain
    - CA
    - Certificate Authority
    - port 443
    - port 80

---

**Problem**: The interwebs is full of criminals trying to read your communication.
**Solution**: Serve requests over `HTTPS` not just `HTTP`.


## About HTTPS, TLS & SSL

`HTTPS` is **H**yper **T**ext **T**ransfer **P**rotocol over Transport Layer **S**ecurity. It is used to secure the data transport between a client (browser) and a server. HTTPS is based on **TLS** (Transport Layer Security), which is the successor to the still better known **SSL** (Secure Sockets Layer). Current browsers show a green lock in the address bar if the connection is over HTTPS, the certificate could be verified and if the certificate is still valid.


## TLS options on fortrabbit

As part of [security efforts](/security) our aim is to give you secure, easy-to-use and affordable options:

### Piggyback TLS

You can already access your [App URL](app#toc-app-url) using HTTPS which is provided by us for free. For example: `https://{{app-name}}.frb.io`. Use this URL for testing or if a custom domain is not important to you.

### TLS via Let's Encrypt

[Let's encrypt](https://en.wikipedia.org/wiki/Let's_Encrypt) is a free certificate authority and can be used to generate free certificates for custom domains. This option does not work for wildcard domains. We offer automatic Let's Encrypt integration for many plans for free.

### Bring your own certificate

There are various use-cases to mandate a custom certificate, for example if you plan on using wildcard domains or need a "green browser bar". Whichever reason it may be: we support installation of custom certificates for many App plans in the Dashboard > {{app-name}} > TLS certificate.

## TLS availability

Which of the options can be used depends on the [App stack](stacks) and the specific plans which are booked for the App:

### Professional App

| **TLS Option**                 | Availability                                                                                   |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| Piggyback TLS                  | Automatic available for your App URL. No extra costs.                                          |
| Let's Encrypt                  | Automatic available for all custom domains (excluding wildcard domains). No extra costs.       |
| Bring your own certificate     | Available as TLS custom plan. See the [specs page for pricing](//www.fortrabbit.com/specs-pro) |


### Universal App

| **TLS Option**                 | Availability                                                                                   |
| ------------------------------ | ---------------------------------------------------------------------------------------------- |
| Piggyback TLS                  | Automatic available for your App URL. No extra costs.                                          |
| Let's Encrypt                  | Available for Apps using either Side Project or Work plan. No extra costs.                     |
| Bring your own certificate     | Available for Apps using Work plan. No extra costs.                                            |


## Using TLS

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
