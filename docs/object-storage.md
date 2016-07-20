---

template:      article
reviewed:      2016-07-11
title:         Object Storage
naviTitle:     Object Storage
lead:          How to work with files that are not part of your code base.

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

Back in the days you had persistent storage with your local file system on the web server. This allowed you to upload an image - or any other static asset - directly to the server so that it would be served from there linked like so: `uploads/photo.jpg`. With modern cloud 12-factor-app based Infrastructures this is different:

fortrabbit Apps have an [ephemeral file system](/new-apps#toc-asset-storage). This means that all changes to the local file system will be undone with each deploy. So, each time you `git push` a completely new package will be deployed, replacing everything else on the Node.

Imagine you have a CMS and you are uploading images. Next time you push some changes, those images will be gone.



## Solution

```
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
```

So you need a place to store stuff you'd like to keep, something like `https://myapp.objects.frb.io/uploads/photo.jpg`. And apart from that you might also want to …

* separate content and code to make your App resilient
* keep your Git clean and lean to keep deployments fast
* separate HTTP and PHP requests to increase performance

The fortrabbit Object Storage is a multi purpose solution for offshore files. You can use it to store user uploads, any files your App generates and all other static assets: logos, compressed JS and CSS... you get the gist.


### Implementation

The fortrabbit Object Storage implements large parts of the AWS S3 REST API making it compatible with most S3 clients, plugins and libraries. In fact, it stores all objects in the highly available and endlessly scalable S3 space.


## Booking & scaling

The Object Storage is available as a core App Component. It is completely optional and it comes in different sizes. You can [scale](scaling) it up and down any time without downtimes from the Dashboard.


## Pricing

The Object Storage is sized in reasonable packages. Traffic is cumulated together with the total traffic of your App. See the [pricing details page](https://www.fortrabbit.com/specs) for up-to-date costs and package sizes.


### Exceeding the quota

Please mind that in alignment with all the other App Components the Object Storage will not be upgraded automatically: when you exceed the Object Storage quota you simply cannot upload more files and need to scale the component in the Dashboard. Again: No downtime, whatsoever.


## Accessing the Object Storage

First you'll need to to put files on the Object Storage then you can serve them.


### Obtaining upload credentials

To upload files to the Object Storage you'll need to authenticate with your individual credentials. Those consist of: a bucket name, a server (aka: endpoint), a key and a secret. As all credentials those are stored with the [App secrets](/secrets). Please see the Dashboard for copy/paste commands to obtain the credentials.

To upload files to the Object Storage you have two general options:


### 1. Programmatic upload from within the App

Your App handles user uploads or storing all other created runtime data on the Object Storage. To that purpose most modern Apps come with file system abstractions. Those allow you to easily switch out the storage layer by changing your App's configuration. How that is done depends on your framework/CMS. Since we live in the Composer age most of those abstraction libraries use the AWS S3 SDK, with which our Object Storage is compatible. The most commonly used libraries are:


#### Flysystem

[Flysystem](http://flysystem.thephpleague.com/) by The PHP League / Frank De-Jonge. Both available AWS Adapters are compatible with the Object Storage.


##### V2

```php
$secrets     = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
$credentials = [
    'bucket'   => $secrets['OBJECT_STORAGE']['BUCKET'],
    'endpoint' => 'https://'. $secrets['OBJECT_STORAGE']['SERVER'],
    'key'      => $secrets['OBJECT_STORAGE']['KEY'],
    'region'   => $secrets['OBJECT_STORAGE']['REGION'],
    'secret'   => $secrets['OBJECT_STORAGE']['SECRET'],
];
$client     = Aws\S3\S3Client::factory($credentials);
$adapter    = new League\Flysystem\AwsS3v2\AwsS3Adapter($client, $credentials['bucket'], 'flysystem-s3-v2');
$filesystem = new League\Flysystem\Filesystem($adapter);
$filesystem->put('hello', 'world...');
```

##### V3

```php
$secrets     = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
$credentials = [
    'credentials' => [
        'key'      => $secrets['OBJECT_STORAGE']['KEY'],
        'secret'   => $secrets['OBJECT_STORAGE']['SECRET'],
    ],
    'region'   => $secrets['OBJECT_STORAGE']['REGION'],
    'bucket'   => $secrets['OBJECT_STORAGE']['BUCKET'],
    'endpoint' => 'https://'. $secrets['OBJECT_STORAGE']['SERVER'],
    'version'  => 'latest',
];
$client     = Aws\S3\S3Client::factory($credentials);
$adapter    = new League\Flysystem\AwsS3v3\AwsS3Adapter($client, $credentials['bucket'], 'flysystem-s3-v3');
$filesystem = new League\Flysystem\Filesystem($adapter);
$filesystem->put('hello', 'world...');
```

#### Gaufrette


[Gaufrette](https://github.com/KnpLabs/Gaufrette) is an alternative file system abstraction by KnpLabs, a bit older, but also actively maintained.


#### Frameworks & CMS

Offshore files is relatively new concept, but support is growing. Most frameworks and CMS systems are already supporting it via plugins or modules. Here is a short list:


**Laravel**:  Just use Flysystem, which can easily be installed via Composer. More infos [over here](/install-laravel#toc-). Use at least Laravel 5.1.

<!-- TODO: Clean up tis stub: Laravel 4 flysystem bridge: https://github.com/GrahamCampbell/Laravel-Flysystem/tree/v1.0.0 what's up with this service provider? https://github.com/aws/aws-sdk-php-laravel -->

**Drupal 8**: There is an [module](https://www.drupal.org/project/flysystem) that adapts Flysystem with Drupal.

**WordPress**:  You can use [WP Offload S3 Lite](https://wordpress.org/plugins/amazon-s3-and-cloudfront/) to upload and serve files. More infos [here](/install-wordpress).

**Symfony**: Use can either use the [OneUp Flysystem bundle](https://github.com/1up-lab/OneupFlysystemBundle) or the [Gaufrette bundle](https://github.com/KnpLabs/KnpGaufretteBundle). More infos [here](/install-symfony).

**Craft CMS**: There is [something in the works](https://github.com/pixelandtonic/Craft-Release/blob/master/app/assetsourcetypes/S3AssetSourceType.php) for Craft 3.

**Shopware**: You might use the [SwagMediaS3](https://github.com/shopwareLabs/SwagMediaS3) Amazon S3 adapter (not tested).

<!--  TODO/TBD:  endpoint missing? https://github.com/shopwareLabs/SwagMediaS3/blob/master/Bootstrap.php#L96 -->

**Magento**: There is the [Arkade S3 Extension](https://github.com/arkadedigital/magento2-s3) which you might can use (not tested).

<!-- TODO/TBD: endpoint not supported? PR  https://github.com/arkadedigital/magento2-s3/blob/master/Model/MediaStorage/File/Storage/S3.php#L59 -->



#### Custom/plain PHP applications

There is an official [AWS PHP SDK](https://github.com/aws/aws-sdk-php) from Amazon you can use.


### 2. Manual upload

In some use-cases you want to upload files manually. Also you might want to manually review the existing files in your Object Storage. We recommend the following software clients:

* **[Cyberduck](https://cyberduck.io/)**, a free cross platform GUI client
* [Transmit](https://panic.com/transmit/), a GUI client for MacOs X
* [s3cmd](http://s3tools.org/s3cmd), a cross platform command line tool written in Python

In those cases S3 behaves pretty much like your good old friend FTP.

### HTTP(S) access

Once you have uploaded some files, the ultimate goal is of course to serve them to the browser. To that purpose all Apps come with an Object Storage URL in the form:

``` plain
https://{{app-name}}.objects.frb.io/path/to/file.jpg
```

Replace `path/to/file.jpg` with the actual path to your file. Note that we recommend to use a secured connection via `HTTPS` but that it is not required. Notice that the Object Storage supports HTTP/2 when using HTTPS.

Most framwork/CMS integrations will already rewrite the URLs in your templates with the correct URLs.

### Log access

As with all fortrabbit components you can use the [logging](help.fortrabbit.com/logging) service to tail live logs.


## Advanced usage, troubleshooting & quirks

Still reading? Go on and dig into the details:


### Caching

All files served from the Object Storage will be served with caching headers. Those caching headers have two effects: The client (browser) knows that it does not need to reload the files from the server, which, of course, makes things quite a bit faster. The other effect of the caching header is that the Object Storage server will cache the files as well. This might sound a bit strange on first view but the result is that besides re-visiting browsers also newcomers will get their files very fast, because they are read mostly from the memory of the Object Storage servers, which is extremely fast.

Not existing files (404) are also cached, but only shortly. See [specs](https://www.fortrabbit.com/specs) for details on default cache durations.


#### Manipulate cache durations

You can change the default cache durations of 24 hours in the Dashboard (Dashboard > App > Settings > Object Storage cache). If you need a finer granulation then you can simple set either of two headers: `Cache-Control` or `Expires`. Those will then be forwarded to the browser and also define the caching time on the server. A helpful guide to work with caching headers can be found [here](http://www.mobify.com/blog/beginners-guide-to-http-cache-headers/).

Take care that we don't do cache purging. So when changing the cache duration, only new assets will be effected. Assets already in the cache will stay there as long as the old value is past.

#### Cache busting

Caching is great but if you want to make changes appear immediately you need a way around them. One approach would be to set manual caching headers with a low value, but this would just annul the positive effect of caching. So what you want to do is query string versioning, eg like so:

* `https://{{app-name}}.assets.frb.io/path/to/file.jpg?2016-05-05.1`
* `https://{{app-name}}.assets.frb.io/path/to/file.jpg?2016-05-05.2`
* `https://{{app-name}}.assets.frb.io/path/to/file.jpg?2016-05-06.1`

Caching works on the whole URL, including the query string. So if you change the query string you are delivering accessing a different item, hence it's not cached. Many frameworks/CMS already do that for you, but it's easy to implement manually as well.

### Resetting the secret key

If you need to change the secret key of your Object Storage: Login to the Dashboard > App > Access > Object Storage and click on "Reset the Object Storage secret key".

### Uploading PHP to the Object Storage

You can upload PHP files to the Object Storage — but that will not make much sense, as those will not be executed. In fact, they will be displayed probably in plain text (depending on the Content-Type). So, unless you want to publish it: Don't do it.


### Using the Object Storage for static site hosting

At this point we do not support custom domains. So, yes, you can host a static page by eg redirecting your custom domain using `Location` header to your Object Storage URL but in the browser address bar will still be the Object Storage URL (ok, you can use iframes to hide that, but that's just nasty).


### Differences to AWS S3

The Object Storage is tightly integrated with fortrabbit. You don't need to fight with complicated IAM rules and the AWS console. Most plugins & clients will work out of the box. Please keep in mind that the domain is always tied to our fortrabbit App: AWS domains will not work for upload or HTTP access. The Object Storage supports both AWS v4 signatures and old AWS S3 signatures.

#### No multi-part upload

In some cases, very big uploads (500 MB for example) might be need to be split into multiple parts and put together after upload again. This is called multi-part-upload. This can be done by your plugin or client — mostly in the background. Please mind that the fortrabbit Object Storage currently does not support multi-part uploading.

#### Advanced S3 REST API

The following bucket or object actions are currently not support - but will reply with a dummy-OK message, to not break clients: ACL, CORS, Lifecycle, Policy, Replication, Tagging, Website, Logging, Notification, RequestPayment, Versioning.

CORS is partially supported: CORS rules will be not be accepted but a default CORS rules, which accepts all, is active.


### Deploying static assets to the Object Storage

You usually Git push to deploy all your files. In our [assets blog article](https://blog.fortrabbit.com/i-love-assets) we have discussed several solutions to deal with compressed front-end assets such as minified and concatenated JS and CSS — those actually should not be part of your Git. We advice to exclude the files from Git, generate them locally and upload them directly from your build tool.

#### Gulp, Grunt & co

You can automate the process of uploading files with a task runner or build script. You can use Gulp with an S3 plugin, such as `gulp-awspublish` or `gulp-S3`. Please mind to find a plugin that supports to send over the `endpoint`, as standard AWS locations will not work. Plugins that are based on `knox` or `aws-sdk` will probably work.


Example `objects.json`. Mind to replace all value with your own ones. Exclude this file from Git for security reasons.
```
{
  "key":      "{{app-name}}",
  "secret":   "your-long-object-storage-secret",
  "bucket":   "{{app-name}}",
  "region":   "eu-west-1",
  "endpoint": "objects.{{region}}.frbit.com"
}
```

Example of a `gulpfile.js`. Reading credentials from the json file then deploying files to the object storage via the S3 protocol.
```
var s3 = require("gulp-s3");

aws = JSON.parse(fs.readFileSync('./objects.json'));
gulp.src('./dist/**')
    .pipe(s3(aws));
```


### The private folder

On root level of your Object Storage you'll find a folder called `private`. This folder is special, as you can not access any of it's contents by HTTP (via the browser). You can only do so via PHP. Hence: You can use it for anything you want to store, but do not publish.


### Data center location

The location of the Object Storage will match the Apps location. So if you choose your App to be hosted in US, the files in your Object Storage will be there as well. We have ideas to extend the Object Storage with by CDN functionality (think CloudFront).


### Big files

The Object Storage is laid out to handle lot's of small to medium sized files, not very large files. Please see our [specs](https://fortrabbit.com/specs) table for current limitations.


### No directory listings

As with S3 it is not possible to list directories. You can place an `index.html` file containing your custom listing.


### Custom domains

The Object Storage can only be accessed by HTTP(S) via the standard App name related URL. You can currently not route any custom domains. Neither you can use your own TLS (SSL) termination here.


### Case sensitivity

Same as S3 the Object Storage is case sensitive. So you can upload `file` and `FILE` in the same folder. Also there is a difference between `https://{{app-name}}.objects.frb.io/file` and `https://{{app-name}}.objects.frb.io/FILE`.


## Alternatives

The use of the Object Storage is optional, you might not need it or you can use AWS S3, or Rackspace Cloud Files or alike. You probably also have a look at image transformation services, which will render images in any possible format/compression and size.
