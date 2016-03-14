---

template:      article
reviewed:      2016-03-10
title:         Transactional e-mail on fortrabbit
naviTitle:     Transactional e-mail

tags:
    - advanced

keywords:
    - performance
    - AMQP
    - RabbitMQ

seeAlsoLinks:
    - quirks
    - sendgrid

---

Please mind that you can NOT use `sendmail` out of the box with fortrabbit — see our [quirks article](/quirks#toc-mailing). So you either use SMTP in combination with a classical e-mail provider (IMAP/POP) or a transactional e-mail provider.

So, you want to send your "forgotten passsword link mail" from your App effortless. Unlike a newsletter which is initiated by you or your marketing devision and send in a batch, transactional e-mails are automated customer relationship messages triggered by user action, one by one.

## Transactional e-mail examples

* double-opt-in e-mail during signup for a service
* receipt e-mails like order confirmation or invoice
* follow-up e-mails
* forgotten password, reset password e-mail


## Pricing models

Transactional mail service providers usually offer a subscription model based on a monthly volume or a pay as you go plans. The later are often more expensive but still a good choice to get started. There might also be a free tier.

## Scope of service

The basic feature is just sending e-mails. Most providers will also offer analytical tools. Some are also offering marketing e-mail marketing tools (newsletter), others offer processing of incoming e-mail handling as well. 


## Setup

Transactional e-mail is not bound to data latency or infrastructure — as e-mail is not real time communication.

The tricky thing is to set up your domain. As you want send e-mails from your own domain, you'll need to set up DKIM and SPF for your domain. Please see the providers docs.


## Sending e-mails

Transactional e-mail services usually offer an API or simply SMTP interface. In our experience the API way should be preferred, as it is faster and more reliable. There might be a Composer package which can easily be integrated and used.