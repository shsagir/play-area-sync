---

template:         article
naviTitle:        CloudFlare
reviewed:         2016-12-20
title:            Using CloudFlare with fortrabbit
group:            Domains_and_DNS
section:          Extending_fortrabbit
stack:            all

websiteLink:      https://www.cloudflare.com?utm_source=fortrabbit
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

Go to the [sign up page](https://www.cloudflare.com/a/sign-up?utm_source=fortrabbit) and enter email and choose a password.


## Integrating CloudFlare with fortrabbit

There is no technical connection between CloudFlare and fortrabbit. You basically configure CloudFlare with your [external domain](/domains).

Many fortrabbit clients are using CloudFlare to get SSL (https) for their own custom domain without the need to book and setup the [HTTPS custom](/https-custom-pro) Component. 

CloudFlare also offers [domain forwarding](/domains#toc-forwarding-a-naked-domain) from naked domains to the www-version.
