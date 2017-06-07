---

template:      article
reviewed:      2017-03-29
title:         Billing Contact
naviTitle:     Billing Contact
excerpt:       What you can do with a Billing Contact
lead:          You want certain Apps to be charged from a different credit card while keeping team and access settings.
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

A Billing Contact represents the payment details which are used by fortrabbit to bill your [Company](/company). 

Most people are happy with just one Billing Contact, but in some cases you need one App to be paid by one credit card and another App to be paid by another credit card. Billing Contacts allow just that.

A Billing Contact persists of:

* A [payment method](#toc-changing-the-payment-method), which can be credit card or SEPA direct debit (EU only)
* A [invoice address](#toc-changing-the-invoice-address), which will be written on the invoice
* A [billing e-mail address](#toc-changing-the-billing-e-mail-address), to which the invoices will be send to

Each App (except trial Apps) has is always associated with a specific Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing article](/billing).

## The difference between Billing Contact and Company

The Billing Contact is part of a [Company](/company) on fortrabbit. Within the fortrabbit Company you can create multiple Billing Contacts and then assign individual Apps to either Billing Contacts. 

You could also create multiple Companies to achieve that, but with multiple you can still manage the same team on the Company.

### Creating a Billing Contact

You can create a Billing Contact in the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button. Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Billing Contact, no costs, as long as you don't assign an App to the Billing Contact. You must be Owner of the Company for this. You might need to book a Company Plan to enable more Billing Contacts.


### Deleting a Billing Contact

You cannot, because they are bound to previously created invoices and are needed for our accounting. You can, of course, delete or move all Apps away from a Billing Contact so that it becomes inactive and no more bills will be created for it. As the Billing Contact also contains an archive of previously issued invoices, you can always access your old invoices.


### Changing the billing e-mail address

Each month we'll send an invoice and send a link by e-mail. You can change this e-mail to go directly to your book keeping department. Within the Dashboard > Your Account > Companies (Your Company) > Billing Contact > "Invoice e-mail".

### Changing the invoice address

Your invoices contain the street address of your organization. You will be asked for that while setting up a Billing Contact. You can change this invoice address (for future invoices) at any time like so:

Within the Dashboard > Your Account > Companies (Your Company) > Billing Contact > "Invoice address".


### Changing the payment method

Will charge on a monthly basis. You will be asked for your payment method while setting up a Billing Contact. You can change this at any time like so:

Within the Dashboard > Your Account > Companies (Your Company) > Billing Contact > "Payment method".


### Changing the Billing Contact of an App

In the [Dashboard](dashboard), go to your [App](app) and hit the "Change ownership" button. Here you can change the Company (and the [team](collaboration)), as well as the Billing Contact (only affects billing). You must be Owner of the Company for this.


### Moving an App from one Billing Contact to another

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company.


### Adding a VAT IN

It is recomended to add your VAT IN to your Billing Contact when your company is registered in the European Union. This can save you (and us) upfront value added taxes. Please see our [tax topic](/billing#toc-taxes). To enter your VAT IN:

1. Login to the Dashboard
2. Go to "Your Account"
3. Go to your Company
4. Click on "VAT IN"
5. Enter your companies VAT IN

Please mind that we need to validate the VAT IN. So the number you enter needs to match the correct format. Please mind that your VAT IN is not the tax number. The correct number should start with your country code. Our VAT IN looks like so: 

Entering the VAT IN will change how VAT will be charged for future invoices, entering the VAT IN can not affect past invoices.



### Changing the Billing Contact of an App

1. Go to the App in the Dashboard, click the "Change ownership" button
2. This will open a form, where you choose a different Billing Contact

This action can only be executed by Owners or Admins of a Company. The App will then be billed on the old Billing Contact until that day of change and from the next day on to the new Billing Contact. For example: If you move your App on the 15ths of a month to the Billing Contact of another Company, it will be billed until the 15ths to the old Billing Contact and starting from the 16ths to the new Billing contact.