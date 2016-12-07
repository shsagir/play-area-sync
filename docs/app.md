---

template:      article
reviewed:      2016-12-01
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

* [Set up a new domain for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/domains/new/name)

</div>


#### Root path

<!-- TODO: check if this correct according to latest specs for the whole root path thing -->
<!-- TODO: is TILDE the way you enter this in the Dashboard? I guess not, maybe remove TILDE to make this more obvious here -->

Per default all the [domains](/domains) of the App will route to the same root path. When you have chosen [Laravel](/install-laravel) when creating the App `~/htdocs/public` will be used.

When no framework or CMS has been chosen while creating the App, the root path [folder](directories) is `~/htdocs`. You can change this setting with your App in the Dashboard.

<!-- TODO: check link, just guessed the URL -->

<div markdown="1" data-user="known">

* [Change the root path for your App {{app-name}}](https://dashboard.fortrabbit.com/apps/{{app-name}}/root-path)

</div>

##### Different root paths for different domains

In some cases, you might want to route different domains to different folders. Please mind that there are limited use cases for this and don't use one App to host multiple website (see [here](/app#toc-one-website-per-app) as well).

When visiting the root path settings form with a [Professional stack Apps](/app-pro) you will find a switch to change from a global path for all domains to custom root paths for each domain.

<!-- TODO: write something about HTACCESS solution to rewrite URLS for everyone. maybe just a hint without example.  -->

## Further readings

* [Quirks and constrains](/quirks)
* [App Specs](https://www.fortrabbit.com/specs)
* [About the Stacks](/stacks)
