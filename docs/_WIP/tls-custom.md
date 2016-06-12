---

template:      article
reviewed:      2016-02-13
dontList:      true
title:         Using the TLS custom
naviTitle:     TLS custom
lead:          "How to setup HTTPS with your own certificate."
group:         Components

otherVersionLinks:
    - ssl-old-app

keywords:
    - ssl
    - domain
    - subdomain
    - certificate authority
    - CA
    - SNI

seeAlsoLinks:
    - tls
    - about-domains

tags:
    - DNS

---

We assume that your are familiar with the basic concepts of TLS and the [general TLS implementation on fortrabbit](/tls). In this article you can learn why and how to setup "TLS custom". 

## About TLS custom

Instead of making use of Let's Encrypt in the free version, you can bring your own certificate with "TLS custom". TLS custom costs a little monthly fee, current costs can be found in our [specs table](https://www.fortrabbit.com/specs#tls). For TLS custom there is an extra load balancer hosting your SSL/TLS certificate and that's also why we have to charge for it.


## Why to use TLS free (not this)

* You don't want to pay extra
* You fear the complicated setup
* Your application is not business critical

## Why to use TLS custom (this)

* You want to use a more secure commercial certificate 
* You need a wildcard certificate
* You want an internationalized domain name (punycode)
* You don't trust Let's Encrypt that much
* You don't fear the complicated setup


## Usage

The roadmap to setup TLS custom reads like so:

1. have an external [domain](about-domains) registered with your fortrabbit App
2. create a key and a cert locally
3. purchase an TLS/SSL cert from an external CA provider
4. book the TLS component for your App in the Dashboard
5. upload your key and cert(s) to the Dashboard
5. route all your (sub)domains to your App's hostname via CNAME


### Route domains

With "TLS custom" you just install the certificate for your qualified domain name (from the cert). Take to also have that [domain](about-domains) registered and routed to your App.


### Create a new key and certificate request

Your external SSL/TLS Certificate Authority provider will ask you for those. To create a new key and a certificate signing request (CSR), issue the following commands on the console of your local machine (Mac/Linux):

```bash
openssl req -new -nodes -keyout my-app.key -out my-app.csr -newkey rsa:2048
# Generating a 2048 bit RSA private key
# ..........................................................................................++
# .............................................+++
# writing new private key to 'my-app.key'
# -----
# You are about to be asked to enter information that will be incorporated
# into your certificate request.
# What you are about to enter is what is called a Distinguished Name or a DN.
# There are quite a few fields but you can leave some blank
# For some fields there will be a default value,
# If you enter '.', the field will be left blank.
# -----
# Country Name (2 letter code) [AU]:DE
# State or Province Name (full name) [Some-State]:Berlin
# Locality Name (eg, city) []:Berlin
# Organization Name (eg, company) [Internet Widgits Pty Ltd]:fortrabbit
# Organizational Unit Name (eg, section) []:Website
# Common Name (e.g. server FQDN or YOUR name) []:www.fortrabbit.com
# Email Address []:info@fortrabbit.com
# -----
# Please enter the following 'extra' attributes
# to be sent with your certificate request
# A challenge password []:
# An optional company name []

# Update key format
openssl rsa -in my-app.key -out my-app.rsa.key
```

Do not enter a password! Also: if you plan on using `www.yourdomain.tld`, don't miss the `www.` in the "Common Name"!

With the now generated CSR, you can go to an external certificate vendor, which will issue a certificate for you.


### Convert an existing key to RSA format

If you already have a private key and it begins with `----BEGIN PRIVATE KEY----` you need to convert it to the RSA format, using:

```bash
openssl rsa -in my-app.key -out my-app.rsa.key
# writing RSA key
```

The RSA format is required by our TLS implementation.

### Finding the right Certificate Authority provider

Most Certificate Authority provider look slightly dusty, but they do the job (We have business relations here):

* StartSSL from Israel is known to have a free cert
* Comodo
* DigiCert
* GoDaddy does CA and domains
* Thawte
* GeoTrust

Book your cert there. You will be guided.

### Book & install with fortrabbit

Now you are ready to book the "TLS custom" Component in the fortrabbit Dashboard for your App. During the booking you will be asked to insert your certificate, your private key and possibly your intermediate (chain) certificates. You should have got all those from your Certificate Authority vendor already.


## Renewing certificates

Remember that certificates are usually only valid for one year. So you need to re-purchase new ones from your provider and re-install them with your App. The steps are the same as described above.



## Common issues

It's not you. It's the whole surprisingly confusing process here. Unsure about certain parts? Have questions? Hanging somewhere? Don't be afraid to ask. We are experienced and we can help you get this done.

### Solve SSL verification errors

This is the fix for verification errors when using ssl_verify. If you are receiving an error like the following:

```
error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed
```
You need to set the `capath` as well. Assuming you are using PHP streams, it would look like this:
```php
$context = stream_context_create();
stream_context_set_option($context, 'ssl', 'verify_peer', true);
stream_context_set_option($context, 'ssl', 'capath', '/etc/ssl/certs'); # <<< that's the one
stream_context_set_option($context, 'ssl', 'allow_self_signed', false);

$fp = stream_socket_client("thedomain.tld:443", $errno, $errstr, 5, STREAM_CLIENT_CONNECT, $context);
# ..
if (stream_socket_enable_crypto($fp, true, STREAM_CRYPTO_METHOD_SSLv3_CLIENT) === false) {
    die("Failed to verify certificate");
}
#...
```

For cURL, the option CURLOPT_CAPATH needs to be set to `/etc/ssl/certs`.