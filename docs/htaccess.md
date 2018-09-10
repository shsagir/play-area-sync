---

template:    article
reviewed:    2018-09-10
title:       Using .htaccess
lead:        Browsing the docs here you will find lot's of reference to a mysterious invisible file called ".htaccess". What's that about?
naviTitle:   .htaccess
group:       deployment
stack:       all

keywords:
    - HTTP Auth
    - Apache config
    - apache2.conf
    - php
    - ssi
    - gzip

---

## About .htaccess

`.htaccess` is a hidden file that usually lives in the web root folder of your code base. It enables altering the web server's (Apache) configuration directives. Therefore the syntax is a bit cryptic. `.htaccess` rules apply to all subdirectories. 

### .htaccess example

```htaccess
## Redirect all requests to www domain
RewriteEngine on
RewriteCond %{HTTP_HOST} ^facebook\.com [NC]
RewriteRule ^(.*)$ https://www.facebook.com/$1 [L,R=301,NC]
```

## Common .htaccess usage

You usually will not have to wrangle with `.htaccess`. Modern frameworks and CMS come with predefined ones, that are also managed. You will also find examples in context on these help pages here on fortrabbit. Following are common categories of usage: 

### Redirects

The most common use case for `.htaccess` is to re-write URLs with `mod_rewrite`. You can direct requests to a subdirectory, add the www subdomain to all requests, prettify URLs by omitting file endings, [force https](https#toc-redirect-all-requests-to-https) and much more.

#### Redirect all requests to the primary domain

Once you've added a [custom domain](/domains) you may want to prevent requests to your [App URL](/app#toc-app-url). The example below shows how to set up a redirect in your `.htaccess` file.

```htaccess
# From App URL to your domain
RewriteEngine On
RewriteCond %{HTTP_HOST} ^{{app-name}}.frb.io$ [NC]
RewriteRule ^(.*)$ https://www.your-domain.com/$1 [r=301,L]
```

#### Redirect all requests to https

<!-- TODO!  -->

### Authentication

You can implement a simple standard browser prompt password check for your website without any extra PHP magic with .htaccess. Hop over to our dedicated [HTTP Auth article](/http-auth) to learn on how.

### Custom error pages

You can define templates to make your error pages look more cool like so:

```htaccess
ErrorDocument 404 /404.html
```

### GZIP compression

You can set GZIP compression for static files like `*.html`, `*.js`, `*.css` and alike, so that they get first compressed, then send over and then decompressed by the users browser.

### Other usage

That's not all. `.htaccess` can do much more like: prevent hot-linking, have multiple domains in one root path, manage translated content, shorten URLs, capture variables …


### Secure access

You can use htaccess to ban user agents, referrers and script-kiddies from accessing your website.


<!--

Found multiple ways to do the next one, not sure which one is correct?!

#### Secure WordPress against pingback DDOS


```htaccess
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} Wordpress
RewriteRule . - [F,L]
```

or

```htaccess
<FilesMatch "^(xmlrpc\.php|wp-trackback\.php)">
  Order Deny,Allow
  Deny from all
</FilesMatch>
```

or

```htaccess
# protect xmlrpc
<Files xmlrpc.php>
  Order Deny,Allow
  Deny from all
</Files>
```

-->


## .htaccess on fortrabbit

These sensitive defaults are set on the fortrabbit platform regarding .htaccess:

* Apache configuration syntax 2.2 and 2.4 are supported 
* GZIP compression is enabled per default
* Access on all `.ht*` files is disabled, so nobody can read your .htaccess

<!-- Anything else? -->


## Tips and tricks

### Comment your .htaccess file

Apache directives with regular expressions tend to look glibberish. Make sure to comment what you have been doing there so that "the later you" can understand what's going on.


## Troubleshooting

Here are the most common mistakes we see in combination with `.htaccess`:

### Missing .htaccess

Another common mistake we see quite often is to miss out the `.htaccess` file. The dot at the beginning of the file name indicates that this file should be invisible in your operating system. So when you dragged files in your FTP client from the Downloads folder, it's very likely that the `.htaccess` file was left home. This can cause 403 errors.

In macOS Finder you can toggle invisible files with ` CMD + SHIFT + .`, easy to remember, like dot files. SFTP clients with a split view option are mostly showing those files by default. Take care, there are possibly other hidden files in your project, like `.gitignore` and `.env`.


### Wrong location

The `.htaccess` file is usually placed in the [root path](/app#toc-root-path) of the website. Per default, that is the `htdocs` folder. But certain modern frameworks and CMS have path below that, like Laravel with `public`. So make sure to place your `.htaccess` in the correct directory. You can place the `.htaccess` files in folders below the root path as well, those will have a higher specificity and can override rules. 


## Possible performance issues

The fortrabbit hosting platform is managed. Therefore your access to critical services is limited. This is actually good for most parts. We believe in a clear separation of concerns: We run the servers, you do the coding. One consequence is that we can not give you direct access on the Apache web server configuration (httpd.conf).

The `.htaccess` file is a very common method to achieve common web server configurations. In fact, almost every framework and CMS comes with a `.htaccess` out of the box. `.htaccess` lives in your code base and is deployed along with Git and therefore it's just very convenient to use.  

Consider it a workaround that got too popular. The official Apache docs have a whole section on "[When NOT to use .htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html#when)". The NGNIX team has setup a marketing page [bad-mouthing .htaccess](https://www.nginx.com/resources/wiki/start/topics/examples/likeapache-htaccess/) for a bad design, performance-wise.

Now, usually we prefer clean, smooth and fast implementations. But sometimes one have need to make trade-offs. `.htaccess` brings convenience and security (separation of concerns). 

In our experience, the possible performance degradation can not be experienced.