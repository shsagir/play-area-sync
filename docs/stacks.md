---

template:      article
naviTitle:     "About the Stacks"
title:         "About the Stacks"
reviewed:      2016-10-15
lead:          "With each App you create, you can choose between two technology stacks. This article helps you to understand why there are two stacks and how to decide."
group:         platform
stack:         all

---

## The facts table

|                             | Universal Stack                         | Professional Stack                                |
| --------------------------- | --------------------------------------- | ------------------------------------------------- |
| Websites per App            | 1 [Why?](/app#toc-one-website-per-app)  | 1 [Why?](/app#toc-one-website-per-app)            |
| Pricing model               | fixed plans                             | flexible Components                               |
| Price per App               | 3, 6, 12 (EUR / USD)                    | 8 - 3000 (EUR / USD)                              |
| Traffic                     | low, medium                             | low - very high                                   |
| Scalability                 | xxs - s                                 | xxs - xxxl                                        |
| High Availability           | no                                      | yes, except Development plans                     |
| Local storage               | persistent                              | [ephemeral](#toc-ephemeral-storage)               |
| Primary Application type    | websites                                | web applications                                  |
| Secondary Application type  | web applications                        | websites                                          |
| Architecture                | single webserver + db server            | distributed containers                            |
| Required skill level        | beginner                                | sophisticated                                     |
| Deployment protocols        | SFTP, Git, SSH                          | Git                                               |
| Composer integration        | after Git push, via SSH                 | after Git push                                    |
| SSH integration             | standard                                | [remote SSH execution](/remote-ssh-execution-pro) |


## Universal Stack

The Universal Stack is made for general purpose PHP web development — websites and web applications. It's design unifies legacy development workflows with modern web development paradigms. If you ever have deployed any website anywhere, you should be able to use the Universal stack out-of-the-box or with only minimal effort.

### Scaling

The purpose of the Universal Stack is not to be very scalable in terms of performance. The various plans differ in amount of available storage (Web and MySQL), but not in "PHP power". This means: your site can grow over time with bigger databases and more user uploaded contents, but it won't deliver vastly more performance in higher plans then in lower ones.

### Target audience

The Universal Stack is, well, universally usable. It's a good start for developers new to the cloud to get their feet wet without having to learn a bunch of new technologies and workflows. In short: The target groups is everybody.

### Application types

Unless your project is very resource intensive, there is virtually no application type which won't fit. To name a few:

* Company homepages
* Small community sites
* Small e-shops
* Blogs
* Weekend projects

### Persistent storage

Persistent storage is a fancy word to describe: it's just regular storage, which won't be deleted. In contrary to [Ephmeral storage](#toc-ephemeral-storage), it allows you to use the website storage as you would with any VPS or shared hosting solution. All data your App writes is written to the disk and not removed upon deploy. In short: It is persistent.


#### Logs

You can access either live logs or historic logs of your App. Please [read the logging article](logging-uni).

- - -

## Professional Stack

Professional Stack is made for modern PHP development workflows building modern web applications. It subscribes to the [12-factor paradigm](https://12factor.net/)

### Scaling

Professional Apps are designed to be scalable. Any Professional App consists of multiple Components: PHP, [MySQL](mysql), [Memcache](memcache-pro), [Object Storage](object-storage). Think of Components as micro services. Some Components are integral — meaning: you can't turn them off — while others are optional. Each Component is individually scalable, allowing you to increase the resources where needed instead of having to pay for additional resources you don't need. Some Components are capped to an upper limit, but other Components can be scaled virtually infinitely.


### Target audience

To work with Professional Apps, you need to be familiar with modern patterns and workflows like: file system abstraction, caching, Git, Composer. If that matches your skill set, then the good news is: We offer the perfect infrastructure for you.

### Application types

It's less a question of application type and more a question of underlying frameworks/CMS. Not all are up2date in terms of modern PHP development. Some do not support storage abstraction, others are hard to combine with Git-only deployment. Either way: Most maintained frameworks/CMS are moving towards support of 12-factor environments, so that developers using them can leverage truly scalable infrastructures as well. To name a few of those framework/CMS which are easily usable:

* Laravel (since 5.0)
* Symfony (since 2.3)
* Drupal (since 8.2)
* Craft (since 2.5)

### Ephemeral storage

Ephemeral storage is a concept of the [12-factor model](https://12factor.net/), which says: Whenever you deploy new code changes, all (since your last deploy)  generated runtime data will be deleted. The best example for runtime data are user uploads.

This behavior sounds harsh on first reading, but makes sense if you dig deeper: Allowing to scale your App to high degrees implies that the App's resources must be horizontally scaled. Horizontal scaling, in contrast to vertical scaling, is the technique of adding more resources (more servers), instead of using bigger resources (bigger servers).

To allow an App to scale horizontally means that it runs on multiple servers on the same time. Those servers do not share a storage and all solutions, which would allow sharing storage, would need to build on a network storage, which is infamously slow by nature. To still allow persistent runtime data, your App needs to use an cloud storage, like our [Object Storage](object-storage) or [S3 from AWS](https://aws.amazon.com/s3/) or equivalent.

You still can use the regular file system, eg for temporarily storing uploads, but upon every deploy your App will only consist of your deployed code.

> The filesystem … can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database. The twelve-factor app never assumes that anything cached … on disk will be available on a future request or job … a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.


#### Logs

As a result of the horizontal scalable resources and the ephemeral storage, [logs can only be accessed live](logging-pro). If you need historic logs, we recommend [Logentries](logentries).

### High availability

Most plans are available as Development and Production. All Production plans, and those which are not grouped into both, are highly available.
