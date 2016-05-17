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

OK, you might have a look what [changes with New Apps](new-apps) first to get a high level view. 

This guide is a skeleton to help you migrating from [Old App](old-apps) to New App, step by step. You will have your Old App and your New App running side by side, once the New App works perfectly, you can switch domains and turn off the Old App.


## Motivational intro

Yes: it takes some efforts from your side — to move your Apps. As much as we would like, we can not automate it. On the other side your Apps will be more affordable, future-proof and faster. It's also an opportunity for a spring cleaning. And you'll likely learn some cool new tricks here. Thank you in advance for investing your precious time in a change with us.

**We are here to help!** Don't hesitate to ping us, when you hang somewhere or you simply have a question. Now let's go:


## Create a New App

Set up a new New App (in the [Dashboard](dashboard)). This is where you will have it hosted in the future. Use an App-name easy to identify. 

Please go ahead and use the trial mode and ask us to extend your trial time immediately. We are happy to do so, as we don't want you to pay for more than you need. We can also give you a discount for the transfer period.

## Get your New App up and running

The Old App should stay untouched until we have the New App up.

**Copy Dashboard settings**: Check your Old Apps settings within the Dashboard and match your New App to those. Choose the same [root path](app#toc-set-a-custom-root-path) for both Apps. Enable the same PHP extensions. The PHP version most likely does not need to match. Try to run your App on PHP 7 first.


### Setup Git

The first step is to add the New Apps Git remote as a new remote to your local Git repo. You'll have (at least) two remotes then, one for the Old App, one for the New App. We also advice to create a new local branch to have your code changes for the New App save (maybe call it new-app), this way you can still work with the Old App and you don't mess up. Remember to always push to the master branch, when pushing to fortrabbit. The Git branch setup might look something like this:

```
local: master  -> fortrabbit remote: Old App / master
local: new-app -> fortrabbit remote: New App / master
```

It's up to your preference, you can also work with environment detection to differentiate between New and Old App. See our [multi-staging article](multi-staging) to get the idea.


### Deploy code to the New App

With the New App you can now `git push` to deploy your code base to remote. Composer packages might be installed during the first run. Please also see the [deployment article](/deployment) for details. You can already see your New App in the browser now — just visit the App URL of the New App. Most likely it will throw an error right now. Let's go on:


### Transfer & connect the MySQL database

Only applies if your App makes use of a [MySQL database](mysql) of course:

**Export the database**: Connect to your Old Apps database using the SSH tunnel – [see here](mysql-old-app#toc-remote-mys&ql-access). Export the database, you can download a .sql file to your local machine using a MySQL GUI.

**Import the database**: Connect to your New Apps database — [see here](mysql#toc-remote-mys&ql-access). Import the database, you can upload that local .sql file now.

**Connect the New App to the database**: Right now, the code base still contains the MySQL credentials for the Old Apps MySQL database. You need to change this to connect to the new database. Open the config file where your database access informations are stored. How this file is called and where it is located is defined by your framework or CMS or even by your own preference when using plain PHP. Please look in our install guide when unsure. 

The handling of passwords has changed a bit: Passwords are now stored with the [App Secrets](app-secrets). There is an SSH command to grab the current password, those can be found in the Dashboard, with your App under access.

Once you have your database access information stored, push your local new-app branch to the New Apps master branch again. Let's see if it works now — chances are good.


### Debugging errors

Don't panic when it still throws you an error now. This can have multiple causes and it's probably an individual problem. You might see a white screen, or a 500 error or some template error. There is a good chance that you can find the root of the problem in the [logs](/logging). Stream the logs and see if you can find a hint.

If not, check the settings again. And of course, don't hesitate to ping us.



### Setup the Object Storage

When your App deals with uploads or any other kind of user generated files, you want to use the [Object Storage Component](object-storage). 

Remember: New Apps have [ephemeral storage](quirks#toc-ephemeral-storage), not persistent storage any more, they can still write on the local file storage, but it will get wiped. 

Offshore all those files. Once again, see your framework or CMS guides (here in the Help) on how to set this up. There are plugins and Composer packages to help you.

**Migrate files**: Once you have set that up and it's working, you possibly need to transfer the files that already have been uploaded. You can do so, by logging in to the Old App by SSH (or sFTP), download all files and upload them to the Object Storage of your App.

At this point, new uploads should be transferred and hot-linked from the Object Storage, old uploads should also be there.


## Other settings and services

Now you got the basics covered, go into the details and see if you might forgot some settings. Maybe you need an additional firewall white-listing, or you need to install New Relic for the New App, or integrate another 3rd party service?


## Check everything

Test all functions of your web application on the New App once again, just to be sure.


## Get ready for the switch

Now that you have your New App and your Old App both running perfectly, check the TTL of your top level domain(s) and set them down, if needed. You do so with your domain provider of your TLD.


## Switch

Depending on how business critical your App is and how much data it generates, you might prepare/switch to maintenance mode (part of your CMS / framework / code). You might also import/export the database again to have the latest contents available.

You now delete the customs domains from the Old App (in the Dashboard) and then immediately set up those domains with the New App accordingly. 

If you use the [TLS Component](tls), now also copy and save the Certs and book and configure a new TLS with the same Certs on the New App.

In the last step you [route the domains(s)](about-domains#toc-route-a-custom-domain) to the New App. With your domain provider you remove the old settings and point the domain (with www prefix) via CNAME to your App URL.



## Done

Your App has been transferred.

