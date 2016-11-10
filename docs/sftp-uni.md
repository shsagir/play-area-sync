---

template:      article
reviewed:      2016-11-07
title:         SFTP
naviTitle:     SFTP
lead:          Learn about the classical way to deploy and access your App on fortrabbit.

stack:         uni
oldLink:       ssh-sftp-old
group:         deployment

keywords:
    - beginner
    - deployment
    - dreamweaver
    - 

---

Albeit [deploying with Git](git-deployment) has many advantages, sometimes it's more of a burden then it helps. Especially when working with CMS. These systems are often not designed for Git-workflows and write on the file system when installing plugins and upgrading versions. Those changes can not be back-ported to the version control. Stuff get's out of control.

All Universal Stack Apps come with SFTP access out-of-the-box to legacy applications and workflows.


## Accessing SFTP

There are various GUIs out there, which make your life easier. We recommend [Cyberduck](https://cyberduck.io/) (Mac, Windows).

<!-- TODO: Describe configuration of Cyberduck connection with example -->

Many modern editors or IDEs also feature SFTP integrations by plugin.

* **Mode**: SFTP (not regular FTP)
* **Host**: deploy.{{region}}.frbit.com
* **Port**: 22
* **Username**: {{ssh-user}}
* **Password**: Your Account password OR public SSH key


## File synchronization

Most SFTP clients feature a file synchronization mode. You can choose your local folder and sync it to the remote folder on fortrabbit. All files will be compared and only changed ones will be uploaded. This works in the other direction as well, of course.

<!-- TODO: for Frank, describe file exclution patterns maybe example with Transmit -->

## Troubleshooting authentication

Got an error when trying to login? fortrabbit supports username + password and public key authentication. Please continue here to troubleshoot access:

* [See the access methods article](/access-methods)


## About SFTP

SFTP stands for SSH File Transfer Protocol. It's a separate protocol packaged with [SSH]([SSH](ssh-uni)) — think of it as the little sister of SSH. SFTP is very different than FTP or FTPS but all clients will speak it anyways, so for the usage it doesn't really makes a difference.

