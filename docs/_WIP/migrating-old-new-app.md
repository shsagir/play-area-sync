---

template:      article
reviewed:      2016-05-16
title:         Migrating from Old App to New App
naviTitle:     Migrate: Old App > New App
lead:          Learn how to move your Old App to a New App.
group:         Kitchen_sink

seeAlsoLinks:
     - app
     - new-apps
     - old-apps
     - app-design
     - migrating

---

OK, you might have a loook what [changes with New Apps](new-apps) first to get a high level view. This guide is a skeleton to help you help you migrating from [Old App](old-apps) to New App. You will have your Old App and your New App running side by side, once the New App works perfectly, you can switch domains and turn off the Old App.


## Motivational intro

Yep, it takes some efforts from your side — to move your Apps. As much as we would like, we can not automate it. On the other side your Apps will be more affordable, future-proof and faster. It's also an opportunity for a spring cleaning. And you'll likely learn some cool new tricks here. Thank you in advance investing your time in a change with us.

**We are here to help!** Don't hesitate to ping us, when you hang somewhere or you have a question. Now let's go:


## Create a New App

Create a New App — this is where you will have it hosted in the future. Use an App-name easy to identify. Please go ahead and use the trial mode and ask us to extend your trial time. We are happy to do so, as we don't want you to pay for more than you need.


## Get your New App up and running

The Old App should stay untouched until we have the New App up.

### Setup Git

The first step is to add the New Apps Git remote as a new remote to your local Git repo. You'll have two remotes then, one for the Old App, one for the New App. We advice to create a new local branch to have your code changes for the New App save (maybe new-app), this way you can still work with the Old App. Remember to always push to the master branch, when pushing to fortrabbit. The Git branch setup might look something like this:

```
local: master  -> fortrabbit remote: Old App / master
local: new-app -> fortrabbit remote: New App / master
```

### Deploy code to the New App

With the New App you can `git push` to deploy your code base to remote. Composer packages might be installed during the first run. Please also see the [deployment article](/deployment) for details. You probably can already see your New App in the browser already — but most likely it will throw an error. 



### Setup Object Storage

When you have 



### Migrate the MySQL database




## Get ready for the switch



## Switch



## Done

