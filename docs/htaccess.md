---

template:    article
reviewed:    2018-08-12
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

You usually will not have to wrangle with `.htaccess`. Modern frameworks and CMS come with predefined ones, that are also managed. You will also find copy-pastable examples in context on these help pages here on fortrabbit. Following are common categories of usage: 

### Redirects

The most common use case for `.htaccess` is to re-write URLs with `mod_rewrite`. You can direct requests to a subdirectory, add the www subdomain to all requests, prettify URLs by omitting file endings, [force https](https#toc-redirect-all-requests-to-https) and much more.

### Authentication

Hop over to our dedicated [HTTP Auth article](/http-auth) to learn on how to implement a simple password check.

### Custom error pages

You can also define templates to make your error pages look more cool.

### Gzip compression

You can set gzip compression for static files like .html, .js, .css, .jpg and alike, so that they get first compressed, then send over and then decompressed by the users browser. This might or might not speed up things.



## Tips and tricks

### Comment your .htaccess file

Apache directives with regular expressions tend to look glibberish. Make sure to comment what you have been doing there so that "the later you" can understand what's going on.


## Troubleshooting

Here are the most common mistakes we see in combination with `.htaccess`:


### Wrong location

The `.htaccess` file has to be placed in the [root path](/app#toc-root-path) of the website. Per default, that is the `htdocs` folder. But certain modern frameworks and CMS have path below that, like Laravel with `public`. So make sure to place your `.htaccess` in the correct directory.


### Missing .htaccess

Another common mistake we see quite often is to miss out the `.htaccess` file. The dot at the beginning of the file name indicates that this file should be invisible in your operating system. So when you dragged files in your FTP client from the Downloads folder, it's very likely that the `.htaccess` file was left home. 

In macOS Finder you can toggle invisible files with ` CMD + SHIFT + .`, easy to remember, like dot files. SFTP clients with a split view option are mostly showing those files by default. Take care, there are possibly other hidden files in your project, like `.gitignore` and `.env`.



## Possible performance issues

The fortrabbit hosting platform is managed. Therefore your access to critical services is limited. This is actually good for most parts. We believe in a clear separation of concerns: We run the servers, you do the coding. One consequence is that we can not give you direct access on the Apache web server configuration (httpd.conf).

The `.htaccess` file is a very common method to achieve common web server configurations. In fact, almost every framework and CMS comes with a `.htaccess` out of the box. `.htaccess` lives in your code base and is deployed along with Git and therefore it's just very convenient to use.  

Consider it a workaround that got too popular. The official Apache docs have a whole section on "[When NOT to use .htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html#when)". The NGNIX team has setup a marketing page [bad-mouthing .htaccess](https://www.nginx.com/resources/wiki/start/topics/examples/likeapache-htaccess/) for a bad design, performance-wise.

Now, usually we prefer clean, smooth and fast implementations. But sometimes one have need to make trade-offs. `.htaccess` brings convenience and security (separation of concerns). 

In our experience, the possible performance degradation can not be experienced.