---

template:         article
reviewed:         2016-05-17
title:            Install Phalcon 2
naviTitle:        Phalcon
lead:             Looking for sPHPeed? Phalcon is a web framework delivered as C extension providing high performance and low resource consumption. Here you learn how to best getting started with Phalcon 2 on fortrabbit.
dontList:         true

group:            Install_guides

websiteLink:      https://phalconphp.com/en/?utm_source=fortrabbit
websiteLinkText:  phalconphp.com
category:         framework
image:            phalcon-mark.png
version:          2

otherVersionLinks:
    - install-slim-phalcon-3

keywords:
    - phalconphp
    - framework
    - php extension
    - c extension
    - hhvm


tags:
    - php
    - install


---


## Get ready

We assume you've already created an [App](app) with fortrabbit. You should also have a [PHP development environment](/local-development) running on your local machine.

### Set Phalcon root path

Phalcon uses `public` as doc root, you need change that in the fortrabbit Dashboard under your Apps Domains settings.

### Enable Phalcon extension

As Phalcon is a C Extension, you need to enable it in the Dashboard under your Apps PHP Extensions.

## Proceed as usual

See our [deployment article](deployment) to get the picture.