# fortrabbit help pages


Welcome to the source of the official fortrabbit documentation. These files here are written in Markdown, with a little frontmatter meta-data on top. We include this repo as a Git subtree and publish it on: http://help.fortrabbit.com

This repo lives under (clone URL):

* https://github.com/fortrabbit/help.git
* git@github.com:fortrabbit/help.git


## Contributing

Found a typo or an error? Do you want to add something about your framework or service of choice? You are more than welcome to contribute. Please send us a Pull Request. 

Do you run a 3rd party service or an open source project that can be integrated with fortrabbit? You are also more than welcome to add it. 


### _WIP

The folder _WIP contains — as you might can guess — work in progress. All infos in there are subject to major errors. Please contribute!


### Logos and brand assets

Place logos for external services and projects in the media folder. They ideally should be svg or either PNG with transparent background. Favor an "image mark" over a "word image mark". 



### Front Matter syntax

Each markdown file requires a yaml block at the top. See here which attributes are available, how and when to use them.

```yaml
# which template to use - "article", if in doubt
template: article

# title shown in navigation -> short but descriptive
naviTitle: About Apps

# title shown in article header on display
title: What is an App anyways?

# When the article was last checked? required, must be checked every year.
# Don't forget to edit this each time you have reviewed a page
# pages older than one year will be marked outdated
reviewed: 2016-02-22

# under which headline the content will be shown on home 
# (use underscore instead of space)
group: Extending_fortrabbit


# don't show in list on homepage
dontList: true

# do not include in search, don't show in search results
dontIndex: true

# The actual link you will send people to
websiteLink: https://www.abcdefg.com?utm_source=fortrabbit

# A human readable version of that same link, exclude clutter!
websiteLinkText: abcdefg.com

# set for latency relevant 3rd party services, otherwise use nor acceptable
dataCenters: n/a

# lead text for article detail view -> what to expect from content
lead: Forget servers. Think services instead. Learn the basic fortrabbit concepts.

# disable TOC generation for this article
noToc: true

# logo shown for 3rd party service or open source
image: myfunkyimage.png


# type, for CMS/framework
type:  CMS

# Other versions of this article (=filename without .md)
otherVersionLinks:
    - install-laravel-4
    - install-laravel-6
    - install-laravel-7

# names (=filename without .md) of linked articles related
seeAlsoLinks:
    - app
    - composer

# additional keywords for document search to help users find this article
keywords:
    - foo
    - bar

# tag article to filter
# (currently not in use)
tags: 
    - beginner
    - cms
```



## Writing help & blog articles

* Don't bullshit. Avoid the use of marketing buzzwords.
* Avoid "eg", otherwise choose one style to write it
* Keep lists readable (no sublists, etc).
* available headlines in text: h2, h3, h4, please don't go deeper.
* Text structure is important, but readability is even more important.
* Use ASCII art to illustrate topographies and such things.
* code blocks follow standard markdown formatting.
* Use sentence case in headlines.
* Don't use too many headlines to structure text.
* Don't use italic > it looks ugly, it's rendered by the browser not the font.
* Start code examples right away: PHP without "<?php", Bash without "$"
* Don't use a paragraph for every sentence.
* Write speaking links, not like so: `[here](http://somewhere)` but so: `[GitHub project](http…)`

## Maintainability

Find the right balance between being general and being precise (aka Captain Obvious). Very detailed step-by-step articles are easy to follow but get outdated very quickly. 


## fortrabbit owned words and common casing

* **App URL**
* **Admin** ~~admin~~
* **App** — ~~app~~, ~~website~~, ~~application~~
* **Account** — ~~profile~~, ~~account~~
* **Billing Contact** ~~bc~~
* **Activities** ~~history~~, ~~diary~~, ~~log~~
* **Dashboard** — ~~control panel~~, ~~console~~
* **Help** — ~~documentation~~, ~~handbook~~, ~~docs~~
* **Developer** ~~developer~~
* **Company** — ~~company~~
* **User** — ~~member~~, ~~account~~
* **Owner** ~~owner~~
* **Workers** — ~~Worker~~
* **fortrabbit**
* **fortrabbit.yml** — ~~deployment file~~
* **fortrabbit scheduler** — ~~scheduler~~
* **App Secrets** — ~~Secrets~~
* **HTTPS** — ~~https~~
* **HTTP** — ~~http~~
* **URL** — ~~Url~~, ~~url~~


## Times & dates

* use the same time-zone E V E R Y W H E R E when communicating times
* the communicated time-zone is UTC
* use relative times (an hour ago, a few seconds ago)
* Markup example: `<time datetime="2016-01-01 08:22:10 UTC" title="2016-01-01 08:22:10 UTC">A few minutes ago</time>`

* **ms** — ~~MS~~
* **s** - ~~scds~~
* **m** - ~~mins~~, ~~min~~
* **h** - ~~hrs~~

### Metric units

* **16 MB** — ~~mb~~, capital letters, a space between value and unit
* **3 GB** ~~3078 MB~~ < prefer a readable unit
* **0 B**
* **16 KB**
* 10k — ~~10,000~~
* times a value: ×, `&times`; ~~x~~


### Other words

* €23 < without space char
* $23 < without space char
* **add-on** — add-ons, ~~Add-on~~, ~~Add-On~~, ~~AddOn~~
* **client** — ~~customer~~, ~~user~~
* **Component** ~~plan~~
* **default domain**
* **e-mail** — ~~eMail~~, ~~email~~, ~~Email~~, ~~E-Mail~~
* **free trial** — ~~test flight~~, ~~test drive~~
* **host** — ~~server~~
* a space char belongs before the unit
* **MySQL** — ~~MySql~~
* **OPcache** — ~~OPCache~~ ~~Opcache~~
* **node** — ~~server~~
* **non-persistent storage** — ~~storageless~~
* **payment details** — ~~complete account~~, ~~payment credentials~~
* **PHP requests** — ~~requests~~
* **PHP response time** — ~~response time~~
* **PHP** — ~~Php~~
* **Scale** — ~~upgrade~~
* **session time**
* **SFTP**
* **SSH Key** — ~~pubkey~~, ~~public SSH key~~
* **SSH**
* **SSL cert** — ~~SSL certificate~~
* **web storage** — ~~webspace~~
* **Composer** — ~~composer~~


## Avoid words list

please do not use the following "bullshit" words:

* great
* awesome
* innovative
* game-changing
* empowering
* revolutionizing 


## Corporate identity

There is no fortrabbit logo as graphic file. To create the brand logo just type: "• fortrabbit" — bullet character, a normal space and then name of the company with a f. Use the [Georgia Typeface](http://en.wikipedia.org/wiki/Georgia_(typeface)) in bold and italic. Use lot's of whitespace around the logo, don't put other text nearby the logo. When using the company name within a paragraph of text, write "fortrabbit" with a f, even at the beginning of a sentence. Don't use the bullet or any other typeface here.