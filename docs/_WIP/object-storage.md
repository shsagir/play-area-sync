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

seeAlsoLinks:
    - ssh-sftp-old-app
    - scaling

---

## Problem

Back in the days you have had persistent storage with your local file system on the server. Upload an image to the server and just serve it from there. This is not possible with fortrabbit [New Apps](/new-apps) and any other modern 12-factor-app environments. Those (only) have [ephemeral storage](/new-apps#toc-asset-storage), which means that all changes to the file system, will be wiped on each deploy (git push). So you need a place to store stuff you'd like to keep. You also want to …

* separate content and code. 
* keep your Git clean and lean. 
* separate the HTTP and PHP requests to increase performance.

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

The fortrabbit Object Storage is very similar to the AWS S3 cloud storage, thus it is compatible with most S3 clients, plugins and libraries. Infact it is based on S3 on the backend, but uses a customized implementation on the front.


## Booking & scaling

The Object Storage is implemented as an core App Component. It's use is optional and it comes in different sizes. You can [scale](scaling) it up and down any time without downtimes in the Dashboard. 


## Pricing

The Object Storage is sized in reasonable packages. Once you exceed the packages quota, you'll be upgraded. Traffic is cumulated together with the total traffic of your App. See the [pricing details page](https://www.fortrabbit.com/specs) for up-to-date costs and package sizes. 


## Accessing the Object Storage

First you'll need to to put files on the Object Storage then you can serve them.


### Obtaining upload credentials

To upload files to the Object Storage you'll need to identify with: a Bucket name, maybe a server?, a key and a secret. Those are storeed with the [App secrets](/secrets) (Dashboard > Your App > App Secrets). Please see the Dashboard for copy/paste commands to obtain the credentials.

To upload files to the Object Storage you have two general directions:

### 1. Programmatic upload from within the App

This is what you want to do in most cases. Your App will handle user uploads or runtime data on the Object Storage. The idea is, that you flip the local file storage with a remote one. You upload files in the cloud and then "hot-link" them in your output. The way this is implemented depends on your framework/CMS of choice. Most offer easy-to-use plugins for this:

#### Laravel

Just use flysystem, which abstracts the Aws S3 Adapter - SDK V3 which can easily be installed via Composer. More infos [over here](/install-laravel#toc-).

#### Drupal

asdasd asdasd

#### WordPress

asdasd asasd

#### Symfony

asdasd asdasd asd


### 2. Manual upload

In some scenarios you might want to upload the files manually, ony by one. Or you want to manually check how your Object Storage is doing. You can do so with:

* **Cyberduck**, a free cross platform GUI client has been successfully tested
* Transmit, a GUI client for MacOs X has been tested
* s3cmd, a cross platform command line tool has been tested

The clients pretty 


### HTTPs access

Once you have uploaded files, you want to serve them in the browser. That's easy as linking to:

´´´
https://your-app.assets.frb.io/myfile.jpg
´´´

Replace `your-app` with the actual name of your [App](app) and `myfile.jpg` with the actual name of your file. Note that we recommend to use a secured connection via `https` but that it is not required. You might notice that Object Storage files are served via HTTP/2.

Please mind that most plugins will already rewrite the URLs in your template with the correct remote ones.


## Advanced usage, troubleshooting & quirks

Still reading? Go on digg the details:


### Resetting the secret key

You might want to change the secret key to your Object Storage from time to time, for example when your team changed. You can do so in the Dashboard, there is a button labled "Reset the Object Storage secret key" and it does exactly that.


### Uploading PHP to the Object Storage

You can theoretically also upload PHP files to the Object Storage — but that will not make much sense, as those will not be executed.

### Using the Object Storage for static site hosting

You can theoretically also upload the build of a static site. But that also doesn't makes much sense as you can't put custom domains on the Object Storage and it also coupled to a PHP App.


### Differences to AWS S3

The Object Storage is tightly integrated with fortrabbit. You don't need to fight with complicated IAM rules and the AWS console. Most plugins & clients will work. Please keep in mind that the domain is always tight to our fortrabbit App, AWS Urls will not work for upload or HTTP access.


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



TB filesize (denke Max-Groesse wird im unteren GB Berreich liegen)


## Advanced S3 Bucket API

Currently only standard S3 operations (protocol version 4) are supported.


## Custom domains

Currently, the Object Storage can only be accessed by HTTP via the standard app-name related URL. You can not route any custom domains. Neither you can use your own TLS (SSL) termination here.


## Alternatives

The use of the Object Storage is optional, you might not need it or you can use AWS S3, or Rackspace Cloud Files or alike. You probably also have a look at image transformation services, which will render images in any possible format/compression and size.