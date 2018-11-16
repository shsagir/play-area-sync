---

template:    article
reviewed:    2018-11-16
title:       .htaccess
lead:        Browsing the docs here you will find lot's of reference to a mysterious invisible file called ".htaccess". What's that about? How can you make use of it?
naviTitle:   .htaccess
group:       deployment
workInProgress: yes
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

`.htaccess` is a hidden file that usually lives in the web root folder of your code base. It enables altering the web server's configuration directives. Therefore the syntax is a bit cryptic. `.htaccess` rules apply to all subdirectories. `.htaccess` is usually not excluded `.gitignore`so it will be deployed alongside. Take care: htaccess is a sharp sword. With great power comes great responsibility.

### .htaccess on fortrabbit

Your fortrabbit Apps are running on the Apache web server. You can make use of `.htaccess`. Missing (remember it's hidden) and wrong `htaccess` directives are common issues. These sensitive defaults are set on the fortrabbit platform regarding .htaccess:

* Apache configuration syntax 2.2 and 2.4 are supported 
* GZIP compression is enabled per default
* Access on all `.ht*` files is disabled, so nobody can read your .htaccess

<!-- Anything else? -->

### .htaccess and your framework or CMS

When you are using a framework or a CMS, chances are high, that you don't need to wrangle with `.htaccess` at all, as that comes built-in. Here are the most used ones:

* **Laravel** comes with a [boilerplate htaccess](https://github.com/laravel/laravel/blob/master/public/.htaccess) you can extend
* **Craft CMS** comes with a [boilerplate htaccess](https://github.com/craftcms/craft/blob/master/web/.htaccess) you can extend
* **WordPress** [manages htaccess](https://codex.wordpress.org/htaccess) for you


### .htaccess example

Talk is cheap, here is some code:

```htaccess
# This example shows how to redirect all requests to www domain
# THAT is not needed on fortrabbit  ;)
RewriteEngine on
RewriteCond %{HTTP_HOST} ^facebook\.com [NC]
RewriteRule ^(.*)$ https://www.facebook.com/$1 [L,R=301,NC]
```

## Using .htaccess

You usually will not have to wrangle with `.htaccess`. Modern frameworks and CMS come with predefined ones, that are also managed. You will also find examples in context on these help pages here on fortrabbit. Following are common categories of usage with examples: 

### Redirects

The most common use case for `.htaccess` is to re-write URLs with `mod_rewrite`. You can direct requests to a subdirectory, or specific domain, prettify URLs by omitting file endings, force https and much more.

#### Redirect all requests to the primary domain

Once you've added a [custom domain](/domains) you may want to prevent requests to your [App URL](/app#toc-app-url). The example below shows how to set up a redirect in your `.htaccess` file.

```htaccess
# From App URL to your domain
RewriteEngine On
RewriteCond %{HTTP_HOST} ^{{app-name}}.frb.io$ [NC]
RewriteRule ^(.*)$ https://www.your-domain.com/$1 [r=301,L]
```

<!--

TOOD or delete. Maybe not importnt.

#### Redirect custom domains to different folders

You can add as many [custom domains]((/domains)) to one App, as you want. With the Dashboard you can also set individual root paths for those. An alternative is to use .htaccess for that. In the example below domains are getting redirected into folders within the App.

```htaccess
# From App URL to your domain
RewriteEngine On
[TODO]
```
-->


#### Redirect all requests to https

**Force https!** There is no need for your application to be reached over a non-secure connection. Use `.htaccess` to redirect all `http://` requests over to `https://`. This is how:

```htaccess
RewriteEngine On
RewriteCond %{HTTP:X-Forwarded-Port} !=443
RewriteRule (.*) https://%{HTTP_HOST}/$1 [R=301,L]
```

Please note the X-Header part. Other code snippets you have pasted from elsewhere might not work here. Many CMS and frameworks are offering convenient settings and configurations for this.


#### Force HTTPS for future visits with HSTS

This goes one step further than just forwarding requests, it tells the browser to not ever again accept any connection in the future on not secured domain to your App. In your `.htaccess` file you can add this line (in addition to the rewrite rule above):

```htaccess
RewriteEngine on
Header always set Strict-Transport-Security "max-age=31536000"
```

This will make your browser remember to always use the secured version of your App. It makes use of the "[HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)" policy and improves security by eliminating the risks of man-in-the-middle TLS-protocol-downgrade attacks. Be careful: setting this header will tell the browser never (or that is: until max-age) to use `http://` again. So if you later on decide to serve (parts of) your site using no encryption, all those clients (browsers) which saw the header will not comply and keep using `https://`.

### Authentication

You can implement a simple standard browser prompt password check for your website without any extra PHP magic with .htaccess. Hop over to our dedicated [HTTP Auth article](/http-auth) to learn on how.


### Control HTTP headers

HTTP headers are part of an Hyper Text Transfer Protocol request and response. HTTP headers are defining the operating parameters of an HTTP transaction. Use htaccess to modify these.


#### Cache-Control

Don't serve the same content to the same client twice! Control how the browser of the client caches results and files locally. This is especially useful for asset resources that don't change often. Caching reduces the number of request and the data transmitted. On the HTTP part of your App/website caching is achieved by using HTTP headers. You can control the caching in htaccess like so:

```htaccess
# Example to cache images and CSS files
# adjust and extend to your needs
<ifModule mod_headers.c>
  #  images expire after 1 month
  <filesMatch ".(gif|png|jpg|jpeg|ico|pdf|svg|js)$">
    Header set Cache-Control "max-age=2592000"
  </filesMatch>
  # CSS expires after 1 day
  <filesMatch ".(css)$">
    Header set Cache-Control "max-age=86400"
  </filesMatch>
</ifModule>
```

#### CORS headers

For "Cross-Site XMLHttpRequests" you'll need "Cross-origin resource sharing" or in short CORS headers. Those are mostly used in context of JavaScript AJAX requests across different domains. Like when `domain-a.com` loads a script from `domain-b.com`. Per default this is not possible for security reasons. But you can enable it for certain or even all origins:

```htaccess
# Access all areas (use carefully)
Header add Access-Control-Allow-Origin "*"
Header add Access-Control-Allow-Methods: "GET,POST,OPTIONS,DELETE,PUT"
```

Use this with care and only open what you really need. Reduce the risk of XSS. Also check out the new Content Security Policy (CSP) for more advanced control on what you allow and what not. This website is a good reference: [content-security-policy.com](https://content-security-policy.com/)


### Custom error pages

You can define templates to make your error pages look more cool like so:

```htaccess
RewriteEngine on
ErrorDocument 404 /srv/app/{{app-name}}/htdocs/404.html
```

That applies to 4XX and 5XX errors. When using a framework or CMS likely the router will catch such errors and use PHP logic to resolve that, still some 5XX errors might appear before the programm can execute the router.


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
