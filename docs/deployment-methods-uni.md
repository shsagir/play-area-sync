---

template:      article
naviTitle:     "Deployment methods"
title:         "Deployment methods"
reviewed:      2016-11-11
excerpt:       "How Git, SSH & SFTP work side by side."
group:         deployment
stack:         uni

---

[Universal Apps](app-uni) offer three methods to deploy and access code:

3. [SFTP](/sftp-uni) — legacy upload style
1. [Git](/git-deployment) — advanced: push to deploy
2. [SSH](/ssh-uni) — advanced tasks and deployment

The different deployment methods work side by side. This articles explains how they interact, what you need to know when mixing methods and give tips for best practices.

## Understanding the architecture

```nohighlight
# Universal App deployment architecture
                                                         ┌───────────────┐ 
                                                         │ website users │ 
                                                         └──────┬────────┘ 
                                                                │          
                                                          http requests    
                                                                │          
                            ┌───────────────────────────────────┼─────────┐
                            │fortrabbit App                     │         │
                            │                                   │         │
┌────────────────┐          │┌─────────────────┐         ┌──────▼────────┐│
│ local Git repo ├─Git push─┼▶ remote Git repo ├──rsync──▶  web storage  ││
└────────────────┘          │└─────────────────┘         └──────▲────────┘│
                            │                                   │         │
                            │                                   │         │
                            └───────────────────────────────────┼─────────┘
                                                                │          
                                                                │          
                                                            SFTP/SSH       
                                                                │          
                                                          ┌─────┴─────┐    
                                                          │ developer │    
                                                          └───────────┘    
```

### What the web storage contains

Please keep in mind, that the Git repo is not the web storage. After you Git push, first the Git repo will be updated, then changes will be synchronized (overwrite but not delete) to the web storage. So the web storage contains: 

1. The latest changes from Git
2. Changes from SFTP & SSH
3. Uploads from website users

### Git works only one way

You can not `pull` the changes from the web storage back into your Git repo. While this might looks odd at first: this design keeps your Git repo clean of temporary, binary and other blob. Use Git only for code deployment, not to manage all of your Apps runtime data.

### Not all applications work well with Git

Git deployment is great when your App skeleton has clean folder structure with exclude patterns and [Composer](/composer) support. [Laravel](/install-laravel) and [Symfony](/install-symfony) are poster childs for good Git support. [WordPress](/install-wordpress) and other CMS are not Git compatible, out-of-the-box (while good [hacks](install-wordpress-pro) are available).







