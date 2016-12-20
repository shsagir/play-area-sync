---

template:      article
reviewed:      2016-12-20
title:         Learn about your Apps directory structure
naviTitle:     Directory structure
group:         platform
stack:         all
oldLink:       directories-old
hideExamples:  yes

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
    {{app-name}}
      htdocs      < default root path
```


When you login with [SFTP](/sftp-uni) or [SSH](ssh-uni) to your [Universal App](app-uni) you can travel up in the file directory structure. In this article you can learn the predefined set of folders for each App and what they are for.

### htdocs

The default web root (aka document root) of your App. Directory that forms the main directory tree visible from the web. You can change the [routing point](domains#toc-root-path), to any folder below the `htdocs` directory. The [Git deployment](git) syncs to the `htdocs` folder as well.

The `htdocs` folder is also your "login folder", i.e. the folder you are in when logging in via SSH/SFTP.

### tmp

Temporary folder; limited to 2GB of storage. Files older than 15 days will be automatically purged. Typical use cases are the default PHP session file folder or a temp destination for file uploads via PHP (before `move_uploaded_file()` is called).

For multi node plans: The `tmp` folder is not shared. Meaning that each node has "their own" temporary folder.

## Other folders

The rest of the folders (and files which are not shown here) are part of the standard Linux distribution. All this stuff is handled by us for you. So you don't need to care about them. You can't change things outside the above outlined context.
