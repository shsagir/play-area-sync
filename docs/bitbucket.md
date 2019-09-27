---

template:       article
reviewed:       2019-09-27
naviTitle:      Bitbucket
title:          Combine fortrabbit with Bitbucket
lead:           Learn how to integrate the second most popular Git-as-a-service provider with your fortrabbit workflow.
stack:          all
group:          Code_collaboration
section:        Expanding_fortrabbit


websiteLink:      https://bitbucket.org/?utm_source=fortrabbit
websiteLinkText:  bitbucket.org
category:         Code collaboration
dataCenters:      n/a
image:            bitbucket-mark.svg

keywords:
    - addon
    - add-on
    - integrations
    - integration
    - API
    - CI
    - cloud

---

[Bitbucket](https://bitbucket.org?utm_source=fortrabbit) is a Git hosting service that offers advanced Git work-flows, such as 'pull requests'. It offers private repos in a free plan and is a popular choice in combination with fortrabbit. It also offers [paid plans](https://bitbucket.org/product/pricing) for "growing teams".

Bitbucket is similar to GitHub, please hop over to the [GitHub integration](github) article to learn about available work-flows, like having two remotes.

## Deployment pipeline

Bitbucket has pipelines to handle hooks.

In order, for BitBucket to push to your fortrabbit App, you have to [create an SSH key pair with Bitbucket](https://confluence.atlassian.com/bitbucket/use-ssh-keys-in-bitbucket-pipelines-847452940.html) and then [add the public key to your fortrabbit App](access-methods#toc-app-only-ssh-keys).

