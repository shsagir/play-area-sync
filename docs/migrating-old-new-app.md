---

template:      article
reviewed:      2016-05-16
title:         Migrating from Old App to New App
naviTitle:     Migrate: Old App > New App
lead:          Learn how to move your Old App to a New App.
group:         platform
stack:         old

---

OK, you might have a look what [changed with the New Apps](new-apps) first to get a high level perspective.

This guide is a skeleton to help you migrating from [Old Apps](old-apps) to New Apps step by step. When finished, you will have your Old App and your New App running side by side. When the New App works perfectly, you can then remove all domains from the Old App, add them to the New App and delete the Old App.


## Motivational intro

Yes: it takes some efforts from your side. As much as we would like to, we can not automate it. On the other side your Apps will become less expensive, future-proof and the performance will increase. It's also an opportunity for a spring cleaning and you'll likely learn some cool new tricks on the way. Thank you in advance for investing your precious time in a change with us.

**We are here to help!** Don't hesitate to ping us, when you hang somewhere or you simply have a question. Now let's go:


## Create a New App

Set up a new New App (in the [Dashboard](dashboard)). Same as with the Old Apps: your New App needs a unique name. Different than with the Old Apps: you can choose one of two locations: Europe or US.

If in doubt: go ahead and use the trial mode and ask us to extend your trial time, if you need longer. We are happy to do so, as we don't want to mitigate migration costs for you as far as possible.

## Prepare for the switch

Check the TTL of your domain(s) and make sure they are set to the lowest possible value, if they aren't already. You do so with your domain provider. This is very important to circumvent downtimes. If you reduced the TTL then make sure you continue the migration *after* the previous TTL elapsed: If it was 24h, then you need to wait 24h before everybody (=all DNS caches on the way) gets the new TTL.

If you are using TLS you should also enable the [TLS](/tls) component for the New App now and install your key and certificate(s). This won't have any impact on your Old App. Two Apps can have the same certificate installed without any problem.

## Get your New App up and running

The Old App should stay untouched until New App is up and running.

**Copy Dashboard settings**: Check your Old Apps settings within the Dashboard and match your New App to those. Choose the same [root path](app#toc-set-a-custom-root-path) as you have with the Old App. Enable the same PHP extensions. Consider using PHP 7 from now on — it's a lot faster and we recommend to give it at least a try. If you were using PHP 5.5 before then 5.6 will very likely work without any problems. Also do not forget to copy any custom environment variables you might have.


### Setup Git

The first step is to add the New Apps Git remote as a new remote to your local Git repo. This will leave you with (at least) two remotes: one for the Old App, one for the New App. We also advice to create a new local branch which then contains all code changes you might need to undertake. For the purpose of this guide we will refer to it as the `new-app` Branch (alternatively you can also stay on `master` and backup the Old App in another branch). This way you can still work with the Old App during the migration, if you need to. Remember to always push to the master branch on fortrabbit.

The Git branch setup will then look something like the following:

```
local: master  -> fortrabbit remote: Old App / master
local: new-app -> fortrabbit remote: New App / master
```

It's up to your preference, you can also work with environment detection to differentiate between New and Old App. See our [multi-staging article](multi-staging) to get the idea.


### Deploy code to the New App

With the New App you can now `git push` to deploy your code base to remote. Composer packages will be downloaded during the first run, which takes longer. From then on it will be much faster than the Old App deployment. Please also see the [deployment article](/git-deployment) for details on post deploy scripts and suchlike.

After your first push you will be able to see your New App in the browser immediately — just visit the App URL of the New App. Most likely it will throw an error, because configuration needs to be adjusted. Don't worry.


### Update your configuration files

Right now, your code base probably contains MySQL credentials, maybe Memcache access information and so on. The handling of passwords and such has changed with New Apps: All your credentials are now stored in the [App Secrets](/secrets). You can access them using an [SSH command](/secrets#toc-accessing-app-secrets) and of course programatically from PHP:

```
if (isset($_SERVER['APP_SECRETS'])) {
    $secrets = json_decode(file_get_contents($_SERVER['APP_SECRETS']), true);
    // do somehting
}
```

Please check out our [install guide](/#install-guides), which give you framework/CMS specific guidance.

### Transfer & connect the MySQL database

Only applies if your App makes use of a [MySQL database](mysql), of course:

**Export the database**: Connect to your Old Apps database using the SSH tunnel – [see here](/mysql-old-app#toc-remote-mysql-access). To dump the database you can either use the `mysqldump` console client from your local machine or download it with a MySQL GUI.

**Import the database**: Connect to your New Apps database — [see here](/mysql#toc-remote-mysql-access). Import the previously exported dump. Again: Either with the `mysql` console client or with a MySQL GUI. Mind that you can get your credentials using the SSH command mentioned above.

### Give it a try

After modifying the configuration and possibly migrating your MySQL database you should push the changes to the App and see if you are done. Chances are good.

### Debugging errors

Don't panic when it still throws you an error now. This can have multiple causes and it's probably an individual problem. You might see a white screen, or a 500 error or some template error. There is a good chance that you can find the root of the problem in the [logs](/logging). Stream the logs and see if you can find a hint. If you don't see any error output, then check back with our [install guide](/#install-guides) again and find out how to enable logging in your specific CMS or framework. And of course, don't hesitate to ping us.


### Setup the Object Storage

When your App deals with uploads or any other kind of user generated files, you want to use the [Object Storage Component](/object-storage).

Remember: New Apps have [ephemeral storage](/quirks#toc-ephemeral-storage), not persistent. This means: they can still write on the local file storage, but any changes will be wiped upon deployment. So you need to offshore all files which are not part of your code base (or generated during deployment). Once again, see your framework or install guides on how to set this up with your CMS/framwork.

**Migrate files**: Once you have set that up and it's working, you possibly need to transfer the files that already have been uploaded to your Old App. You can do so by logging in to the Old App via SSH/SFTP, download all files locally and upload them to the Object Storage of your App.

**File locations**: Files stored on the Object Storage have a different URL. If they were beforehand available at http://www.yourdomain.tld/users/1234.jpg then they will be now under https://your-app.objects.frb.io/users/12345.jpg (or wherever you copied them).


## Other settings and services

Now you got the basics covered, go into the details and see if you might forgot some settings. Maybe you need an additional firewall white-listing, or you need to install New Relic for the New App, or integrate another 3rd party service?


## Check everything

Test all functions of your web application on the New App once again, just to be sure.


## Get ready for the switch

Double check your domain TTL settings with your domain provider. Consider adding `.htaccess` RewriteRules to your Old App, which delegates incoming requests to your new App (mind that those should redirect to the App URL, not the domain, because all requests which still hit the Old App will do so because the visitor's DNS cache still thinks the domain routes to the old App due to TTL).


## Switch

Depending on how business critical your App is and how much data it generates, you might prepare/switch to maintenance mode (part of your CMS / framework / code). You might also import/export the database again to have the latest contents available.

Now delete all customs domains from the Old App in the Dashboard and then immediately add them to your New App accordingly. Mind to use the same document roots.

In the last step you [route the domains(s)](about-domains#toc-route-a-custom-domain) to the New App with your Domain Provider.


## Done

Your App has been transferred. If everything worked out you can now delete your Old App. Mind that we keep backups for seven days in case of emergency (please contact us asap, in case you need them).
