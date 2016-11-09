---

template:      article
naviTitle:     "About the Stacks"
title:         "About the Stacks"
reviewed:      2016-11-09
lead:          "With each App you create, you can choose between two technology stacks. This article helps you to understand why there are two stacks and how to decide decide."
group:         platform
stack:         all

---

## The facts table

|                             | Universal Stack                | Professional Stack                                |
| --------------------------- | ------------------------------ | ------------------------------------------------- |
| Websites per App            | 1 [Why?](/app)                 | 1 [Why?](/app)                                    |
| Pricing model               | Fixed plans                    | Components: 8 - 600                               |
| Price per App (EUR / USD)   | 3, 6, 12                       | 8 - 600                                           |
| Traffic                     | low, medium                    | low - very high                                   |
| Scalability                 | xxs - s                        | xxs - xxxl                                        |
| High Availability           | no                             | optional                                          |
| Local storage               | persistent                     | [ephemeral](#toc-ephemeral-storage)               |
| Primary Application type    | websites                       | web applications                                  |
| Secondary Application type  | web applications               | websites                                          |
| Architecture                | Classical + magical extras     | 12-factor                                         |
| Required skill level        | Ok for beginners               | sophisticated                                     |
| Deployment protocols        | SFTP, Git, SSH                 | Git                                               |
| Composer intergration       | after Git push + via SSH       | after Git push                                    |
| SSH integration             | [standard SSH](ssh-uni)        | [remote SSH execution](/remote-ssh-execution-pro) |


## Universal Stack

Universal Stack Apps are made for general purpose PHP web development — websites and web applications. It's design is a mix of legacy support with good backward compatibility — while also supporting modern web development.

### Scaling from xs to m

The Universal Stack isn't really scalable. In fact the PHP memory for the different

### Target audience

The Universal Stack is, well universal. It's a good choice for noobs to get started with cloud hosting, it also suits

### Application types



### Persistent storage

adsasd

- - -

## Professional Stack

Professional Stack Apps are strictly made for modern PHP development — web applications. It's designed for high performance and high availability.

### From xxs to xxl

The Professional App is made


### Target audience

To master the advanced features you need to be aware of modern patterns and workflows like file system abstraction, caching, Git, Composer, SSH usage.

### Application types



### Plans & prices

The service structure is modular. Most Components come in different size and flavors, you can combine anything as needed. Please see our **[pricing page](https://www.fortrabbit.com/pricing-pro)** for an up-to-date pricing & product table.


### High availability

All production level


### Ephemeral storage

Each Professional Stack App has a [limited amount](https://www.fortrabbit.com/specs#limits) of local non-persistent, floating — so called ephemeral storage. It's a bit of a unique concept and might look very strange at first, but it's actually a corner stone of cloud enabled applications. From the [12-factor Apps](http://12factor.net/) manifesto:

> The filesystem … can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database. The twelve-factor app never assumes that anything cached … on disk will be available on a future request or job … a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.

So you better not store any runtime data, like user uploads there. Here is what might can happen: You install [WordPress](install-wordpress-pro), you upload images via the wp-admin. Everything works fine, but then you deploy code again and everything you have uploaded is gone.

During each [deployment](/git-deployment) the whole local storage gets whipped and replaced. See also our [deployment video](/deployment-architecture-video) to learn about what happens in the background.

But we have a good solution. With the [Object Storage Component](/object-storage) you can offshore your static assets. It's similar to Amazon S3. It's a very scalable solution that also brings other benefits and speed improvements. You can easily integrate it with a plugin or Composer package and some config, see your framework/CMS guides here as well.

### Components

Think of Components as micro services. Some Components are integral — meaning: you can't turn them off — while others are optional. Most Components are available in multiple expansion states (scaling plans). There are presets with common combination of Components.


### Logs

Only [live logs](logging) are available.
