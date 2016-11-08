---

template:      article
reviewed:      2016-02-15
dontList:      false
title:         Using TLS custom
naviTitle:     TLS custom
lead:          "How to setup HTTPS with your own certificate."
group:         Components
stack:         pro
oldLink:       ssl-old

keywords:
    - ssl
    - domain
    - subdomain
    - certificate authority
    - CA
    - SNI

---

<!-- 

TODO: define is this Hobby + Pro stack? write articles. Current status: custom TLS is available for the highest plan of Universal Apps, but we don't hard code this here, we link to the Specs.

So TLS custom is a Component for Professional Apps and a Setting for Universal Apps.

So: either this is an article for all (remove -uni title and front-matter stack to all) or two different articles.

-->

We assume that your are familiar with the basic concepts of TLS and the [general TLS implementation on fortrabbit](/tls). In this article you can learn why and how to use "TLS custom" instead of it's free counterpart.

## About TLS custom

Instead of making use of Let's Encrypt in the free version, you can bring your own certificate with "TLS custom". TLS custom costs a little monthly fee, current costs can be found in our [specs table](https://www.fortrabbit.com/specs#tls).

## Why use TLS custom

1. You want to use a [higher trusted](https://en.wikipedia.org/wiki/Extended_Validation_Certificate) commercial certificate
2. You need a wildcard certificate, including all subdomains `*.yourdomain`
3. You don't trust freebies

## Why not to use TLS custom

* You don't want to spend money on a certificate
* You don't want to spend time on creating and obtaining a certificate
* Your application is not business critical


## How use TLS custom

The roadmap to setup TLS custom reads like so:

1. have an external [domain](about-domains) registered with your fortrabbit App
2. create a key and a certificate request locally
3. using the certificate request: purchase an TLS/SSL cert from a third party CA
4. book the TLS component for your App in the Dashboard
5. upload your key and cert(s) to the Dashboard
5. route all your (sub)domains to your App


### Route domains

With "TLS custom" you just install the certificate for your qualified domain name (from the cert). Take to also have that [domain](about-domains) registered and routed to your App.


### Create a new key and certificate

To obtain a certificate, you first must create a certificate signing request (CSR). With the newly generated CSR, you can go to an external certificate vendor, which will issue a certificate for you.

#### From Mac/Linux

Using `openssl` from the the command line:

```bash
openssl req -new -nodes -keyout {{app-name}}.key -out {{app-name}}.csr -newkey rsa:2048
# Generating a 2048 bit RSA private key
# ..........................................................................................++
# .............................................+++
# writing new private key to '{{app-name}}.key'
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
openssl rsa -in {{app-name}}.key -out {{app-name}}.rsa.key
```

Do not enter a password! Also: if you plan on using `www.yourdomain.tld`, don't miss the `www.` in the "Common Name"!

#### From all OS

Download the open source software [XCA](https://sourceforge.net/projects/xca/) and create your key and certificate request here. Again: Mind **not** to give the key a password.


### Convert an existing key to RSA format

If you already have a private key and it begins with `----BEGIN PRIVATE KEY----` you need to convert it to the RSA format, using:

```bash
openssl rsa -in {{app-name}}.key -out {{app-name}}.rsa.key
# writing RSA key
```

The RSA format is required by our TLS implementation.

### Finding the right Certificate Authority provider

Most Certificate Authority provider look slightly dusty, but they do the job (We have business relations here):

* [StartSSL](https://www.startssl.com/) provides also free certificates
* [Comodo](https://www.comodo.com/)
* [DigiCert](https://www.digicert.com/)
* [GoDaddy](https://www.godaddy.com/) does CA and domain registrations
* [Thawte](https://www.thawte.com/)
* [GeoTrust](https://www.geotrust.com/)
* [SSLs](https://www.ssls.com/) sells certificates from multiple CAs

Book your cert there. You will be guided.

### Book & install with fortrabbit

Now you are ready to book the "TLS custom" Component in the fortrabbit Dashboard for your App. During the booking in the fortrabbit Dashboard you will be asked to insert your certificate, your private key and any intermediate (chain) certificates you got. You should have gotten all of those from your CA vendor already.


## Renewing certificates

Remember that certificates are usually only valid for one year. So you need to re-purchase a new certificate from your provider and re-install it with your App. The steps are the same as described above.



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
