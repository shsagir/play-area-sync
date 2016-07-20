---

template:         article
reviewed:         2016-03-10
naviTitle:        GitHub
title:            Combine fortrabbit with GitHub
lead:             Learn how to integrate the most popular Git-as-a-service provider with your fortrabbit workflow.

group:            Code_collaboration
section:          Extending_fortrabbit

websiteLink:      https://github.com?utm_source=fortrabbit
websiteLinkText:  github.com
image:            github-octocat.png
dataCenters:      n/a

seeAlsoLinks:
    - about-code-collaboration
    - git-deployment
    - git
    - git-submodules
    - bitbucket

tags:
    - advanced

keywords:
    - git-hub
    - addon
    - add-on
    - integrations
    - integration
    - API
    - CI
    - cloud
    - vcs
    - add-on
    - version control

---

## About GitHub

You hopefully already know that [GitHub](https://github.com) is the most popular choice for versionized code hosting and collaborating. It is free to use with open source projects. This means that all the code you publish in the free version will be visible to everyone. But you can — of course — [purchase a plan](https://github.com/pricing) including private repos as well.

### GitHub is not Git

GitHub is so popular that beginners sometimes confuse GitHub with Git. Git is the version control system established by Linus Torvalds. GitHub is the service, which offers Git remote hosting and additional extra magic collaboration features.

## Integration

It's actually quite simple: add your external Git-provider as one remote and fortrabbit as another remote. Then you can push to your external provider while in development and collaborating; push to fortrabbit to see the code in action.

Here is how you add your App's Git remote on fortrabbit to your already existing local working copy of a GitHub / Bitbucket repo and then work with it:

```bash
# add your App's remote and name it "fortrabbit"
$ git remote add fortrabbit {{ssh-user}}@deploy.{{region}}.frbit.com:{{app-name}}.git

# now push to the master branch of your App's remote on fortrabbit
$ git push fortrabbit master
```

## GitHub API limits and Composer

The reason why you can run into this issue temporarily is that GitHub limits it's API per IP. Given that you deploy on our Git servers (or use composer from our SSH servers) this IP address is shared with other developers deploying there as well. So if there is a deployment spike, GitHub might close down for a while.

The solution is to create a GitHub OAuth token and put it in your `composer.json` file. Open up a terminal and issue the following command:

```bash
curl -u your-github-user -d '{"note": "Fortrabbit Auth"}' https://api.github.com/authorizations
```

This will give you a response like the following:

```
{
  "id": 123456,
  "url": "https://api.github.com/authorizations/123456",
  "app": {
    "name": "Fortrabbit Auth (API)",
    "url": "http://developer.github.com/v3/oauth/#oauth-authorizations-api",
    "client_id": "12345abc12345"
  },
  "token": "12345abc1234512345",
  "note": "Fortrabbit Auth",
  "note_url": null,
  "created_at": "2013-08-08T11:08:18Z",
  "updated_at": "2013-08-08T11:08:18Z",
  "scopes": []
}
```

What you need is the `token` value. In the above example it is `12345abc1234512345`. Open up your `composer.json` file and add the token withunder the `config` directive:

```
{
  "require": {
    "awesome/package": "@dev"
  },
  "config": {
    "github-oauth": {
      "github.com": "12345abc1234512345"
    }
  }
}
```

That's it. Your API token will be used to give you a personal rate limit which is much more higher than the default one.
