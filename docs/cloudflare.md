---

template:         article
naviTitle:        CloudFlare
reviewed:         2018-05-08
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

### Recommended setup

In the following example you first register your domain with your domain provider, then point it to fortrabbit using our standard settings, then CloudFlare will scan the DNS records for you:

1. Make sure to have a domain registered with a domain provider
2. Make sure to have added the domain in the fortrabbit Dashboard
3. Route the domain using our instructions (CNAME for www + IP for naked) still with your old DNS provider
4. Set up the domain with CloudFlare. There you'll be asked you to set their name servers. Set the CloudFlare NS with your domain provider
4. CloudFlare will scan and use the DNS settings already in place
5. DONE

You might also start with a fresh domain from scratch within CloudFlare. Then CloudFlare will act as your DNS provider. Enter the DNS settings from the fortrabbit Dashboard.

### CloudFlare VS fortrabbit Dashboard

CloudFlare is a bit of blackbox, DNS-wise. When you'll visit a domain in the fortrabbit Dashboard that is routed via CloudFlare a definite answer is not possible. The Dashboard is aware of many CloudFlare IPs, so it will likely guess that the domain is routed via CloudFlare. Still you might see an error, while in fact the domain is routed correctly.

### ClouFlare SSL VS Let's Encrypt

Many fortrabbit clients have used CloudFlare to get SSL (https) for their own custom domain without the need to book and setup a custom cert here. Now, fortrabbit also offers free SSL certificates via (free and zero-config) Let's Encrypt. So if that is your aim, you'll might not need CloudFlare.

### Other reasons to use CloudFlare

While free SSL has become a commodity, there are other reasons to use CloudFlare:

* Enhanced DDoS protection
* CNAME Flattening for naked domains (naked domain directly with fortrabbit)
* Domain analytics
* …

## Prevent direct access

You may want to assure all traffic is routed through CloudFlare. This is important to secure 360 DDoS protection. You'll basically disable the App URL to be accessible. To block direct access and whitelist only CloudFlare's IPs, extend your `.htaccess` file with the following rules:

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


## Alternatives to CloudFlare

CloudFlare is a magical package of different services all combined and streamlined in a single offering. Mind: with using CloudFlare, you make yourself dependent that this critical component (DNS/NS)) of your infrastructure is working correctly. It's another SPOF that makes your setup more complex and that can fail. In overall, it's likely more likely that your website will stay up with CloudFlare enabled.

Some of our clients prefer separation of services, instead of CloudFlare, they'll might use dedicated services for DNS/domain and CDN.

