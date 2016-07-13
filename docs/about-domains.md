---

template:      article
reviewed:      2016-07-11
title:         All about domains & DNS
lead:          How to configure and route domains to your fortrabbit App.
naviTitle:     Domains
group:         Kitchen_sink

otherVersionLinks:
    - domains-old-app

tags:
    - beginner

keywords:
    - TLD
    - Top Level Domain
    - top-level-domain
    - registration
    - ordering
    - zone apex
    - apex domain
    - root domain
    - naked domain
    - subdomain
    - domain masking
    - domain name server
    - DNS
    - ns
    - lookup

seeAlsoLinks:
    - tls
    - app
    - directory-structure
    - iwantmyname

---

Each fortrabbit [App](/app) has its own, unique [App URL](/app#toc-app-url). Additionally you can route any external domain to your App. Your goal here is to have your App running under your own domain.

First off, make sure that the App knows about the domain. Then point the domain to your fortrabbit App with your domain provider. Start the process in the fortrabbit Dashboard like so:

1. Click Your App > Domains > "Add a new domain"
2. Choose a domain name
3. Set the [root path](/app#toc-set-a-custom-root-path)
4. Use the provided informations to route the domain with your domain provider


## Advanced routing options

The world of DNS is one of its own. Let's dive into it – understand the backgrounds, explore advanced and alternative settings.


### www and other subdomains

Back in the days the `www.` prefix indicated that this is an address to type into the browser. Nowadays the `www.` prefix indicates a "cloud-enabled" application which can be moved in seconds to another server location. The name of the subdomain prefix is not so important, but `www` is the convention for (marketing) entry points. For example: We use `help.fortrabbit.com` for the page you are currently reading and `blog.fortrabbit.com` to publish our thinkings.

The trick is that you can route subdomains using `CNAME` records. By this you are telling your DNS provider to resolve a `hostname` instead of a fixed `IP` (which would be an `A` record). The great advantage is that the IP address behind the hostname target can change later on — without your intervention.

### Naked domains

There are so called "naked", "APEX" or "root" domains. They have no prefix and look like so: `fortrabbit.com`. Some think that they are aesthetically more pleasing than their subdomain counterparts. But they don't play well as with cloud services — like ours. Naked domains should not be routed using a `CNAME` record; they should be routed using an `A`-Record.

#### Don't dos

You could grab the IP of your App and use that as an `A`-record for your domain. It's technical possible, but than your App will be offline, once we move it to a another Node (which changes the IP address).

In some cases you could just use `CNAME` routing for your naked domain. It's theoretically possible, but not recommended by the [DNS specs](http://www.ietf.org/rfc/rfc1035.txt) and also would break any e-mail delivery for your domain.


### Forwarding a naked domain to www

You still should care about your naked domain, as some users might type it in directly in the browser address bar. So you want to forward all requests from your naked domain to your primary canonical subdomain:

When you register a `www.` domain in the fortrabbit Dashboard, we additionally provide a forwarding service for your naked domain. You'll get two routing values, the main `CNAME` target and an additional `A`-record that points to our forwarding service.

```plain
HOSTNAME      TYPE       VALUE
-------------------------------------------------
@             A          0.0.0.0 < See Dashboard for actual IP
www           CNAME      your-chosen-name.frb.io.
```

This will redirect all requests incoming to the naked domain to the `www.` domain.



### Alternative ways to use a naked domain

You don't have to use our free domain forwarding service, here are some alternative options:

#### ALIAS / ANAME routing

In the fortrabbit Dashboard you can add a naked domain. To make this work you need a domain provider that supports so called "ALIAS" or "ANAME" records. They allow you to have the functionality of CNAME routing (hostname target instead of IP). Following a list of domain / DNS providers who offer these special records:

* **[DNSimple: ALIAS records](https://support.dnsimple.com/articles/alias-record/)**
* [DNS made easy: ANAME records](http://help.dnsmadeeasy.com/managed-dns/records/aname-records/)
* [Easy DNS: ANAME records](https://fusion.easydns.com/index.php?/Knowledgebase/Article/View/190/7/aname-records/)


#### Forwarding, using your domain provider

Some domain providers also support a simple HTTP redirect. Please see your domain providers documentation. Here are some examples:

* [GoDaddy: domain forwarding](https://support.godaddy.com/help/article/422/manually-forwarding-or-masking-your-domain-name)
* [Gandi: domain forwarding](https://wiki.gandi.net/en/domains/management/domain-as-website/forwarding)
* [1&1: domain forwarding](http://help.1and1.com/domains-c36931/manage-domains-c79822/domain-destination-c38672redirectforward-your-domain-a594868.html)


### Using CloudFlare

Please see our [CloudFlare article](/cloudlfare) on how to setup and use CloudFlare together with fortrabbit.


### Wildcard domains

Do you want to route all requests for all possible subdomains to your fortrabbit App (`*.mydomain.com`)? That is possible, but for security reasons we'll need to verify that you own the domain.

## Changing the default domain

Per default your App URL is the "default domain". You can change it so that links from the Dashboard and the thumbnail preview generation will uses a custom domain of yours. We also use the default domain for various internal monitoring services.

To change the default domain: In the Dashboard go to your App > Domains. There click on the star icon beside the domain name in the list.

## Using HTTPS

Domains on fortrabbit can be accessed via `HTTP` and `HTTPS`. Please see the [TLS article](/tls) for informations on secured connections and SSL certificates.


- - -


## Troubleshooting DNS

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

Alternately you can use a browser based DNS lookup tool. See [these results](http://lmgtfy.com/?q=dns+lookup).

## Time delays

Many DNS providers default the TTL (Time To Live) of all records to 24 hours. This means that all changes will apply with a delay up to 24 hours, because everybody who has looked up the domain caches the result for the TTL duration. Also the TTL is not guaranteed: One badly programmed router on the way, who ignores the TTL or imposes it's own minimal TTL, can change the TTL for everybody "behind it". The caching itself is actually a good thing as it helps to reduce round trips. Some providers let you set down the TTL, which is very useful when moving or changing domains.


## Testing domain routing with your local hosts file

Let's say you are developing a new App and want to use your custom domain before actually routing it to fortrabbit. Just add the domain to fortrabbit, as you would do with any actually routed domain, then modify your local host file, which let's your local machine now ahead of time that the domain is to be served from your fortrabbit App.

### hosts file location

The hosts file is a text file (without file type ending). It can be found here:

* Windows: `c:\windows\system32\drivers\etc\hosts`
* MacOS & Linux: `/etc/hosts`

### How to get your Apps IP

See the above troubleshooting section to grab the current IP of your App. Mind that this IP will change in the future and that this IP is just for now.

### Editing your local hosts file

Your local file contains many entries, do not edit those. Just add a new line with the IP of your App and the domain you want to see routed there like so:

```
# pattern (how it works)
[your App's IP address] [your fully qualified domain name]

# example (how it looks like)
12.0.0.1 mydomain.com www.mydomain.com
```


### Undo changes to your hosts file

After your domain has been moved/propagated be sure to remove the entry from your hosts file.



## Choosing a domain provider

fortrabbit is not offering direct domain registration and management. In classical hosting, domain registration & e-mail hosting where bundled in packages. Today you can find specialized domain services. There are various offerings, so you should consider upfront what you'll need:

1. Additional IMAP/POP3 e-mail hosting
2. Additional SSL certificate ordering
3. Specialized in advanced DNS configurations
4. Exotic TLD-endings
5. Support for forwards with ALIAS or ANAME records — see [below](#toc-forwarding-a-naked-domain)

Many of our clients are using classical offerings combining domain ordering and e-mail hosting. Others are using separated services for domain registration and e-mail hosting.


### Configuring your domain for e-mail

So far we have covered how to route a domain to fortrabbit. To receive and send e-mails from your domain you will configure the MX record of your domain. Please see your e-mail hosting provider for instructions.
