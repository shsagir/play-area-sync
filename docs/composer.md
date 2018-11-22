---

template:      article
reviewed:      2018-05-07
title:         Leveraging Composer
naviTitle:     Composer
lead:          No need to explain this anymore: Composer is the defacto standard to handle PHP application dependencies, as well as providing mechanisms to keep them up-2-date. Learn how to integrate Composer into your development workflow with fortrabbit.
group:         deployment
stack:         all

keywords:
  - composerphp
  - git
  - getcomposer
  - Dependency Manager

---



## Use Git deployment with Composer

**TLDR;** Just  `git push` and you are good to go.

```
Local         fortrabbit deploy service           Your fortrabbit App
┌────────┐    ┌────────────┐  ┌────────────┐      ┌──────────────┐
│        │    │            │  │            │      │              │
│  Git   ├────▶  Git repo  ├──▶  Composer  ├──────▶   Webspace   │
│        │    │            │  │            │      │              │
└────────┘    └────────────┘  └────────────┘      └──────────────┘
```

The above drawing illustrates the fortrabbit architecture. When you deploy with Git, Composer will run **automatically** along within the deployment process on fortrabbit. 

The `vendor` folder should NOT be in Git > make sure that folder ins included in your `.gitignore` file. This directory is created by Composer within in your project locally and contains all the packages you are using locally. Make sure that both the `composer.lock` file and the `composer.json` file are present and part of Git.

The deployment build process executes `composer install` (not update) to make sure that exactly the packages your App requires are installed.

### Composer in the deployment file

You can fine tune your deployment behavior and aspects of Composer in the deployment file. See the options withunder the `composer:` keyword [here](deployment-file-v2).

#### Alternative locations

If your `composer.json` and `composer.lock` file are not on top level, then they will be ignored by the deployment. However, you can use the `pre` directive from the [deployment file](deployment-file) to setup a custom composer run in a different directory.

Following an example which assumes that your `composer.*` files are within the folder `sub-folder`:

**fortrabbit.yml**

```yaml
version: 2

# execute alternate composer run before anything
pre: ccomposer install -d sub-folder

# make sure the new vendor folder is sustained during deploys
sustained:
  - sub-folder/vendor
```

### Private repositories in Composer

You need to add the private repositories into your `composer.json` file? Read on [here](private-composer-repos).

## Composer from SSH

Universal Apps only: While we recommend to leverage Composer with the Git deployment (see above), you can also execute Composer when logged in via [SSH](ssh-uni). Composer is pre-installed. 

```bash
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com
$ composer install
```

This way Composer will be executed within your Apps web delivery environment, which is not optimized for such tasks. Please don't use Composer update as this might cause Composer to hit the Apps memory limits.


## Local Composer

It's kinda mandatory that you have Composer running in your [local development environment](/local-development) as well. See the [offical Composer install guides](https://getcomposer.org/download/) on how to download and install Composer locally.
