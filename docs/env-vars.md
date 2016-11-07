---

template:   article
reviewed:   2016-09-25
title:      Using environment variables in PHP
naviTitle:  Environment variables
lead:       ENV vars help to create and shape the environment of where the code runs.
group:      platform
stack:      all

keywords:
    - ENV vars
    - Environment variables
    - php
    - beginner

---

## Problem

You probably run at least two deployments of your App: a local one for development and one in the cloud (here on fortrabbit) for production. Both instances probably have access to a database. Your local MySQL has of course different credentials than the remote one. Your config file, storing this information, is under Git version control. So how to deal with different environment-specific configurations of one App?

## Solution

Use environment variables to keep local configurations out of the main code base.

## ENV vars in PHP

You can access your ENV vars from PHP either using the global variable `$_SERVER` or the function `getenv()`:

```php
echo $_SERVER["MY_ENV_VAR"];
# or
echo getenv("MY_ENV_VAR");
```

## Adding ENV vars

You can add ENV vars to your App in the [Dashboard](dashboard) > Your App > Settings > ENV Vars. The input supports the dotenv format and allows you to create or update multiple variables at once.

<div markdown="1" data-user="known">
[Add ENV vars to your App: **{{app-name}}**](https://dashboard.fortrabbit.com/apps/{{app-name}}/vars)
</div>

## ENV var types

There are four different kinds of ENV vars which are available to your App at runtime.


### Custom ENV vars

Those are the ones you add yourself in the Dashboard.


### App ENV vars

Generic ENV vars cannot be overwritten by you. They are always available.

* `APP_NAME` contains the name of your App
* `APP_SECRETS` contains the path to a JSON encoded file containing [App Secrets](secrets)


### Stack ENV vars

Depending on which Stack you have chosen when creating your App, additional ENV vars will be pre-created (seeded) for you. For example: When choosing Laravel the ENV var `APP_KEY` with a random, 32 char long string will created (among others).

You can replace or remove those Stack ENV vars after App creation the same way you can replace or remove your manually created ENV vars.


### Dynamic ENV vars

Dynamic ENV vars become available if you have enabled *"Populate App Secrets in ENV vars automatically"* in the Dashboard, which is the default for new Apps. They contain access details for services offered by fortrabbit.

They will never overwrite existing, manually created ENV vars. This means: if you manually create an ENV var, we guarantee that we won't replace it's value by a dynamically generated ENV var.

Following a list of available dynamic ENV vars, which are available, assuming your App supports the service: <!-- TODO: We cannot say component here, cause Universal Apps don't have components .. idea? -->

**MySQL**

* `MYSQL_DATABASE`: Contains the App's database name
* `MYSQL_HOST`: Contains the App's database hostname
* `MYSQL_PASSWORD`: Contains the App's database password
* `MYSQL_PORT`: Contains the App's database port
* `MYSQL_USER`: Contains the App's database user

**Object Storage**

* `OBJECT_STORAGE_BUCKET`: Contains the App's bucket name
* `OBJECT_STORAGE_KEY`: Contains the App's bucket access key
* `OBJECT_STORAGE_REGION`: Contains the App's bucket region
* `OBJECT_STORAGE_SECRET`: Contains the App's bucket access secret
* `OBJECT_STORAGE_SERVER`: Contains the App's Object Storage API server

**Memcache**

* `MEMCACHE_HOST1`: Contains the first Memcache node hostname
* `MEMCACHE_PORT1`: Contains the first Memcache node port
* `MEMCACHE_HOST2`: Contains the second Memcache node hostname (if using a Production plan)
* `MEMCACHE_PORT2`: Contains the second Memcache node port (if using a Production plan)



## Nested variables

You can use simple, [nested variables](https://github.com/vlucas/phpdotenv#nesting-variables) in your custom ENV vars. Simple means, that you can set variables which reference other variables, which contain a value, for example:

```plain
# will work:
MY_VAR=${OTHER_VAR}
OTHER_VAR=something
```

We do not support multiple levels of interpolation, which means that you cannot use variables, which reference other variables, which again reference other variables, for example:

```plain
# will not work:
MY_VAR=${OTHER_VAR}
OTHER_VAR=${ANOTHER_VAR}
ANOTHER_VAR=something
```

## ENV vars vs security

Storing credentials (passwords, secrets, ..) in environment variables is not without risk. They can be exposed, due to programming errors or oversights. Please read an BLOG[in-depth discussion in our Blog](how-to-keep-a-secret). We offer a convenient solution for this problem with our [App secrets](secrets).


### ENV var encryption

If you prefer not to use [App secrets](secrets), but want to secure your ENV vars we recommend to store them encrypted and encoded in the Dashboard. You then decode and decrypt them later on in your App. Following a generic example:

Create a secret key and use the `mcrypt_encrypt()` function to encrypt and then base64 encode your variables. An exemplary PHP script doing that can be [downloaded from this Gist](https://gist.github.com/ukautz/3573878af39e81c009fa) and then executed locally.

Start with generating a new key and storing this key somewhere in your app's bootstrap code:

```bash
php encrypt.php genkey
# QEbfdTYNH23YddNUa1srixRAwBVs2L5p
```

Then you can encrypt the values of each environment variable, you want to store safely:

```bash
php encrypt.php enc "The Key" "Some Value"
# ENC:YToyOntp...t8CI7fQ==
php encrypt.php enc "The Key" "Some Other Value"
# ENC:XioptiwF...tmMNs9p==
```

To access the decrypted values you simply do the reverse: decode from base64 and decrypt using `mcrypt_decrypt()`. Again, here is an [exemplary Gist](https://gist.github.com/ukautz/0b430aafc7cc996fc946). Using it, you can then decode the environment variables in your app's bootrap:

```php
// ..
bootstrapSecEnv("Your Key");
// ..
```

Later, when you need to access your now decrypted environment variables:

```php
// ..
$dbUser = secEnv("DB_USER");
$dbPass = secEnv("DB_PASS");
// ..
```
