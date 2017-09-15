---

template:       article
reviewed:       2017-09-15
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

Bitbucket has pipelines to handle hooks. [Here](https://gist.github.com/ukautz/4f3219c3eb5d97fbd018027dca4b8808) is an example for a pipeline that will push to fortrabbit, with the `pipeline.yml` and a `deploy.php`.

