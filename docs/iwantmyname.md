---

template:         article
naviTitle:        iwantmyname
reviewed:         2016-02-23
title:            Using iwantmyname with fortrabbit

group:            Extending_fortrabbit

websiteLink:      https://iwantmyname.com/
websiteLinkText:  iwantmyname.com
category:         queues
dataCenters:      n/a
image:            iwantmyname-logo.png

seeAlsoLinks:
     - domains

keywords:
     - domain
     - beginner
     - DNS
     - domain hosting
     - domain registration
     - domain registrar
     - TLD
     - gTLD

---


<!--

TODO: 

* review whole article 
* outsource general BLA BLA to category article

-->

## About domains on fortrabbit

If you found this article then you must know: fortrabbit does not offer domain registration. However, most clients run their Apps on custom top level domains. And those need to be registered somewhere.  Back in the days hosting providers sold packages including domain, webspace and email. We believe in decoupled services. So a specialized domain registration service without extra gimmicks fits perfectly in the picture.

### E-mail hosting

Before jumping right in: think about your personal e-mail addresses as well. Will you need e-mail accounts for that domain? With a dedicated domain registration service you need to host your e-mail addresses elsewhere. There are providers out there for this as well.


## About iwantmyname

iwantmyname aims to make domain management painless. The business is located in New Zealand, the founders have a German background. The service is simple to use yet powerful and developer-friendly.


## Pricing

Prices vary for different TLDs. In general, they are not the cheapest, but they offer a huge range of different TLDs. And, as mentioned before, they are painless to use. Something really worth mentioning because most DNS providers are not fun to work with. Billing is — as usual in domains — yearly in advance. 


## Signing Up

Visit their [website](https://iwantmyname.com/), fill out the web address field and click on the scope symbol. This will show you all available variations of the domain you just entered. Once you decided which you want follow the basket to the checkout, provide your address and payment info. Done.


## Connecting

Each fortrabbit App comes with an App URL: `your-chosen-name.frb.io`. This is not only your first address to see and test your App, it is also the hostname to which you can point CNAME records.

Within the inwantmyname dashboard you find the DNS settings of your domain `funkybiz.tld` and add a new record like so:

```plain
HOSTNAME      TYPE       VALUE
-------------------------------------------------
www           CNAME      your-chosen-name.frb.io.
```

**Note:** Mind the `.` at the end!

Now head over to the fortrabbit Dashboard > Your App > Settings > Domains, add `www.funkybiz.tld` and `funkybiz.tld` (if you want both) and save.

After the DNS changes are populated everywhere (this can take a while), you can reach your fortrabbit App under `www.funkybiz.io` as well. Of course you can choose any other subdomain — `blog.funkybiz.io` for example.


### Naked domain forwarding

To forward use your naked domain as well you forward it to the `www.`-prefixed-version. Use the "Web forwarding" tool from iwantmyname for this. You find it under setup.


## Further readings

Check out our [general article about DNS & domains](/domains) to dive into the strange world of the Domain Name System. 