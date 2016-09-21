---

template:      article
title:         "Which Stack to choose"
reviewed:      2016-09-15
naviTitle:     Hobby & Professional Stack
lead:          "With each App you create, you can choose between two technology stacks: Hobby Stack & Professional Stack. This article helps you to decide."
group:         platform
stack:         all

---

## Hobby Stack

Hobby Stack Apps are made for general purpose PHP web development — websites and web applications. It's design is a mix of legacy support with good backward compatibility — while also supporting modern web development.

### From xs to m



### Persistent storage

adsasd

- - -

## Professional Stack

Professional Stack Apps are strictly made for modern PHP development — web applications. It's designed for high performance and high availability. 

### From xxs to xxl



### Target audience

To master the advanced features you need to be aware of modern patterns and workflows like file system abstraction, caching, Git, Composer, SSH usage.


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