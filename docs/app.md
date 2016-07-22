---

template:      article
reviewed:      2016-07-13
naviTitle:     About Apps
title:         What is an App?
lead:          Forget servers. Think servers instead. Learn the basic fortrabbit concepts.
group:         getting_started

seeAlsoLinks:
    - dashboard
    - migrating
    - scaling
    - app-design
    - old-apps
    - new-apps
    - git-deployment
    - terminology

tags: 
    - beginner
    - platform

---

## General concept

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

An App is a virtual container for your web project, website, web application, staging branch, project or whatever you do. It consists of multiple Components like the PHP runtime or a MySQL database. 

Think of Components as micro services. Some Components are integral — meaning: you can't turn them off — while others are optional. Most Components are available in multiple expansion states (scaling plans). There are presets with common combination of Components.

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

There are various settings to control the routing of domains:

#### App URL

While creating your fortrabbit App you will be asked for an App name. This name can't be changed later on and is used in many places as an identifier. The most prominent one is your default App URL which looks like this: `https://your-app.frb.io/`. This is where you can always reach your App thru the web browser. Use the App URL for development, testing and to connect to external services.

#### Set up a custom domain

You can register your App to accept requests from any external domain you route to fortrabbit — see also [the domain article](/about-domains). To set up a domain routing, you add a new custom domain within your Apps domain settings in the Dashboard. 

<div markdown="1" class="asdad">

* [Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new/name)

</div>


#### Set a custom root path

Per default the domains of the App will route to the `~/htdocs` [folder](directory-structure). In some cases, you need to set a different document root or want to route different domains to different folder. [Laravel](/install-laravel), for examples, requires you to use `~/htdocs/public` per default.

You can set a custom root path by writing the relative path to the sub-folder (all folders below the htdocs folder are allowed). You can do so for each domain (including App URL) you have registered within the Dashboard.

For example, if you want to use the folder `~/htdocs/web`, just enter `web` in the input. If you want to use the folder `~/htdocs/app/webbroot`, just enter `app/webroot`.





## Old and New Apps

You can currently run two different generations of Apps on fortrabbit: classical [Old Apps](old-apps) and [New Apps](new-apps). Old Apps were introduced 2013 andwill end ~~at around mid 2016~~ start of 2017, until then all Old Apps have to be migrated to the new ones. New Apps have been introduced in August 2015. 

## Further readings

* [Official pricing page](http://www.fortrabbit.com/pricing) - overview about scaling plans & options
* [Universal specs page](http://www.fortrabbit.com/specs) - detailed informations
* [When and how to scale](/scaling) - help article about scaling
* [fortrabbit VS Digital Ocean](http://www.fortrabbit.com/why-not-digitalocean) - marketing article comparing fortrabbit with VPS hosting

- - -

## FAQ

### Can i change the App URL later on?

Sorry that is not possible as it as an identifier for many services.

### I want to reuse an App URL but the name unavailable

After 7 days you can reuse the App URL.



