---

template:      article
naviTitle:     "About the Stacks"
title:         "About the Stacks"
reviewed:      2016-11-10
lead:          "With each App you create, you can choose between two technology stacks. This article helps you to understand why there are two stacks and how to decide."
group:         platform
stack:         all

---

## The two sentence story

**The Universal Stack** is for everyone to build websites and web applications, combining legacy workflows with modern design paradigms. **The Professional Stack** is for spohisticated developers to build modern cloud-enabled high-performance PHP web applications with lot's of traffic.


## The facts table

|                             | Universal Stack                         | Professional Stack                                |
| --------------------------- | --------------------------------------- | ------------------------------------------------- |
| Websites per App            | 1 [‹ why?](/app#toc-one-website-per-app)| 1 [‹ Why?](/app#toc-one-website-per-app)          |
| Pricing model               | fixed plans                             | flexible Components                               |
| Traffic                     | low, medium                             | low - very high                                   |
| Scalability                 | xxs - s                                 | xxs - xxxl                                        |
| High Availability           | no                                      | yes, except Tinkering                             |
| Local storage               | persistent                              | [ephemeral](#toc-ephemeral-storage)               |
| Primary application type    | websites                                | web applications                                  |
| Secondary application type  | web applications                        | websites                                          |
| Architecture                | single webserver + db server            | distributed containers                            |
| Required skill level        | beginner                                | sophisticated                                     |
| Deployment protocols        | SFTP, Git, SSH                          | Git                                               |
| Composer integration        | after Git push + via SSH                | after Git push                                    |
| SSH integration             | [full](ssh-uni)                         | [remote SSH execution](/remote-ssh-execution-pro) |

## More infos on the stacks

* [Universal Stack article](/app-uni)
* [Professional Stack article](/app-pro)

## Legacy stacks

* [Old Apps](/app-old)