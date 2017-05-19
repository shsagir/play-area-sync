---

template:      article
reviewed:      2017-05-19
naviTitle:     About Apps
title:         What is an App?
lead:          Forget servers. Think services instead. Learn the basic fortrabbit concepts.
group:         platform
stack:         all

---

## Understanding the platform

We claim that the fortrabbit hosting platform is different to classical hosting. Our comparsion pages give you a good picture what fortrabbit is about:

* [How fortrabbit is different than VPS hosting](https://www.fortrabbit.com/why-not-vps)
* [How IaaS is being used](https://www.fortrabbit.com/why-not-aws)
* [See the fortrabbit terminology](/terminology)



## The App Concept

```nohighlight
# simplified App topology
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃App                                                 ┃
┃  ┏━━━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━┓ ┏━━━━━━━━━━━━━┓ ┃
┃  ┃               ┃ ┃Webserver    ┃ ┃Database     ┃ ┃
┃  ┃               ┣─┫& PHP        ┣─┫cluster      ┃ ┃
┃  ┃               ┃ ┃             ┃ ┃             ┃ ┃
┃  ┃Load Balancer  ┃ ┗━━━━━━┳━━━━━━┛ ┗━━━━━━┳━━━━━━┛ ┃
┃  ┃               ┃ ┏━━━━━━┻━━━━━━┓ ┏━━━━━━┻━━━━━━┓ ┃
┃  ┃               ┃ ┃Webserver    ┃ ┃Database     ┃ ┃
┃  ┃               ┣─┫& PHP        ┣─┫cluster      ┃ ┃
┃  ┃               ┃ ┃             ┃ ┃             ┃ ┃
┃  ┗━━━━━━━━━━━━━━━┛ ┗━━━━━━━━━━━━━┛ ┗━━━━━━━━━━━━━┛ ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```

An App is a virtual container for your web project, website, web application, staging branch, project or whatever you do.



### One website per App

With classical server hosting you rent out a machine to host multiple websites. While this seems to make economic sense — it's not smart. Just think about these scenarios:

* "website A" needs runtime version y, while "website B" requires an version x
* "website C" has a nasty bug and crashes the whole server
* "website D" has a security vulnerability, the whole server get's hacked
* "website E" get's popular and receives lot's of traffic, slowing other websites down
* Your fellow web developer colleague "accidentally" removed all files on the server

Apps are better:

* Apps start a friction of a server but can be scaled across multiple servers
* Each App has it's own settings
* Apps are isolated from each other


So, a fortrabbit App is designed to host one website:

* There is only one MySQL database per App
* Each App has only one Git repo
* Each App has it's own collaboration rules
* Each App has it's own perfomance metrics




## Dashboard links

* [Go to your list of Apps](https://dashboard.fortrabbit.com/apps)
* [Go to your App: {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}})



## Settings

Each App comes with a set of basic features and [limits](https://www.fortrabbit.com/specs).

* a dedicated Git repo
* a unique [App URL](#toc-app-url)
* custom metrics
* included monthly traffic
* various settings in the Dashboard
* [collaboration](collaboration) options

Most of it can be viewed and edited in the [Dashboard](/dashboard).

### Domains

There are various settings to control the routing of domains. Please also see the [domains help article](/domains).

#### App URL

While creating your fortrabbit App you will be asked for an App name. This name can't be changed later on and is used in many places as an identifier. The most prominent one is your default App URL which looks like this: `https://your-app.frb.io/`. This is where you can always reach your App thru the web browser. Use the App URL for development, testing and to connect to external services.

Please mind that you can not change the App name later on. 7 days after you have deleted an App, the same App name will become available to use again.

#### Set up a custom domain

You can register your App to accept requests from any external domain you route to fortrabbit — see also [the domain article](/domains). To set up a domain routing, you add a new custom domain within your Apps domain settings in the Dashboard.

<div markdown="1" data-user="known">

* [Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new)

</div>


#### Root path


Per default all the [domains](/domains) of the App will route to the same root path (sometimes this is also called: document root, docroot or root folder): `htdocs`. This path is, where the first `index.php` will be called, when people are visiting your App on any domain.

This path setting can vary, depending on what the framework or CMS you have selected in the software chooser when creating the App:

| Framework/CMS                 | Root path     |
| ----------------------------- | ------------- |
| Laravel, Phalcon, Craft CMS   | htdocs/public |
| Symfony                       | htdocs/web    |
| Drupal 8, WordPress, Grav     | htdocs        |

You can however set the root path afterwards at any given time by visiting the according setting in the Dashboard under your App. 

<div markdown="1" data-user="known">

* [Change the root path for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/rootpath)

</div>


##### Individual root paths per domain

In some cases, you might want to route individual domains to different folders. Please mind that there are limited use cases for this and don't use one App to host multiple website (see [here](/app#toc-one-website-per-app) as well).

When visiting the root path settings form with a [Professional stack Apps](/app-pro) you will find a switch to change from a global root path for all domains to custom root paths for each domain.

Mind that you can also use `.htaccess` files with `RewriteRule` directives to handle different domains differently.



#### Metrics

<!--

TODO: extend this topic, it's a major selling point. explain how to use it with use cases bla bla bla.

-->

Each App comes with "**Usage metrics**": Those show you the current status of the Web storage, MySQL storage and Object Storage and context of your 

Some Apps also come with "**Performance metrics**": Those show you how fast your App is and where you might need to improve. Perfomance metrics are: requests, PHP response time, traffic, errors and other useful stuff. You can set different time intervals to monitor perfromance over time.




## Trial

You can test fortrabbit for free. Therefore each Account can have one trial App running. The purpose of the trial is testdrive the platform. See if it works as advertised. See if fortrabbit is the right hosting solution for you.


#### Starting a new trial App

Find the "Create an App" on the Dashboard home. When asked for the plan, you can choose the free trial option instead. Trial Apps are available for both stacks, Universal & Professional.


#### Trial limits

The trial App scaling specs are matching a small preset, always enough to get started. The trial is NOT a forever-free tier. An urgent limitation is, that the trial has a limited lifetime:


#### Extending the trial

The default trial time is short. A timer in the Dashboard with your App will show you the time left. You can extend the evaluation period by completing some simple tasks like: 

* Confirming your Accounts e-mail address
* Setting your real name under your Account
* Deploying some code to the trial App
* Routing a domain to the trial App

A task that is completed has a checkmark `✔`. Tasks that are still todo will show how much time you will gain `+24h`.

Additionally, after some trial time has passed, we show a link to a contact form where you can ask us to extend the trial time. Here you'll write us something about your goals with the trial and we are happy to extend the trial for much longer period. You can see those tasks in the Dashboard with the overview of your App.

#### Upgrading the trial

You can, of course, upgrade from your trial to a paid plan at any time. You can do so under your Apps overview. 


#### The end of a trial

When the trial is finally over, the trial App will be deleted — we will not keep your data hostage. Good news is: we will inform you by e-mail before it happens and you can start a new trial right away, after your current trial ended.


## Further readings

* [Quirks and constrains](/quirks)
* [App Specs](https://www.fortrabbit.com/specs)
* [About the Stacks](/stacks)
