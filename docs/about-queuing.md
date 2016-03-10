---

template:      article
reviewed:      2016-03-10
title:         Queuing on fortrabbit
naviTitle:     Queuing
lead:          What you can do to safely offload work from your App to the background.

tags:
    - advanced

keywords:
    - performance
    - AMQP
    - RabbitMQ

seeAlsoLinks:
    - cloudamqp
    - app-design

---

OK, you want to make real good use of your [Worker](/worker) Component on fortrabbit? Combine it with a message queuing service.

Message queues allow you to stash work for later so that it gets done eventually. Imagine sending out a mail to your thousands of customers. You can send it from a controller in your admin web interface - and wait for a couple of coffess while blocking the resources for actual visitors. Or you can use the Worker and send them in a big batch - during which your mail provider experiences a short load surge and your execution fails half way through.

Or you can use a message queue, in which you stash an individual job for sending each mail to each individual customer. Then you pull the queue one by one from your Worker and send out the mails. If your mail providers surges then the particular mail is not send and the queue message is put back on the queue. Problem solved.