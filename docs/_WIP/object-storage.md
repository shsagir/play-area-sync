---

template:      article
reviewed:      2016-04-10
title:         Object Storage 
naviTitle:     Object Storage
lead:          How to work with files that are not part of your code base.

group:         Components

keywords:
     - S3
     - riak
     - 

seeAlsoLinks:
    - ssh-sftp-old-app
    - scaling

---

## Problem

You want to separate content and code. You want to keep your Git clean and lean. You don't want to store static assets such as compiled JavaScript and CSS in your Git. Your App is generating content on file system level which should not be destroyed (mind the ephemeral storage). You want to separate the HTTP requests from the PHP requests for better performance.


## Solution

The fortrabbit Object Storage is a multi purpose solution for offshore file distribution. Use it to store user uploads, generated files from your App and other static assets, such as logos, compressed JS and CSS. 

### Implementation

The fortrabbit Object Storage is very similar to the AWS S3 storage, thus it is compatible with most S3 clients and libraries. 


## Booking & scaling

The Object Storage is implemented as an App Component. It's use is optional and it comes in different sizes. You can [scale](scaling#toc-mysql) it up and down any time without downtimes in the Dashboard. See the [pricing details page](https://www.fortrabbit.com/specs) for up-to-date costs.

## Structure

Each Object storage is associated to an App. The top level is the bucket and it's name matches the Apps name. On root level you have three folders: 

```
/public   < accessible via public URL
/private  < only accessible via PHP
/system   < read only for future features
```

## Accessing the Object Storage

To upload files to the storage you basically have general directions:

### Manual upload

In some scenarios you might upload the files manually, you can do so with:

* Transmit, a GUI client for MacOs X has been tested
* Cyberduck, a free cross platform GUI client is not supported yet
* s3cmd, a cross platform command line tool has been tested

### Programmatic upload

In most cases you want your App to handle the files, you can do 

* flysystem
* Drupal
* WordPress
* Symfony
* …

### 



### Obtaining credentials

To access the Object Storage you'll need a Bucket name, a server?, the key and the secret. You will find all of this in the [App secrets]().

```

```


## Alternatives

The Object Storage is optional, you can use AWS S3, or Rackspace Cloud Files or alike.