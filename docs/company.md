---

template:      article
reviewed:      2016-11-03
title:         Companies on fortrabbit
naviTitle:     Company
lead:          How to work with Companies on fortrabbit
group:         platform
stack:         all

keywords:
    - beginner
    - platform
    - teamwork
    - sharing
    - roles
    - organization
    - access
    - tenant
    - client
    - epmloyees

---

<!-- TODO: maybe remove this intro? "you are wrong here" = negative tone?!

If you only have one or a few Apps and want to pay them directly from the same credit card, you are wrong here: All is good, no need to get into the details. 

Now, if that is not sufficient for you and you need to manage Apps within multiple Companies, paid by the same or multiple different entities while using the same Account to do all of that: You are right here. Let us show you how it works.

-->

## Compaines on fortrabbit

Your fortrabbit Account represents YOU and is your personal login and path to access Apps. A Company on fortrabbit helps you to structure and manage team and billing aspects for your Apps. A Company can have one team and multiple Billing Contacts.

## Creating a Company

You can create as many Companies you need — free of charge. In the the Dashboard > Your Account > Companies > "Create a Company" button.

In the following steps you will create a Company with a first Billing Contact. Each Company needs at least one Owner, you are the one at first.

<div data-markdown="1" data-user="known">

* [Create a new Company](https://dashboard.fortrabbit.com/account/company/new) (login required)

</div>

## Collaboration

There are two levels of collaboration:

1. [App level collaboration](app-collaboration): Grant code access on a per-App basis
2. **[Company level collaboration](company-collaboration)**: Grant company-wide access to all Apps and the Companies themselves

## Support

<!-- TODO: write about how to book support -->

[Support](//www.fortrabbit.com/support): Get help from us, when needed


## Billing Contacts

<!-- TODO: check with billing article: how much infos should be displayed here? -->

A Billing Contact represents the payment details which are used by fortrabbit to bill you. Usually you need only one, but in some instances you need one App to be paid by one credit card and another App to be paid by another credit card. Billing Contacts allow that: A Company within fortrabbit is a container to manage billing aspects. Within one Company you can create multiple Billing Contacts and then assign individual Apps to either Billing Contacts. In contrast to using multiple Companies: you can still manage the same developer team on the Company.

A Billing Contact persists of:

* A payment method, which can be credit card or SEPA direct debit (EU only)
* A billing address, which will be written on the invoice
* A billing e-mail address, to which the invoices will be send to

Each App (except trial Apps) has is always associated with a specific Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing FAQ](/billing#toc-faq).

### Creating a Billing Contact

You can create any number of Billing Contact free of charge in the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button

### Deleting a Billing Contact

You cannot, because they are bound to previously created invoices and are needed for our accounting. You can, of course, delete or move all Apps away from a Billing Contact so that it becomes inactive and no more bills will be created for it. As the Billing Contact also contains an archive of previously issued invoices, you can always access your old invoices.


### Deleting a Company

As an Owner you can also decide to delete a Company. This is a huge step, as all Apps owned by the Company will be deleted and all the team will loose access. To do so: In the Dashboard > Your Account > Companies > {{ Your Company }} > Hit the "Delete Company" button and follow the instructions.



## App alerts

Each Company can has one or many technical contacts which will be used for automatic alerting when App resources are about to exceed and in special cases like where we to interact manually need to contact you personally quickly — for instance when we detect abnormal patterns which indicate an attack or otherwise unwanted behavior.

<!--

TODO: uncomment and extend when Technical Contact feature launches

### Setting a technical contact

If you have a Company collaboration plan booked, all Owners and Admins of the Company can change the technical contact.

In the Dashboard under "Your Account" > "Companies" > {{ Company }} > "Technical contact" you can set one or more contacts. This can be any Account associated with the Company or any e-mail address you like.

You can define the services you want to receive alerts for. By default all services are enabled. Further: you can tune the settings to include or exclude certain Apps. You can also overwrite those settings on App level.

Individual Accounts can opt-out of receiving those alerts by deselecting this from their Accounts notification settings.

-->

### Automatic resource exhaustion alerts

Each App with fortrabbit has limited resources, most of them are scalable or extendible. We advice to start small and only scale when needed. Some resources scale by usage. For example, when your App has users who are uploading images, the web space or the Object Storage space will get fuller and fuller by time.

All collaborators on the App will see those usage metrics with the App overview in the Dashboard. When a resource is about to exceed, it will be shown in a yellow color to alert you. In addition App alert e-mails will be send once a resource limit is about to exceed.


<!--  TODO: decide what about Play App plan, which originally does not include metrics at all - see tikcet -->

All owners of the App will get an e-mail, once a usage metric is about to exceed or already exceeded it's limit. Please see our [specs page](https://www.fortrabbit.com/specs#limits) to learn about the resources with App alerts and how the limits are handled.
