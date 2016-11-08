---

template:      article
reviewed:      2016-11-08
title:         Directory structure
naviTitle:     Directory structure
group:         platform
stack:         pro
uniLink:       directories-uni
oldLink:       directories-old


keywords:
    - create directory
    - create
    - folder
    - directory
    - directories
    - linux
    - unix
    - web root
    - doc root
    - document root


---


```nohighlight
bin
dev
etc
lib64
proc
tmp               < 2GB temporaray files
usr
srv
  app
    appname
      htdocs      < default root path
```


<!-- TODO: re-unite with the other more up-to-date directories article for universal App  -->

Professional stack Apps only have [Git deployment](git-deployment) and [SSH remote execution](remote-ssh-execution-pro). So you will actually not be able to 'see' those folders. But it might be good to know  the predefined set of folders for each App:

### appname

The actual name of your App, for example `/srv/app/{{app-name}}` and so on.

### htdocs

The default web root (aka document root) of your App. Directory that forms the main directory tree visible from the web. You can change the [routing point](domains#toc-set-a-custom-root-path), to any folder below the `htdocs` directory. The [Git deployment](git) syncs to the `htdocs` folder as well.

### tmp

Temporary folder; limited to 2GB of storage. Files older than 15 days will be automatically purged. Typical use cases are the default PHP session file folder or a temp destination for file uploads via PHP (before `move_uploaded_file()` is called).

For multi Node plans: The `tmp` folder is not shared. Meaning that each node has "their own" temporary folder.

## Other folders

The rest of the folders (and files which are not shown here) are part of the standard Linux distribution. All this stuff is handled by us for you. So you don't have access privileges. You can't change things outside the above outlined context.
