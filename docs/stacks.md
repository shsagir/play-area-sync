
---

template:      article
naviTitle:     "About the Stacks"
title:         "About the Stacks"
reviewed:      2017-04-10
lead:          "With each App you create, you can choose between two technology stacks. This article helps you to understand why there are two stacks and how to decide."
group:         stacks
stack:         all

---


<!--  TODO: this is the same text as on the home page. a bit longer text here probably. write something about project descions, first app this stack, second app that stack … -->

## The two sentence story

**The Universal Stack** is for everyone to build websites and web applications, combining legacy workflows with modern design paradigms. **The Professional Stack** is for spohisticated developers to build modern cloud-enabled high-performance PHP web applications with lot's of traffic.


## The facts table

|                             | Universal Stack                         | Professional Stack                                |
| --------------------------- | --------------------------------------- | ------------------------------------------------- |
| Websites per App            | 1 [‹ why?](/app#toc-one-website-per-app)| 1 [‹ Why?](/app#toc-one-website-per-app)          |
| Pricing model               | fixed plans                             | flexible Components                               |
| Traffic                     | low, medium                             | low - very high                                   |
| Scalability                 | xxs - s                                 | xxs - xxxl                                        |
| High Availability           | no                                      | yes, except Development level                     |
| Local storage               | persistent                              | [ephemeral](#toc-ephemeral-storage)               |
| Primary application type    | websites                                | web applications                                  |
| Secondary application type  | web applications                        | websites                                          |
| Architecture                | single webserver + db server            | distributed containers                            |
| Required skill level        | beginner                                | sophisticated                                     |
| Deployment protocols        | SFTP, Git, SSH                          | Git                                               |
| Composer integration        | after Git push + via SSH                | after Git push                                    |
| SSH integration             | [full](ssh-uni)                         | [remote SSH execution](/remote-ssh-execution-pro) |
| Background Jobs             | a single Cron Job                       | multiple Cron and Nonstop Jobs                    |


## More infos on the stacks

* [Universal Stack article](/app-uni)
* [Professional Stack article](/app-pro)
* [Universal Stack specs](https://www.fortrabbit.com/specs)
* [Professional Stack specs](https://www.fortrabbit.com/specs-pro)