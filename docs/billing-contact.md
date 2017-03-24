---

template:      article
reviewed:      2017-03-24
title:         Billing Contact
naviTitle:     Company
excerpt:       What you can do with a Billing Contact
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

### Goal

You want certain Apps to be charged from a different credit card but want to keep team and access settings for the App.


A Billing Contact represents the payment details which are used by fortrabbit to bill you. Usually you need only one, but in some instances you need one App to be paid by one credit card and another App to be paid by another credit card. Billing Contacts allow that: A Company within fortrabbit is a container to manage billing aspects. Within one [Company](/company) you can create multiple Billing Contacts and then assign individual Apps to either Billing Contacts. In contrast to using multiple Companies: you can still manage the same developer team on the Company.

A Billing Contact persists of:

* A payment method, which can be credit card or SEPA direct debit (EU only)
* A billing address, which will be written on the invoice
* A billing e-mail address, to which the invoices will be send to

Each App (except trial Apps) has is always associated with a specific Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing article](/billing).

### Creating a Billing Contact

You can create a Billing Contact in the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button. Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Billing Contact, no costs, as long as you don't assign an App to the Billing Contact. You must be Owner of the Company for this. You might need to book a Company Plan to enable more Billing Contacts.


### Deleting a Billing Contact

You cannot, because they are bound to previously created invoices and are needed for our accounting. You can, of course, delete or move all Apps away from a Billing Contact so that it becomes inactive and no more bills will be created for it. As the Billing Contact also contains an archive of previously issued invoices, you can always access your old invoices.


### Changing the billing e-mail address

Each month we'll send an invoice and send a link by e-mail. You can change this e-mail to go directly to your book keeping department. Within the Dashboard > Your Account > Companies (Your Company) > Billing Contact > "Invoice e-mail".

### Changing the Billing Contact of an App

In the [Dashboard](dashboard), go to your [App](app) and hit the "Change ownership" button. Here you can change the Company (and the [team](collaboration)), as well as the Billing Contact (only affects billing). You must be Owner of the Company for this.


### Moving an App from one Billing Contact to another

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company.

### Changing the Billing Contact of an App

1. Go to the App in the Dashboard, click the "Change ownership" button
2. This will open a form, where you choose a different Billing Contact

This action can only be executed by Owners or Admins of a Company. The App will then be billed on the old Billing Contact until that day of change and from the next day on to the new Billing Contact. For example: If you move your App on the 15ths of a month to the Billing Contact of another Company, it will be billed until the 15ths to the old Billing Contact and starting from the 16ths to the new Billing contact.