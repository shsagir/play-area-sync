---

template:         article
naviTitle:        CloudFlare
reviewed:         2018-01-30
title:            Using CloudFlare with fortrabbit
group:            Domains_and_DNS
section:          Extending_fortrabbit
stack:            all

websiteLink:      https://www.cloudflare.com
websiteLinkText:  cloudflare.com
category:         CDN
dataCenters:      n/a
image:            cloudflare-mark.png

keywords:
     - advanced
     - CDN
     - ddos
     - ssl

---


## About CloudFlare

CloudFlare is a "magical" multi-purpose website performance & security service, which works on DNS level. It can be used as a kind of Content Delivery Network and as a protection against Denial Of Service Attacks. It is also popular choice to get an SSL certificate for your domain without costs and hustle.


## Pricing

It starts with a free plan with the basic features to get you started. [cloudflare.com/plans/](https://www.cloudflare.com/plans?utm_source=fortrabbit).


## Signing Up

Go to the [sign up page](https://www.cloudflare.com/a/sign-up) and enter email and choose a password.


## Integrating CloudFlare with fortrabbit

There is no technical connection between CloudFlare and fortrabbit. CloudFlare will be in between of your DNS provider and fortrabbit. 

### Setup CloudFlare in addition to a working connection

Here is how to combine CloudFlare with your domain provider and fortrabbit. In the following example you first register your domain with your domain provider and point it to fortrabbit using our standard settings. Then CloudFlare will 

1. Have your domain registered with a domain provider.
2. Route the domain using our instructions (CNAME for www + IP for naked).
3. Set up the domain with CloudFlare. There you'll be asked you to set their name servers. Set the CloudFlare NS with your domain provider.
4. Done

Many fortrabbit clients have used CloudFlare to get SSL (https) for their own custom domain without the need to book and setup the [HTTPS custom](/https-custom-pro) Component. Mind that fortrabbit (now) also offers free SSL certificates via Let's Encrypt on it's own.

Still, CloudFlare offers enhanced dDos protection and CDN functionality. And it also offers CNAME Flattening for naked domains. This way you can use your non-www-domain directly with fortrabbit.

## Prevent direct access

You may want to assure all traffic is routed through CloudFlare. To block direct access and whitelist only CloudFlare's IPs, extend your `.htaccess` file with the following rules:

```
ErrorDocument 403 "Not Allowed"
Deny from all

### Cloudflare IPs ###

Allow from 103.21.244.0/22
Allow from 103.22.200.0/22
Allow from 103.31.4.0/22
Allow from 104.16.0.0/12
Allow from 108.162.192.0/18
Allow from 131.0.72.0/22
Allow from 141.101.64.0/18
Allow from 162.158.0.0/15
Allow from 172.64.0.0/13
Allow from 173.245.48.0/20
Allow from 188.114.96.0/20
Allow from 190.93.240.0/20
Allow from 197.234.240.0/22
Allow from 198.41.128.0/17
Allow from 2400:cb00::/32
Allow from 2405:8100::/32
Allow from 2405:b500::/32
Allow from 2606:4700::/32
Allow from 2803:f800::/32
Allow from 2c0f:f248::/32
Allow from 2a06:98c0::/29
```

As the IP ranges might change from time to time, you should update them on a regular basis. [Here](https://www.cloudflare.com/ips/) you can find the current IP ranges. [This little script](https://gist.github.com/ostark/e9c577cd63a573b7417828d99da540e1) helps you to keep your whitelist up-to-date, ideally you run it [during deployment](/deployment-file-v2).   


