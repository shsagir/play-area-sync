---

template:      article
reviewed:      2016-09-16
naviTitle:     About Apps
title:         What is an App?
lead:          Forget servers. Think servers instead. Learn the basic fortrabbit concepts.
group:         platform
stack:         all

tags: 
    - beginner
    - platform
---

## Concept

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

There are various settings to control the routing of domains:

#### App URL

While creating your fortrabbit App you will be asked for an App name. This name can't be changed later on and is used in many places as an identifier. The most prominent one is your default App URL which looks like this: `https://your-app.frb.io/`. This is where you can always reach your App thru the web browser. Use the App URL for development, testing and to connect to external services.

#### Set up a custom domain

You can register your App to accept requests from any external domain you route to fortrabbit — see also [the domain article](/about-domains). To set up a domain routing, you add a new custom domain within your Apps domain settings in the Dashboard. 

<div markdown="1" class="asdad">

* [Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new/name)

</div>


#### Set a custom root path

Per default the domains of the App will route to the `~/htdocs` [folder](directory-structure). In some cases, you need to set a different document root or want to route different domains to different folder. [Laravel](/install-laravel), for examples, requires you to use `~/htdocs/public` per default. You can set a custom root path by writing the relative path to the sub-folder (all folders below the htdocs folder are allowed). 


- - -

## FAQ

### Can i change the App URL later on?

Sorry that is not possible as it as an identifier for many services.

### I want to reuse an App URL but the name unavailable

After 7 days you can reuse the App URL.



