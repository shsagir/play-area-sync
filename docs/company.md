---

template:      article
reviewed:      2016-11-11
title:         Companies on fortrabbit
naviTitle:     Company
excerpt:       How to work with Companies on fortrabbit
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


Your fortrabbit Account represents YOU and is your personal login and path to access Apps. A Company on fortrabbit helps you to structure and manage team and billing aspects of your Apps.

```nohighlight
┌─────────────────────────────────────────────────────┐
│                       Company                       │
│                                                     │
│    Apps           Accounts        Billing Contacts  │
│  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐  │
│  │ {{app-one}} │ │  {{owner1}}  │ │ {{Billing1}} │  │
│  ├─────────────┤ ├──────────────┤ ├──────────────┤  │
│  │ {{app-two}} │ │  {{owner2}}  │ │ {{Billing1}} │  │
│  ├─────────────┤ ├──────────────┤ └──────────────┘  │
│  │{{app-three}}│ │  {{admin2}}  │                   │
│  └─────────────┘ ├──────────────┤                   │
│                  │  {{admin1}}  │                   │
│                  └──────────────┘                   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

The above illustration shows how Companies, Apps, Accounts and Billing Contacts relate to each other:

* Each App belongs to a Company.
* A Company can have multiple Apps.
* A Company can have multiple members ("employees") to collaborate on the Apps.
* A Company also can have multiple Billing Contacts to pay for the Apps.
* Each App is associated with one Billing Contact of the Company.

## Managing Companies

We believe in graceful disclosure: Companies are virtually invisible and for all intends and purposes synonym to your Account - until you need them.

### Creating a Company

You can create as many Companies as you need — free of charge. In the the Dashboard > Your Account > Companies > "Create a Company" button. Following the guided create process, you will not only create a new Company, but also it's first Billing Contact. Once created, you will be the Owner of the new Company.

<div data-markdown="1" data-user="known">

* [Create a new Company](https://dashboard.fortrabbit.com/account/company/new) (login required)

</div>

### Deleting a Company

As an Owner you can also decide to delete your Company. This is a huge step, as all Apps owned by the Company will be deleted and all the team will loose access. To do so: In the Dashboard > Your Account > Companies > {{ Your Company }} > Hit the "Delete Company" button and follow the instructions.

### Collaboration

There are two levels of collaboration

1. [App level collaboration](app-collaboration): Grant code access on a per-App basis
2. **[Company level collaboration](company-collaboration)**: Grant company-wide access to all Apps and the Companies themselves

Since collaboration can easily become rather complex, we also have [collaboration overview article](collaboration), which shows you the big picture.

### Support

Beside basic free support, you an also get [Professional Support](//www.fortrabbit.com/support) from us. Support plans are booked on behalf the Company and are available for the whole team.

### Automatic resource exhaustion alerts

Each App with fortrabbit has limited resources, most of them are scalable or extendible. We advice to start small and only scale when needed. Some resources scale by usage. For example, when your App has users who are uploading images, the web space or the Object Storage space will get fuller and fuller by time.

All collaborators on the App will see those usage metrics at the App overview in the Dashboard. When a resource is about to hit the limit, it will be shown in a yellow color to alert you.

All Owners of the App will get an e-mail, once a usage metric is about to hit the limit. Please see our [specs page](https://www.fortrabbit.com/specs#limits) to learn about the resources with App alerts and how the limits are handled.

<!--

TODO: uncomment and extend when Technical Contact feature launches

#### Setting a technical contact

If you have a Company collaboration plan booked, all Owners and Admins of the Company can change the technical contact.

In the Dashboard under "Your Account" > "Companies" > {{ Company }} > "Technical contact" you can set one or more contacts. This can be any Account associated with the Company or any e-mail address you like.

You can define the services you want to receive alerts for. By default all services are enabled. Further: you can tune the settings to include or exclude certain Apps. You can also overwrite those settings on App level.

Individual Accounts can opt-out of receiving those alerts by deselecting this from their Accounts notification settings.

-->


## Billing Contacts

A Billing Contact represents the payment details which are used by fortrabbit to bill you. Usually you need only one, but in some instances you need one App to be paid by one credit card and another App to be paid by another credit card. Billing Contacts allow that: A Company within fortrabbit is a container to manage billing aspects. Within one Company you can create multiple Billing Contacts and then assign individual Apps to either Billing Contacts. In contrast to using multiple Companies: you can still manage the same developer team on the Company.

A Billing Contact persists of:

* A payment method, which can be credit card or SEPA direct debit (EU only)
* A billing address, which will be written on the invoice
* A billing e-mail address, to which the invoices will be send to

Each App (except trial Apps) has is always associated with a specific Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing article](/billing).

### Creating a Billing Contact

You can create any number of Billing Contact free of charge in the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button. Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Billing Contact, no costs, as long as you don't assign an App to the Billing Contact. You must be Owner of the Company for this.

Do this when you want certain Apps to be charged from a different credit card but want to keep team and access settings for the App.


### Deleting a Billing Contact

You cannot, because they are bound to previously created invoices and are needed for our accounting. You can, of course, delete or move all Apps away from a Billing Contact so that it becomes inactive and no more bills will be created for it. As the Billing Contact also contains an archive of previously issued invoices, you can always access your old invoices.


### Changing the billing e-mail address

Each month we'll send an invoice and send a link by e-mail. You can change this e-mail to go directly to your book keeping department. Within the Dashboard > Your Account > Companies (Your Company) > Billing Contact > "Invoice e-mail".

### Changing the Billing Contact of an App

In the [Dashboard](dashboard), go to your [App](app) and hit the "Change ownership" button. Here you can change the Company (and the [team](collaboration)), as well as the Billing Contact (only affects billing). You must be Owner of the Company for this.
