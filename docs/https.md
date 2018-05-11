---
template:      article
reviewed:      2015-05-09
dontList:      false
title:         Using HTTPS with fortrabbit
naviTitle:     HTTPS on fortrabbit
excerpt:       "How to make use of HTTPS on fortrabbit. Learn about the options."
group:         platform
stack:         all

keywords:
    - ssl
    - TLS
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


## HTTPS options on fortrabbit

As part of [security efforts](/security) our aim is to give you secure, easy-to-use and affordable options:



### Piggyback HTTPS

PSSST. This is a bit of a hidden feature: You can already access your [App URL](app#toc-app-url) using HTTPS. For example: `https://{{app-name}}.frb.io`. Use this URL for testing or if a custom domain is not important to you.

### Automatic HTTPS

[Let's Encrypt](https://en.wikipedia.org/wiki/Let's_Encrypt) is a free certificate authority and can be used to generate free certificates for custom domains. The Automatic HTTPS feature is included for all fortrabbit Apps: All domains correctly added and routed will receive an SSL certificate by Let's Encrypt, no configuration or extra setup required. The certs will also be renewed automatically. This option does not work for wildcard domains (yet).


### Custom HTTPS

Bring your own certificate: There are various use-cases to mandate a custom certificate, for example if you plan on using wildcard domains or need a "green browser bar". Whichever reason it may be: we support installation of custom certificates for many App plans in the Dashboard > {{app-name}} > TLS certificate.



## Availability

Which of the options can be used depends on the [App stack](stacks) and the specific plans which are booked for the App:

| **HTTPS/TLS Option** | Professional App                         | Universal App   |
| -------------------- | ---------------------------------------- | --------------- |
| Piggyback HTTPS      | Yes                                      | Yes             |
| Automatic HTTPS      | Yes                                      | Yes             |
| Custom HTTPS         | Yes, as extra Component, [see specs](//www.fortrabbit.com/specs-pro#https) | Depends on plan |


## Using TLS/HTTPS

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

## Troubleshooting TLS

Sometimes it just doesn't work as supposed to. Don't panic. You can of course always ask us for support. Here are some things you can do on your own:

### You see a certificate warning 

You visit your Apps domain under the `https://` address and the browser throws an error that the certificate can't be verified. This happens, when the domain is brand new and the cert is not YET installed (can take up to 24 hours). In this case, please wait a little. This can also happen, when your domain is not routed to fortrabbit (YET), only domains that are already routed to fortrabbit will receive a Let's Encrypt cert. Please the domain settings in the Dashboard.

### Cert is installed but browser bar is not green

In most cases this is due to "**mixed content**", which means, the cert is installed and everything is working, but your website is requesting external resources over non-secure addresses (http). Check the source code of your website and find and replace all `http:` with `https://` requests.

