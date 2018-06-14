---

template:         article
reviewed:         2018-06-14
title:            Manage Craft assets
naviTitle:        Manage Craft assets
lead:             Learn how to deploy Craft CMS runtime data to the Object Storage with fortrabbit Professional Apps.
group:            craft
stack:            pro
workInProgress:   yes

websiteLink:      https://craftcms.com/
websiteLinkText:  craftcms.com
category:         CMS
image:            craft-cms-logo.png
version:          3.0.9
uniLink:          craft-3-assets-uni

otherVersions:
    2.6 : install-craft-2-pro

keywords:
  - craft
  - craftCMS
  - setup
  - install-guide

---

## Get ready

For best results here, make sure you have completed all steps from the [get ready guide](/craft-3-about) and have [Craft running on your local machine](/craft-3-install-local), [setup](/craft-3-setup) and [deployed](/craft-3-deploy-git). This guide is for advanced users on the advanced [Pro Stack](/app-pro).

## About Craft CMS assets

With Craft 3, the "assets" folder contains files that are managed by the CMS. This is the user generated stuff, uploaded files, mostly images — also see [the official Craft docs](https://docs.craftcms.com/v3/assets.html) on that. 

The [fortrabbit Craft CMS starter .gitignore](https://raw.githubusercontent.com/fortrabbit/craft-starter/master/.gitignore) file excludes `/web/assets/*` from Git. Why? Because, code and content are separated and the assets uploaded to an Pro App will get destroyed the next you time deploy anyways, see [here](/app-pro#ephemeral-storage) for more and why.

### Upload assets to the Object Storage

So what you want: is to swap the assets folder on the file system with external files stored on the fortrabbit [Object Storage](/object-storage). We have developed a special Craft plugin to connect the fortrabbit Craft App with the Object Storage. All uploads will transferred to the Object Storage directly, all URLs will point to the Object Storage. See the install guide on GitHub for usage:

* [github.com/fortrabbit/craft-object-storage](https://github.com/fortrabbit/craft-object-storage)

That will make the App use the Object Storage. To access the Object Storage yourself, you can use any S3 compatible SFTP client — most of them speak the S3 protocol.

### Craft asset bundler

<!-- TODO: Check this, not sure how it integrates with the Object Storage? -->

One reason to choose our Professional Stack is the option to have a horizontally scaled setup AKA high availability. But there are still some [problems with Craft 3 itself](https://github.com/craftcms/cms/issues/2500) in such cases currently — fix hopefully soon. For now we have published a plugin as a workaround:

* [github.com/fortrabbit/craft-asset-bundler](https://github.com/fortrabbit/craft-asset-bundler)

## Next steps

Continue with [tuning Craft](/craft-3-tune).