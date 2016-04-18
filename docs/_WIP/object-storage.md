---

template:      article
reviewed:      2016-04-11
title:         Object Storage 
naviTitle:     Object Storage
lead:          How to work with files that are not part of your code base in a modern cloud application. 

group:         Components

keywords:
    - S3
    - riak
    - assets
    - uploads
    - runtime-data
    - Amazon s3
    - cloud files
    - IAM
    - Google Cloud

seeAlsoLinks:
    - ssh-sftp-old-app
    - scaling

---

## Problem

Back in the days you had persistent storage with your local file system on the server. So you could upload an image to the server and just serve it from there. This is not possible with fortrabbit [New Apps](/new-apps) or any other modern 12-factor-app environment. Those have an [ephemeral file system](/new-apps#toc-asset-storage), which means that all changes to the file system, will be wiped on each deploy – on each `git push`. So you need a place to store stuff you'd like to keep. And apart from that you might also want to …

* separate content and code to make your App resilient
* keep your Git clean and lean to keep deployments fast
* separate HTTP and PHP requests to increase performance

## Solution

´´´
                  ┌────────────────┐                        ┌──────────────────────────────────────────┐
                  │                │                        │                                          │
                  │                ├───────────link─────────▶                                          │
┌─────────┐       │                │                        │                                          │
│         │       │                │                        │                                          │
│ Visitor ├───────▶  HTML content  │   ┌───────┐            │              Object Storage              │
│         │       │                │   │       │            │                                          │
└─────────┘       │                ◀───┤  App  ├──────S3────▶                                          │
                  │                │   │       │            │ ┌─────┐┌─────┐┌─────┐┌───────┐┌────────┐ │
                  │                │   └───────┘            │ │.jpg ││.png ││.mp4 ││.min.js││.min.css│ │
                  │                │                        │ └─────┘└─────┘└─────┘└───────┘└────────┘ │
                  └────────────────┘                        └────────────────────▲─────────────────────┘
                                                                                 │                      
                                                                            S3 protocol                 
                                                                                 │                      
                                                                           ┌─────┴─────┐                
                                                                           │           │                
                                                                           │ Developer │                
                                                                           │           │                
                                                                           └───────────┘                
´´´

The fortrabbit Object Storage is a multi purpose solution for offshore file distribution. Use it to store user uploads, generated files from your App and other static assets, such as logos, compressed JS and CSS. 


### Implementation

The fortrabbit Object Storage is very similar to the AWS S3 cloud storage, thus it is compatible with most S3 clients, plugins and libraries. In fact it is based on S3 on the backend, but uses a customized implementation on the front.


## Booking & scaling

The Object Storage is implemented as an core App Component. It's use is optional and it comes in different sizes. You can [scale](scaling) it up and down any time without downtimes in the Dashboard. 


## Pricing

The Object Storage is sized in reasonable packages. Traffic is cumulated together with the total traffic of your App. See the [pricing details page](https://www.fortrabbit.com/specs) for up-to-date costs and package sizes. 


### Exceeding the quota

Please mind that you will not be updated automatically, when you exceed the Object Storage quota. So when you reach the limit of the  You have to login to the Dashboard and upgrade the Component to a bigger plan yourself. You will however see 


## Accessing the Object Storage

First you'll need to to put files on the Object Storage then you can serve them.


### Obtaining upload credentials

To upload files to the Object Storage you'll need to identify with: a Bucket name, maybe a server?, a key and a secret. Those are storeed with the [App secrets](/secrets) (Dashboard > Your App > App Secrets). Please see the Dashboard for copy/paste commands to obtain the credentials.

To upload files to the Object Storage you have two general directions:


### 1. Programmatic upload from within the App

This is what you want to do in most cases. Your App will handle user uploads or runtime data on the Object Storage. The idea is, that you flip the local file storage with a remote one. You upload files in the cloud and then "hot-link" them in your output. The way this is implemented depends on your framework/CMS of choice. The most common Composer packages for file abstraction (AWS S3 Adapter, SDK V3) are: 

* [Flysystem](http://flysystem.thephpleague.com/) by The PHP League / Frank De-Jonge, newer more hip
* [Gaufrette](https://github.com/KnpLabs/Gaufrette) by KnpLabs, a bit older, also actively maintained

Most CMS/frameorks offer easy-to-use plugins for either one of the above.


#### Laravel

Just use Flysystem, which can easily be installed via Composer. More infos [over here](/install-laravel#toc-). Use at least Laravel 5.1.
<!-- 
Laravel 4 flysystem bridge
https://github.com/GrahamCampbell/Laravel-Flysystem/tree/v1.0.0
-->

<!-- TODO:
what's up with this service provider?
https://github.com/aws/aws-sdk-php-laravel
-->


#### Drupal

There is an extra module that adapts Flysystem with Drupal. More over [here](/install-drupal).


#### WordPress

You can use [WP Offload S3 Lite](https://wordpress.org/plugins/amazon-s3-and-cloudfront/) to upload and serve files. More infos [here](/install-wordpress).

<!--
TODO/TBD: 
endpoint not supported?
https://github.com/deliciousbrains/wp-amazon-s3-and-cloudfront/blob/master/classes/amazon-s3-and-cloudfront.php#L2122

Requires this 'plugin' (plugins/amazon-s3-alternative.php)

<?php
/*
Plugin Name: Amazon S3 alternative
*/

add_filter('aws_get_client_args', function($args) { 
	if (getenv('S3_API_ENDPOINT')) {
		$args['endpoint'] = getenv('S3_API_ENDPOINT');
	}
	return $args; 
});

-->


#### Symfony

Use can either use the [OneUp Flysystem bundle](https://github.com/1up-lab/OneupFlysystemBundle) or the [Gaufrette bundle](https://github.com/KnpLabs/KnpGaufretteBundle). More infos [here](/install-symfony).

<!-- TODO / TBD:
what's up with this AWS service provider?
https://github.com/aws/aws-sdk-php-symfony
-->

#### Craft CMS

There is [something in the works](https://github.com/pixelandtonic/Craft-Release/blob/master/app/assetsourcetypes/S3AssetSourceType.php) for Craft 3.

<!-- TODO / TBD:
Craft 2 is important! We try to get a patch in core.
-->


#### Shopware

Use the [SwagMediaS3](https://github.com/shopwareLabs/SwagMediaS3) Amazon S3 adapter.

<!-- 
TODO/TBD: 
endpoint missing?
https://github.com/shopwareLabs/SwagMediaS3/blob/master/Bootstrap.php#L96
-->

#### Magento

There is the [Arkade S3 Extension](https://github.com/arkadedigital/magento2-s3) which you can use.

<!--
TODO/TBD: 
endpoint not supported? PR 
https://github.com/arkadedigital/magento2-s3/blob/master/Model/MediaStorage/File/Storage/S3.php#L59
-->

#### eZ publish

...

<!--
PR/Patch for S3 on the way
-->


#### Custom/plain PHP applications

There is an official [AWS PHP SDK](https://github.com/aws/aws-sdk-php) from Amazon you can use.




### 2. Manual upload

In some scenarios you might want to upload the files manually, one by one. Or you want to manually check how your Object Storage is doing. You can do so with:

* **Cyberduck**, a free cross platform GUI client has been successfully tested
* Transmit, a GUI client for MacOs X has been tested
* s3cmd, a cross platform command line tool has been tested

In those cases S3 behaves pretty much like your good old friend FTP. You can do all kind of CRUD operations there.

### HTTPs access

Once you have uploaded files, you want to serve them in the browser. That's easy as linking to:

´´´
https://your-app.assets.frb.io/myfile.jpg
´´´

Replace `your-app` with the actual name of your [App](app) and `myfile.jpg` with the actual name of your file. Note that we recommend to use a secured connection via `https` but that it is not required. You might notice that Object Storage files are served via HTTP/2.

Please mind that most plugins will already rewrite the URLs in your template with the correct remote ones.


## Advanced usage, troubleshooting & quirks

Still reading? Go on and dig the details:


### Caching

All files on the Object Storage will be cached for a short time by default. The benefit of this is, that cached files are getting delivered even faster as no requests to backing file system need to be made. The downside is that you or your users will not see latest updates on a file immediately. Say for instance that your App allows to crop an image. Now your application will be replace the old original file with new cropped version. But still the cached version will be delivered for a while.

You want:

* no caching when your files often change, during development for instance
* **little caching** when your files might change from time to time
* massive caching to happen when your files will never change

Little caching is the default. You can control caching in two ways:

* Object Storage cache settings in the Dashboard: quickly change the default
* Over-ride the default with: custom HTTP cache headers on individual files


### Resetting the secret key

You might want to change the secret key to your Object Storage from time to time, for example when your team changed. You can do so in the Dashboard, there is a button labled "Reset the Object Storage secret key" and it does exactly that.


### Uploading PHP to the Object Storage

You can theoretically also upload PHP files to the Object Storage — but that will not make much sense, as those will not be executed.


### Using the Object Storage for static site hosting

You can theoretically also upload the build of a static site. But that also doesn't makes much sense as you can't put custom domains on the Object Storage and it also coupled to a PHP App.


### Differences to AWS S3

The Object Storage is tightly integrated with fortrabbit. You don't need to fight with complicated IAM rules and the AWS console. Most plugins & clients will work. Please keep in mind that the domain is always tight to our fortrabbit App, AWS URLs will not work for upload or HTTP access. The Object Storage uses the S3 protocol version 4.


### Deploying static assets to the Object Storage

You usually Git push to deploy all your files. In our [assets blog article](https://blog.fortrabbit.com/i-love-assets) we have discussed several solutions to deal with compressed front-end assets such as minified and concatenated JS and CSS — those actually should not be part of your Git. We advice to exclude the files from Git, generate them locally and upload them directly from your build tool. You can do so, with Gulp for example and an extra S3 plugins, such as `gulp-awspublish`. 

Please mind to find a plugin that supports to send over the `endpoint`, as standard AWS locations will not work.



### No multi-part upload

In some cases, very big uploads (500 MB for example) might be splitted into multiple parts and put together after upload again — this is called multi-part-upload. This can be done by your plugin or client — mostly in the background. Please mind that the fortrabbit Object Storage currently does not multi-part uploading, so please switch it off.


### The private folder

On root level of your Object Storage you'll find a virtual folder called `private`. This folder is special, as you can not access any of it's contents by HTTP (via the browser). You can only do so via PHP. 


### Data center location

The location of the Object Storage will match the Apps location. So if you choose your App to be hosted in Virginia, the files in your Object Storage will be there as well. We have ideas to extend the Object Storage with by CDN functionality (think CloudFront).


## Big files

The Object Storage is laid out to handle lot's of small to medium sized files, not very large files. Please see our [specs](https://fortrabbit.com/specs) table for current limitations. 


## No directory listings

It is currently not possible to activate directory listings. Place an `index.html` file with a custom lisiting.



## No versioning

Version history on file basis is currently not supported.


## Advanced S3 Bucket API

Currently only standard CRUD S3 operations (protocol version 4) are supported. Advanced policies, like IAM access right changes and most operations on bucket level are not allowed.


## Custom domains

The Object Storage can only be accessed by HTTP via the standard app-name related URL. You can not route any custom domains. Neither you can use your own TLS (SSL) termination here.


## Alternatives

The use of the Object Storage is optional, you might not need it or you can use AWS S3, or Rackspace Cloud Files or alike. You probably also have a look at image transformation services, which will render images in any possible format/compression and size.
