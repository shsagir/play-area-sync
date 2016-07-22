---

template:    article
reviewed:    2016-07-22
title:       Using secure App secrets
naviTitle:   App secrets
lead:        App secrets provide a secure storage for all the credentials your App needs to run.
group:       deployment

keywords:
    - Secrets
    - Credentials
    - ENV vars
    - Environment variables

seeAlsoLinks:
   - env-vars

tags:
  - php
  - beginner

---

## Problem

Your App needs confidential credentials to connect to other services (user-names, passwords, API keys or alike). Those credentials can be on fortrabbit like MySQL database access, or externally like the Amazon S3 cloud storage API key.

You [should not store them within your code base](//blog.fortrabbit.com/how-to-keep-a-secret#not-in-git), since it's controlled by Git - so version controlled. You [should not store them unencrypted in ENV vars](//blog.fortrabbit.com/how-to-keep-a-secret#not-in-an-env-var-either-) either.

In addition: your App will run in at least two environments: locally and on fortrabbit. Any solution must address the problems resulting from multiple runtime environments.

## Solution

Use fortrabbits App secrets to store your credentials safely. App secrets are stored in a JSON file called `secrets.json` which is only accessible by your App. The location of this JSON file is stored in a predefined environment variable called `APP_SECRETS`.

## App secrets access

### Using App secrets in PHP with your App

```php
// read all App secrets from the JSON file, get the location via ENV var
$secrets = json_decode(file_get_contents($_SERVER["APP_SECRETS"]), true);

// use a specific secret
$meaning_of_life = $secrets['CUSTOM']['MEANING_OF_LIFE'];
```

Secrets are ordered in a tree structure `["CONTEXT" => ["KEY" => "value"]]`:

```php
$secrets == [
    'MYSQL' => [
        'PASSWORD' => "{{mysql-password}}",
        'USER'     => "{{mysql-user}}",
        'DATABASE' => "{{mysql-database}}",
        'HOST'     => "{{mysql-host}}",
        'PORT'     => "{{mysql-port}}",
    ],
    'CUSTOM' => [
        'YOUR_CUSTOM_SECRET'    => "{{YOUR_CUSTOM_SECRET}}",
        'yourOtherCustomSecret' => "{{yourOtherCustomSecret}}"
    ]
];
```

Available contexts depend on the Components you have enabled for your App. 


### Reading secrets from outside

Read the secrets from your local machine by using a terminal command:

```bash
# show all App secrets
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets
# {
#     "MYSQL": {
#         "PASSWORD": "{{mysql-password}}",
#         "USER": "{{app-name}}",
#         "DATABASE": "{{app-name}}",
#         "HOST": "{{app-name}}.mysql.{{region}}.frbit.com",
#         "PORT": "3306",
#     },
#     "CUSTOM": {
#         "YOUR_CUSTOM_SECRET": "The custom content",
#         "yourOtherCustomSecret": "The custom content"
#     }
# }

# show only MySQL secrets
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL
# {
#     "PASSWORD": "{{mysql-password}}",
#     "USER": "{{app-name}}",
#     "DATABASE": "{{app-name}}",
#     "HOST": "{{app-name}}.mysql.{{region}}.frbit.com",
#     "PORT": "3306",
# }

# show only MySQL password
$ ssh {{ssh-user}}@deploy.{{region}}.frbit.com secrets MYSQL.PASSWORD
# {{mysql-password}}
```


## Adding custom App secrets

You can add or remove custom App secrets in the [Dashboard](dashboard). You'll do so in the settings of your [App](app). The contents of the App secrets cannot be viewed in the Dashboard due to the underlying encryption, which we consider a feature, not a bug.

<div markdown="1" data-user="known">

[See App secrets of your App **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/secrets)
[Add new App secrets for your App **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/secrets/new)

</div>


## App secrets vs ENV vars

App secrets are closely related to ENV vars insofar that they are both available to your App at runtime. The big difference between them is that App secrets are stored highly secured and they are not automatically dumped out by debug tools - such as `phpinfo()` or your favorite debug toolbar.

To achieve a level of security approaching the App secrets with ENV vars you can encrypt them, as described in [the ENV var article](env-vars#toc-env-vars-vs-security).

## App secrets vs local environment

Since access to your App secrets should be done using `$_SERVER['APP_SECRETS']`, you can easily set this environment variable locally with a path to a local JSON file containing your local (dummy) secrets.
