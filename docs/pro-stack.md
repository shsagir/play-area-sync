---

template:      article
title:         "Working with the Professional Stack"
reviewed:      2016-09-15
naviTitle:     Professional Stack
lead:          "What you need to know when working with the Professional stack."

group:       Kitchen_sink
---

TODO!


## Ephemeral storage

Each App has a [limited amount](https://www.fortrabbit.com/specs#limits) of local non-persistent, floating — so called ephemeral storage. It's a bit of a unique concept and might look very strange at first, but it's actually a corner stone of cloud enabled applications. From the [12-factor Apps](http://12factor.net/) manifesto:

> The filesystem … can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the results of the operation in the database. The twelve-factor app never assumes that anything cached … on disk will be available on a future request or job … a restart (triggered by code deploy, config change, or the execution environment relocating the process to a different physical location) will usually wipe out all local (e.g., memory and filesystem) state.


So you better not store any runtime data, like user uploads there. Here is what might can happen: You install [WordPress](install-wordpress), you upload images via the wp-admin. Everything works fine, but then you deploy code again and everything you have uploaded is gone. 

During each [deployment](/git-deployment) the whole local storage gets whipped and replaced. See also our [deployment video](/deployment-architecture-video) to learn about what happens in the background.

But we have a good solution. With the [Object Storage Component](/object-storage) you can offshore your static assets. It's similar to Amazon S3. It's a very scalable solution that also brings other benefits and speed improvements. You can easily integrate it with a plugin or Composer package and some config, see your framework/CMS guides here as well.

## Logs

Only [live logs](logging) are available.