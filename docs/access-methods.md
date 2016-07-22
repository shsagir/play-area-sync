---

template:      article
reviewed:      2016-07-22
naviTitle:     Access methods
title:         How to access fortrabbit services
lead:          Learn about the different authentication methods with fortrabbit.
group:         deployment

otherVersionLinks:
    - code-access-old-app

keywords:
     - ssh key
     - git
     - git-deployment
     - ssh
     - username
     - password
     - access
     - authentication

tags:
     - git

seeAlsoLinks:
    - git
    - git-deployment
    - ssh-keys
    - deployment-architecture-video

---

So far so good: You are using your Account e-mail address and your Account password to login to the fortrabbit [Dashboard](/dashboard). Beside that, there are these interaction channels with fortrabbit services:

* Accessing the [MySQL database](/mysql#toc-remote-mysql-access)
* Accessing the [Object Storage](/object-storage#toc-accessing-the-object-storage)
* Deploying code with [Git](/git-deployment#toc-usage)
* Execution [SSH Remote commands](/remote-ssh-execution#toc-usage)
* Viewing [log files](/logging)

Of course these operations need to be protected. We need to make sure that only you and your team have access. You can choose between two authentication methods: "[password](#toc-password-authentication)" and "[public SSH key](#toc-ssh-key-authentication)".



## How to edit your access method

In the "[Dashboard](/dashboard)", go to "Your Account" (upper right). Under "Access method" you can find your current settings. You see which method is currently set.




## SSH key authentication

We recommend to use public SSH key authentication to identify yourself with fortrabbit services. It's more secure than password authentication and also more convenient, once you have set it up. SSH key authentication is commonly used with services like BitBucket and GitHub.



### How to add and remove SSH keys with your Account

In the "Dashboard" > "Your Account" > "Access method" you can "Add a new SSH key". The **[direct link](https://dashboard.fortrabbit.com/account/keys/new)** can also take you there (login and re-authentication maybe required).


### How to create a public SSH key locally

We have a [dedicated article](ssh-keys) on setting up and troubleshooting your local SSH keys.



### GitHub SSH key import

**Automatic import**: When signing up to fortrabbit, we'll check at GitHub if there are any public SSH keys associated with your e-mail. If we find any, we'll import them and install them with your account. This is a one time setup, your SSH keys will not be synced. You can manage the SSH keys like any other keys then.

**Manual import**: You can also tell the import helper your GitHub account, when using different e-mail addresses here and on GitHub. There is a small link which will take you 

The [direct Dashboard link](https://dashboard.fortrabbit.com/boarding/keys/github) (login and re-authentication maybe required) will take you there directly.


### App-only SSH keys

In certain cases you might want to add code access to an App without the need to register a new Account with fortrabbit. One case is a hectic ad-hoc hotfix scenario (good luck!), another case is that you have some advanced deployment with a third party continuous integration service bot going on. So you can install additional App-only custom public SSH keys with each App. You manage those App-only SSH keys in the Dashboard with your App.


## Password authentication

This is the default method when no public SSH keys are installed. Use this, when you just want to check out fortrabbit or when you have trouble setting up your SSH keys locally. **Hobbyists are using Password authentication. Professionals are using SSH key authentication.**

### How to change from password to SSH key authentication

In the "Dashboard" > "Your Account" > "Access method" you can add an SSH key. Once you have added your first public SSH key, password authentication will be disabled and SSH key authentication will be enabled.

* **[Add an public SSH key](https://dashboard.fortrabbit.com/account/keys/new)** (login required)


### How to change from SSH key to password authentication

When for some reason SSH key authentication does not work for you, you can downgrade to password like so: In the "Dashboard" > "Your Account" > "Access method" you can click on your public SSH keys, this will bring up a view where you can delete the key. When deleting the last key, password authentication will be re-enabled.


### When you change your Account password

When you change your Dashboard password, for instance when you [regain access to the Dashboard](/dashboard#toc-regaining-access) in case of a forgotten password, all access to the services will change as well – the new password will be used.

- - -


## How it works

### Access schema

URLs and terminal commands depend on your chosen access method. 

```
# Git clone example
$ git clone [[ssh-user]]@deploy.[[region]].frbit.com:[[your-app]].git
```

* [[your-app]] is the name of your App (see Dashboard)
* [[region]] is `eu2` or `us1`, depending on the location of your App


#### With SSH key authentication

* No need to enter passwords, your public key will be used
* [[ssh-user]] will be: [[your-app]]

#### With password authentication

* You will need to enter your fortrabbit Account password (each time)
* [[ssh-user]] will be: [[your-app]].[[long-user-string]]


#### The code example helper

When you are logged in to the Dashboard, you will see a yellow box on the right side with a select to choose one of your Apps. This helper knows which authentication method your Account uses. It also changes all code examples on the current page according to the selected App. So you can literary copy/paste all code examples here.

##### Try it out yourself

```
# change the values on the right and see the below change
SSH user: {{ssh-user}}
Region:   {{region}}
Your app: {{app-name}}
```



### Authentication in teams

You manage your access method with your user Account on fortrabbit. This way you always have up-to-date code access on each App you own and collaborate on. It also makes managing the team easy — add/remove collaborators and code access is handled "automagically". Please mind that your team members might have a different acess method and that your settings might not work for them.

- - -

## Troubleshooting

### Authenticity error

The first time you are connecting to fortrabbit service via SSH, you might get an error and a prompt like this:

```
The authenticity of host '…' can't be established
RSA key fingerprint is … 
Are you sure you want to continue connecting (yes/no)?
```

Please answer with `yes`. This will add our servers to the known hosts. That should be a one time setup. If you are being asked this every time you deploy, something with your `known_hosts` file is probably wrong. See [this Stack Overflow question](http://stackoverflow.com/questions/9299651/git-says-warning-permanently-added-to-the-list-of-known-hosts).


### General connection errors

If you can't establish a connection at all, a corporate or personal firewall might be the cause. This can a protection software on your computer or something in your company. You can fix this by allowing communication on the SSH standard port: `22`.


### Authentication errors

Your local machine is maybe trying to use a different authentication method. Please check and correct your SSH configuration like so:

```
# terminal command to edit your ssh config file
nano ~/.ssh/config
```

Within this file change the preferred authentications accordingly:

```
# when using password authentication use:
PreferredAuthentications    password,publickey

# when using SSH key authentication use:
PreferredAuthentications    publickey,password
```





### Problems with SSH keys

Please see our [SSH key trouble shooting guides](/ssh-keys#troubleshooting) to resolve common problems with SSH key authentication.