---

template:      article
reviewed:      2016-11-09
title:         Company collaboration on fortrabbit
naviTitle:     Company collaboration
lead:          Leverage Company level collaboration to map your real world structures back to fortrabbit.
group:         platform
stack:         all

keywords:
    - collaboration
    - administrator
    - platform design

---


## Problem

In the real world, you are working with a team. The members of this team have different functions in your company: Some are pure code developers, others take care of your accounting and others oversee your various projects.

## Solution

A [Company](/company), within the fortrabbit platform, represents your business and Company collaboration allows you to map your business setup to fortrabbit. To name a few use cases:

* Designate a responsible Admin to handle maintenance tasks, such as scaling and monitoring metrics
* Let your project managers handle their projects by themselves, by granting them rights to invite and manage App level collaborators
* Grant your accounting access to fortrabbit, so that they can download the invoices on their own


## Booking a Company collaboration plan

Company collaboration is a paid feature, see our [fancy marketing page](https://www.fortrabbit.com/collaboration) for plans and prices. They are optional and always booked for an individual Company. Like any other resource with fortrabbit, they are billed on a precise daily settlement.

To book a Company collaboration plan: In the Dashboard > navigate to your Account > Companies > {{ Your Company }} > and there to "Company collaboration", this will show you a screen to book a plan. This way you can also upgrade and downgrade the Company collaboration plan.


## Roles

A unique quality of Company collaboration are the role based permissions. Delegation is the key motivation. Only with Company collaboration members with the according roles will get automatic access to newly created Apps and members with according rights can create Apps on behalf of the Company. Each Company can have multiple Accounts associated with it:

### Owner

The Owner role can NOT be modified by someone else, multiple Owners per Company are possible. Each Company must have at least one Owner. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* access code of all Apps of the Company
* delete, create, change Billing Contacts for the Company
* manage all roles (but other Owners) of the Company
* invite new Users for any role to the Company
* leave the Company (if other Owners are present)
* delete the Company


### Admin

The Admin role can be modified by Owners. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* access code of all Apps of the Company
* manage all Collaborators of the Company
* invite new Admins or Collaborators to the Company
* leave the Company

### Collaborator

The Collaborator role can be modified by Owners and Admins. It's the same as with [App collaboration](app-collaboration) and has the same permissions. When any paid Company collaboration plan is booked, all Apps, including all Universal Apps, are granted unlimited App collaborators.



## Inviting a Company collaborator

The steps to invite a Company collaborator are the same as described [in the App collaboration](app-collaboration#toc-inviting-an-app-collaborator). The difference is that you can choose additional Company level roles.



## Advanced collaboration usage

Still reading? Cool go on to dive even deeper.

### Working with multiple Companies

Just like in real life, you can create or join any number of Companies with your Account. So one Account can own multiple Companies while also being involved in other Companies as Admins or even App level collaborators.


### Working with the same person in different Companies

If you have multiple Companies and you want to collaborate with same person on all or a subset of them: Just invite them to each Company. The same person then will have access to all Companies while logged in with their Account.

### Moving an App from one Billing Contact to another

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company.

### Changing the Billing Contact of an App

1. Go to the App in the Dashboard, click the "Change ownership" button
2. This will open a form, where you choose a different Billing Contact

This action can only be executed by Owners or Admins of a Company. The App will then be billed on the old Billing Contact until that day of change and from the next day on to the new Billing Contact. For example: If you move your App on the 15ths of a month to the Billing Contact of another Company, it will be billed until the 15ths to the old Billing Contact and starting from the 16ths to the new Billing contact.


### Moving an App from one Company to another

In some cases you might want to move the App to a different Company. This will change billing AND team. Of course you first need to be at least Admin in two Companies.

### Changing the Company of an App

2. Go to the App in the Dashboard, click the "Change ownership" button
3. This will open a form, where you choose a different Company (and Billing Contact)

This action can only be done by Accounts who have Owner or Admin privileges on both Companies.

<!--  TODO: Confirm/test this: is it REALLY only possible to move an App to another Company, if I am only Collab in the other Compnay?  -->


### Moving a Company member to another Company

Well, that's not what you do, actually. What you'll do is: invite the same person again to the another Company. The Account will then be able to see and access Apps from both Companies under one login. The Account itself or you (as the Admin or Owner) can then revoke access to the first Company.

### Downgrading a Company collaboration plan

Visit the booking page as described above and select the new, smaller plan or no plan, as needed. If you want to downgrade a paid Company collaboration plan to the free Company collaboration plan, then you need first to make sure, that:

* Only one Owner exists
* No Admins exist
* No App has more App collaborators then the App allows (Professional unlimited, Universal are limited, see [specs page](//www.fortrabbit.com/specs#limits))




### Promoting and demoting people

Just like in real life the roles of Accounts inside a Company can change like so:

* Accounts can not downgrade or upgrade themselves to another role
* Admins can promote App collaborators to become Admins
* Owners can promote App collaborators to become Admins or Owners
* Owners can promote Admins to become Owners
* Owners can demote Admins to become App collaboratoes

Role changes have immediate effect, they do not require another confirmation by the affected Account.


#### Changing the role of a collaborator

If Company collaboration is active and you are either Owner or Admin, you can promote and demote other people like so: Visit the Company the Account is part of, click on the role of Account. This will open a form in which you can change the role of the Account.



### If people leave

The Account who leaves a Company will loose the ability to see and edit the App in the Dashboard and also will loose all personal code access to the App. There are two directions of leaving a Company:

#### Active: An Account leaves a Company

You might want to leave the Company when the project you have collaborated on has ended or you are actually leaving the Company in real live as well. Each Account can leave a Company at any time.

##### How to activly leave a Company

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > "Leave Company" button

There is one special case in which you can not leave a Company and that is when you are the last Owner. A Company must have at least one Owner.


#### Passive: An Owner or Admin terminates Company access for another Account

Let's say a project phase has ended, so that an App Collaborator does not need have access any more. Who can can fire whom:

* An Owner can remove Admins and App collaboratores
* An Admin can remove App collaborators
* An App Collaborator can remove nobody
* No one can remove an Owner

##### How to remove Accounts

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > Under Collaborators > Click the role of the Account you would like to remove. This will bring up a form in which you can revoke access.


#### Confirmation on cancellations

To make the process of leaving transparent, all involved parties will get a notification mail about the action: All owners, the account who did the removal and the removed account itself.


#### A note on security when people leave

Please mind that the person who has left still might have local code copies and thereby access to sensitive data like MySQL passwords. We advice to reset service passwords. Please see the [security article](security#password-reset) for more.

<!-- TODO: make link to security section above work  -->


### How to work with a non-techie client

<!--  TODO: rewrite on passive owner launch -->

In general, we expect all Users to be involved in web development — this is true for agency- and startup-workflows. So the Dashboard boarding is designed for developers. In some real live business relationships the "client" is not a techie at all and just a passive business owner. There are two ways to do this this now (we have plans for something better):


#### 1. The client signs up and invites the developer

1. The client creates a free Account: [sign up](https://dashboard.fortrabbit.com/signup)
2. After e-mail confirmation the client skips the App setup by clicking "Your Account
3. The client clicks "[Set up a new Company](https://dashboard.fortrabbit.com//account/company/new)" and follows the steps
4. The client clicks on "Invite someone" and invites the developer as an Admin
5. The developer accepts the invitation and is now Admin for the Company

This clean workflow maps the real world relationship closely. The client is the business owner, while the developer is an Admin in the Company. It requires the client to interact with the fortrabbit Dashboard.


#### 2. The developer manages the clients business

1. The developer clicks "[Set up a new Company](https://dashboard.fortrabbit.com//account/company/new)" and follows the steps
2. The developer uses the billing informations provided by his client
3. The developer changes the "Invoice e-mail" address to the client

This workflow is faster and does not involve interaction from the client at all. It is however not so clean as the the developer needs to have access to confidential credit card informations.
