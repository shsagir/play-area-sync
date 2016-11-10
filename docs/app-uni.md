---

template:      article
naviTitle:     "Universal Stack"
title:         "About the Universal Stack"
reviewed:      2016-11-10
lead:          'With each App you create, you can choose between two technology <a href="/stacks">stacks</a>. This article helps you to understand the Universal Stack.'
group:         platform
stack:         uni

---


The Universal Stack is made for general purpose PHP web development — websites and web applications. It's design unifies legacy development workflows with modern web development paradigms. Everyone should be able to use the Universal stack out-of-the-box.

## Specs & limits

Please visit our [specs page](https://www.fortrabbit.com/specs) to see some hard numbers.

## Scaling

The purpose of the Universal Stack is not to be very scalable in terms of performance. The various plans differ in amount of available storage (Web and MySQL), but not in "PHP power". This means: your site can grow over time with bigger databases and more user uploaded contents, but it won't deliver vastly more performance in higher plans then in lower ones.

## Target audience

The Universal Stack is, well, universally usable. It's a good start for developers new to the cloud to get their feet wet without having to learn a bunch of new technologies and workflows. In short: The target groups is everybody.

## Application types

Unless your project is very resource intensive, there is virtually no application type which won't fit. To name a few:

* Company homepages
* Small community sites
* Small e-shops
* Blogs
* Weekend projects

## Persistent storage

Persistent storage is a fancy word to describe: it's just regular storage, which won't be deleted. In contrary to [Ephmeral storage](app-pro#toc-ephemeral-storage), it allows you to use the website storage as you would with any VPS or shared hosting solution. All data your App writes is written to the disk and not removed upon deploy. In short: It is persistent.

## Logs

You can access either live logs or historic logs of your App. Please [read the logging article](logging-uni).
