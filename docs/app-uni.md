---

template:      article
naviTitle:     "Universal Stack"
title:         "About the Universal Stack"
reviewed:      2016-12-08
lead:          'With each App you create, you can choose between two technology stacks. This article helps you to understand the Universal Stack.'
group:         platform
stack:         uni
showAlways:    true

---


The Universal Stack is made for general purpose PHP web development — websites and web applications. It's design unifies legacy development workflows with modern web development paradigms. 

## Target audience

The Universal Stack is, well, universally usable. Everyone should be able to use the Universal stack out-of-the-box. It's a good start for developers new to the cloud to get their feet wet without having to learn a bunch of new technologies and workflows. In short: The target groups is everybody.


## Specs, limits, purpose

* [Visit our specs page](https://www.fortrabbit.com/specs) to see some hard numbers
* [See our stack article](/stacks) to learn more about the options


## Application types

Unless your project is very resource intensive, there is virtually no application type which won't fit. To name a few:

* CMS driven websites
* Static websites
* Small e-shops
* Blogs
* Weekend projects

## Persistent storage

Persistent storage is a fancy word to describe: it's just regular storage, which won't be deleted. In contrary to [Ephmeral storage](app-pro#toc-ephemeral-storage), it allows you to use the website storage as you would with any VPS or shared hosting solution. All data your App writes is written to the disk and not removed upon deploy. In short: It is persistent.

## Logs

You can access either live logs or historic logs of your App. Please [read the logging article](logging-uni).


## Scaling

The purpose of the Universal Stack is not to be very scalable in terms of performance. The various plans differ in amount of available storage (Web and MySQL), but not in "PHP power". This means: your site can grow over time with bigger databases and more user uploaded contents, but it won't deliver vastly more performance in higher plans then in lower ones.

If you need more horse power and options, please see how you can: [Migrate from Universal App to Professional App](/migrate-uni-to-pro).

## Downgrading

Sorry, at this time it is not possible to downgrade from a higher to a lower plan in the Univeral Stack. While this would be nice for users - please believe that it is not our aim to lock you in - it would be quite complicated to achieve. To downgrade, we would have to implement several limit checks and give infos in the Dashboard which limits should have be to adjusted before downgrading.

As an alternative we suggest to throw away the current App and create a new one: Download the [backups](/bakcups-uni), delete the App, create a new App with correct plan and upload (or deploy) again.

Thanks for understanding!
