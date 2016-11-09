---

template:      article
reviewed:      2016-11-03
title:         Collaborating on fortrabbit
naviTitle:     Collaboration
lead:          Learn about the powerful teamwork features and how to map your real world business relationships on your favorite hosting platform — fortrabbit.
group:         platform
stack:         all

keywords:
    - collaboration
    - beginner
    - platform
    - teamwork
    - sharing
    - permissions
    - roles
    - organization
    - access
    - multi-client capability
    - multitenancy
    - tenant
    - client
    - epmloyees
    - developer
    - collaborator
    - administrator
    - platform design

---

<!--

TODO: this article is veryyyyyyy long. It is not ment to be read from top to down, more like a reference, still it might be possible to break it up in various other articles — a dedicated Company Article for example …

-->


## The pitch

**Usual business scenario**: Most of our clients are running three or more Apps on fortrabbit. And not only that: They want this App to be paid from that card, and the other App from that other card — while still being able to share access with coworkers.

**The natural reflex**: is to set up a new Account for each App and share the login details to the Dashboard across the team. The more Apps you manage that way, the more of a mess you will end up with: no bird's eye view about what is going on, you need to logout and login again to switch to another App, manage all kind of stuff twice, send passwords by e-mail, reset passwords when someone leaves …

**What you really want**: Easily add new members to all of your team’s projects. The solution is a multi client model that maps to your real world in nearly any context: freelancer, startup, digital agency.



## Understanding the Account

When signing up with fortrabbit you'll be asked for your first and your last name. We expect that an Account represents a human being, not a Company. An Account is a person who can login to the Dashboard, manage Apps and Companies. An Account can own or be part of multiple Companies. That is useful for example when you use fortrabbit privately for your week-end projects and for business. Or if you as a freelancer collaborate with various digital agencies.

You must never share your fortrabbit Account; it's "private". It's only you, with all your personal [public SSH keys](/ssh-keys) and your unique access to the [Dashboard](/dashboard).

### Personal Account access to Apps

In classical hosting you have one set of server access credentials for SFTP/SSH which you then have to share. With fortrabbit it's better: each Account has it's own personal access to each App.

```
# Personal code access with fortrabbit
┌────────────────┐                     ┌──────────────────────────┐
│                │                     │                          │
│    Account1    ├───personal access───▶                          │
│                │                     │                          │
└────────────────┘                     │           App            │
┌────────────────┐                     │                          │
│                │                     │                          │
│    Account2    ├───personal access───▶                          │
│                │                     │                          │
└────────────────┘                     └──────────────────────────┘
```


Instead of sending an e-mail with a FTP password to another developer, you are sending an invitation to create a free Account and to get instant personal access to the Apps. This is more secure, more transparent and easier to handle. Also see the [access methods article](/access-methods) to learn about the different ways (SSH key or password) to access code.


## Levels of team collaboration

```
┌────────────────────────────────────────────────────────┐    •
│                                                        │    •
│                                                        │    •
│                                                        │    •
│                        Company                         │    •
│                                                        │    •
│                                                        │    •  Level 2
│                                                        │    •  Advanaced
└─────▲─────────┬─────────────┬────────────┬────────▲────┘    •  Company level
      │                                             │         •  team cooperation
    Owner       │             │            │      Admin       •  where Owners & Admins
      │                                             │         •  delegate on behalf
┌─────┴──────┐  │             │            │ ┌──────┴─────┐   •  of the Company
│            │                               │            │   •
│  Account1  ├──┼────────┐    │    ┌───────┼─┤  Account3  │   •
│            │           │         │         │            │   •
└─────┬──────┘  │        │    │    │       │ └──────┬─────┘   •
      │                  │         │                │         •
      │         │        │    │    │       │        │         •
      │                  │         │                │         •
┌─────▼─────────▼─┐  ┌───▼────▼────▼───┐ ┌─▼────────▼──────┐  •
│                 │  │                 │ │                 │  •
│      App1       │  │      App1       │ │      App1       │
│                 │  │                 │ │                 │  °
└─────────────────┘  └────────▲────────┘ └─────────────────┘  °  Level 1
                              │                               °  Simple
                              │                               °  App level
                       ┌──────┴─────┐                         °  collaboration
                       │            │                         °  where an Account
                       │  Account2  │                         °  has access to a
                       │            │                         °  single App
                       └────────────┘                         °
```

There are two levels of teamwork:

1. **[App level collaboration](#toc-app-level-collaboration)**: Collaborators have access on per-App basis
2. **[Company level collaboration](#toc-company-level-collaboration)**: Company members act on behalf of the Company

Both levels work side by side and can be combined. Also check out our [fancy explaining video](http://help.fortrabbit.dev/teamwork-video).






## App level collaboration

Give someone code access — quickly. Just invite your coworker/colleague to a single App.

### App collaborators permissions

This is what App collaborators can do:

* CAN access code via Git, SSH and SFTP
* CAN see the App in the Dashboard (settings, scaling, collaboration)
* CAN see the Company the App belongs to
* CAN change settings of an App in the Dashboard
* CAN NOT invite other collaborators
* CAN NOT scale an App
* CAN NOT delete an App

### App collaboration use cases

<!-- TODO/TBD: Seems to be one use case, described 4 times

* Simple App sharing to get started with collaboration
* Sharing the weekend project code with colleague to help debugging
* Freelancer coworker get's access on one project
* Contract worker to help out with a project

 -->

* Continuously develop a single App with other developers
* Collaborate with 3rd party freelancer on a single project
* Quickly grant temporary code access to a developer, for tasks as code review or solving a specific problem



### Inviting an App collaborator

1. In the [Dashboard](/dashboard) overview: Click the "Invite someone" button
2. Enter name and e-mail of your colleague
3. If you are part of multiple Companies, you will be asked to choose the Company
4. Next you will be asked, which kind Level of Collaboration you want, choose "App collaboration"
5. In the last step you can choose between Apps you want your colleague to be invited to

This will send an invitation e-mail. The recipient then can either deny, by just ignoring the e-mail, or accept by following the sign-up link.

You can start this process from your Company or from your App.


### Removing an App collaborator

You can find the User App collaborator access form in two locations:

1. **From the Company:**  In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role.
2. **From the App:** In the Dashboard: Navigate to the App the Collaborator is part of, under Team access click on the collaborators access role.

Within this form you can add or remove Apps for this Collaborator.


### Promoting an App collaborator

When you already have a Company collaboration plan you can promote a Collaborator to become Admin or Owner. In the Dashboard: Navigate to the Company the App is part of. Under App collaborators, click on the collaborators access role. This will open a form where you can change the role.

You can also find the Collaborators profile page from the respective App's overview.


### Assigning more Apps to the same App collaborator

You can, of course, grant the same Account collaborating rights on App level to multiple of your Apps. In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role. There you can select all Apps with enabled collaboration for this collaborator.


### App collaboration availability

App collaboration may or may not be available for your App:

* no App collaboration during trial
* limited App collaborators for different Universal stack Apps, see [specs page](https://www.fortrabbit.com/specs).
* unlimited App collaborators for all Professional stack Apps
* unlimited App collaborators for all Apps when a Company level plan is booked





## Company level collaboration

Life is complicated and professional life even more so. [Above](#toc-app-collaboration) we have seen how App collaboration helps to collaborate on a per-App-basis. Collaboration on Company level goes a step further.

A Company within the fortrabbit platform represents your business. When your business is very small you won't notice the difference between your Account and your Company: if you own a single App and you are working on it alone or with another App collaborator your Account and the Company are about the same, for most purposes.

Within your Company you can manage your team — invite and remove collaborators, change the role of Company members and add or remove Apps from App collaborators. Just like in real live the owners have full access, full control, full responsability and need to take care about the financials. While the Admins have all the fun and can act on behalf of the Company.


### Use cases

<!-- TODO/TBD: Not really use-cases, but vague descriptions

* Map real world business relationships
* Project based workflows with delegation
* Multi-cient capable workflows
* Agency and startup style collaboration

-->

* Designate a responsible Admin to handle maintenance tasks, such as scaling and monitoring metrics
* Let your project managers handle their projects by themselves, by granting them rights to invite and manage App level collaborators
* Map your real world company structure to fortrabbit, by granting all Owners their rights




### Creating a Company

You can create as many Companies you want — free of charge. In the the Dashboard > Your Account > Companies > "Create a Company" button.

In the following steps you will create a Company with a first Billing Contact. Each Company needs at least one Owner, you are the one at first.

* [dashboard.fortrabbit.com//account/company/new](https://dashboard.fortrabbit.com//account/company/new) < direct link


### Deleting a Company

As an Owner you can also decide to delete a Company. This is a huge step, as all Apps owned by the Company will be deleted and all the team will loose access. To do so: In the Dashboard > Your Account > Companies > {{ Your Company }} > Hit the "Delete Company" button and follow the instructions.



### Booking a Company collaboration plan

Company collaboration is a paid feature, see our [fancy marketing page](https://www.fortrabbit.com/collaboration) for plans and prices.

Collaboration plans — like [professional support plans](https://www.fortrabbit.com/collaboration) — are optional and mapped to a Company. Like any other resource with fortrabbit, they are billed on a precise daily settlement.

To book a Company collaboration plan: In the Dashboard > navigate to your Account > Companies > {{ Your Company }} > and there to "Company collaboration", this will show you a screen to book a plan. This way you can also upgrade and [downgrade](#toc-downgrading-a-company-level-plan) the Company collaboration plan.


### Roles

A unique quality of Company collaboration are the role based permissions. Delegation is the key motivation. Only with Company collaboration members with the according roles will get automatic access to newly created Apps and members with according rights can create Apps on behalf of the Company. Each Company can have multiple Accounts associated with it:

#### Owner

The Owner role can NOT be modified by someone else, multiple Owners per Company are possible. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* code access to all Apps of the Company
* delete, create, change Billing Contacts for the Company
* manage all roles (but other Owners) of the Company
* invite new Users for any role to the Company
* leave the Company (if other Owners are present)

Each Company must have at least one Owner.

#### Admin

The Admin role can be modified by Owners. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* code access to all Apps of the Company
* invite new Admins or Collaborators to the Company
* leave the Company

### Collaborator

The Collaborator role can be modified by Owners and Admins. It's the same as with App collaboration and has the same permissions (see [above](#toc-app-collabortors-permissions)). But when a Company collaboration plan is booked, all Apps, including all Universal Apps, have unlimited App collaborators.


### Downgrading a Company collaboration plan

To disable Company collaboration, visit the Company collaboration booking page (see [above](#toc-booking-a-company-collaboration-plan)) again and disable it. This will have the following consequences:

* All Owners, except the one who has created the Company or the one who has been the longest time in the Company, will immediately loose access
* All Admins will immediately loose accees
* All App collaborators, who are not part of the included App collaborators in the standard App plans, will loose access. The order of loosing access here again is by join date, so when an App includes three App collaborators by App plan, and four App collaborators are part of the App, the App collaborator who has joined last will loose access.



### Company collaboration in the Dashboard

The steps to invite a Company collaborator are the same as described [above](#toc-inviting-an-app-collaborator): Invite people from the Dashboard, using the "Invite someone" that can be found on the Dashboard home page, on the Company page and on the Apps overview. With Company collaboration you can now also set rules.



## Billing Contact

<!-- TODO: check with billing article: how much infos should be displayed here? -->

A Company within fortrabbit also is a container to manage billing aspects. Within one Company you can distribute the Apps across any number of Billing Contacts (see [below](#toc-billing-contact)). This enables you to separate the billing of Apps while keeping the same team.

Sometimes you have different billing preferences within one Company. That's what Billing Contacts are for. Billing Contacts are managed by the Company Owners, they consist of:

* A payment method, which can be credit card or SEPA direct debit (EU only)
* A billing address, which will be written on the invoice
* A billing e-mail address, to which the invoices will be send to

Each App (except trial Apps) has to be associated with a Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing FAQ](/billing#toc-faq).

### Creating a Billing Contact

You can create any number of Billing Contact you want free of charge: In the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button

### Deleting a Billing Contact

Sorry, it is not possible to delete a Billing Contact, because they are bound to previously created invoices. You can, of course, delete or move all Apps away from the Billing Contact so that it becomes inactive and no more bills will be sent to it. However, as the Billing Contact also contains an archive of previously issued invoices, you can always access it.





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




## Advanced collaboration usage

Still reading? Cool go on to dive even deeper.

### Working with multiple Companies

Just like in real life, you can create or join any number of Companies with your Account. So one Account can own multiple Companies while also being involved in other Companies as Admins or even App level collaborators.


### Working with the same person in different Companies

When you have multiple Companies and you want to collaborate with same person on all or a subset of them: Just invite them to each Company. The same person then will have access to all Companies while logged in with their Account.

### Moving an App from one Billing Contact to another

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company.

#### Changing the Billing Contact of an App

1. Go to the App in the Dashboard, click the "Change ownership" button
2. This will open a form, where you choose a different Billing Contact

This action can only be executed by Owners or Admins of a Company. The App will then be billed on the old Billing Contact until that day of change and from the next day on to the new Billing Contact. For example: If you move your App on the 15ths of a month to the Billing Contact of another Company, it will be billed until the 15ths to the old Billing Contact and starting from the 16ths to the new Billing contact.


### Moving an App from one Company to another

In some cases you might want to move the App to a different Company. This will change billing AND team. Of course you first need to be at least Admin in two Companies.

#### Changing the Company of an App

2. Go to the App in the Dashboard, click the "Change ownership" button
3. This will open a form, where you choose a different Company (and Billing Contact)

This action can only be done by Accounts who have Owner or Admin privileges on both Companies.

<!--  TODO: Confirm/test this: is it REALLY only possible to move an App to another Company, when I am only Collab in the other Compnay?  -->


### Moving a Company member to another Company

Well, that's not what you do, actually. What you'll do is: invite the same person again to the another Company. The Account will then be able to see and access Apps from both Companies under one login. The Account itself or you (as the Admin or Owner) can then revoke access to the first Company.



### Promoting and demoting people

Just like in real life the roles of Accounts inside a Company can change like so:

* Accounts can not downgrade or upgrade themselves to another role
* Admins can promote App collaborators to become Admins
* Owners can promote App collaborators to become Admins or Owners
* Owners can promote Admins to become Owners
* Owners can demote Admins to become App collaboratoes

Role changes have immediate effect, they do not require another confirmation by the affected Account.

<!-- TODO: maybe confirmation for promoting? when a BAD owner want's to leave he just promotes an empty DEV and is out  -->

#### Change the role of a collaborator

When Company collaboration is active and you are either Owner or Admin, you can promote and demote other people like so: Visit the Company the Account is part of, click on the role of Account. This will open a form in which you can change the role of the Account.

#### Job change confirmation







### When people leave

The Account who leaves a Comoany will loose the ability to see and edit the App in the Dashboard and also will loose all personal code access to the App. There are two directions of leaving a Company:

#### Active: An Account leaves a Company

You might want to leave the Company when the project you have collaborated on has ended or you are actually leaving the Company in real live as well. Each Account can leave a Company at any time.

##### Activly leave a Company

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > "Leave Company" button

There is one special case in which you can not leave a Company and that is when you are the last Owner. A Company must have at least one Owner.


#### Passive: An Owner or Admin terminates Company access from another Account

Let's say a project phase has ended, so that an App Collaborator does not need have access any more. Who can can fire whom:

* An Owner can make Admins and App collaboratores leave
* An Admin can make App collaborators leave
* An App Collaborator can make nobody leave
* No one can make an Owner leave

##### How to fire people

In the Dashboard > "Your Account" > "Companies" > {{ Company Name }} > Under Collaborators > Click the role of the Account you would like to fire. This will bring up a form where you can choose to completly revoke access.

<!-- TOOD: recheck this when Dashboard is done  -->


#### Confirmation on cancellations

To make the process of leaving transparent, all involved parties will get a confirmation mail about the action. All Owners of the Company will be informed about someone leaving. The person who left or was left will also get a confirmation.

<!-- TODO: be more specfic about demotion and promotion as well here, who will get a mail on promotion and demotion? -->


#### A note on security when people leave

Please mind that the person who has left still might hold local code copies and have other senstive data. We advice to reset service passwords. Please see the [security article](security#password-reset) for more.

<!-- TODO: make link to security section above work  -->





- - -

## FAQ

### How can I work on behalf of my non-techie client?

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
