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

TODO: this article is toooo long. It is not ment to be read from top to down, more like a reference, still it might be possible to break it up in various other articles — a dedicated Company Article for example

-->


Most of our clients are running three and more Apps on fortrabbit. Say we have [[App X]], which was once part of [[Project A]] but is now not anymore. And actually you don't want to pay it, but your client [[Acme Inc]] should. And not to mention: [[Kevin Flynn]], the guy your client hired for code review, also should have code access.

Well, you just could set up a new Account for each App and be done with it. But the more Apps you have, the more of a mess you will end up with: no bird's eye view about what is going on, logout/login to switch to another App, manage all kind of stuff twice, ...

You want to easily add new members to all of your team’s projects. The solution is a multi client model that maps to your real world in nearly any context: freelancer, startup, digital agency.


## Understanding the Account

When you sign up with fortrabbit you'll be asked for your first and your last name. We expect that an Account represents a human being, not a Company. An Account is a person who can login to the Dashboard, manage Apps and Companies. An Account can own or be part of multiple Companies. That is useful for example when you use fortrabbit privately for your week-end projects and for business. Or if you as a freelancer collaborate with various digital agencies.

Don't share your fortrabbit Account, it's your "private" one. Here you'll deposit YOUR own [public SSH keys](/ssh-keys), that's your very own access to the [Dashboard](/dashboard).

### Personal Account access to Apps

In classical hosting you have one set of server access credentials for SFTP/SSH which you then have to share. With fortrabbit this is different: each Account has it's own personal access to each App.
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


So instead of sending an e-mail with the FTP password, you are sending an invitation to create a free Account and to get instant personal access on all Apps. This is more secure, more transparent and easier to handle. Also see the [access methods article](/access-methods) to learn about the different ways (SSH key or password) to access code.


## Understanding Companies

A Company within the fortrabbit platform represents your business. When your business is very small you won't even notice the difference between your Account and your Company. So when you have one App and you are working solely on that or with an App collabartor your Account and the Company are nearly the same. 

But when your business get's bigger you'll be happy to learn that a Company on fortrabbit can do much more for you. Manage the team 

### Team features

Within your Company you can manage your team — invite and remove collaborators, change the role of Company members and add or remove Apps from App collaborators.


### Billing features

A Company within fortrabbit also is a container to manage billing aspects. Within one Company you can distribute the Apps across any number of Billing Contacts (see [below](#toc-billing-contact)). This enables you to separate the billing of Apps while keeping the same team.


### Working with multiple Companies

Any-to-many relation: Just like in real life, you can create or join any number of Companies with your Account. So one Account can own multiple Companies while also be envolved in other Companies in different other roles. This way you can collaborate with different teams.


## Levels of team collaboration

```
┌────────────────────────────────────────────────────────┐                               
│                                                        │                               
│                                                        │  ****                         
│                                                        │     *                         
│                        Company                         │     *                         
│                                                        │     *                         
│                                                        │     *  Level 2
│                                                        │     *  Advanaced              
└─────▲─────────┬─────────────┬────────────┬────────▲────┘     *  Company level          
      │                                             │          *  team cooperation       
    Owner       │             │            │      Admin        *  where Owners & Admins  
      │                                             │          *  delegate on behalf     
┌─────┴──────┐  │             │            │ ┌──────┴─────┐    *  of the Company         
│            │                               │            │    *                         
│  Account1  ├──┼────────┐    │    ┌───────┼─┤  Account3  │    *                         
│            │           │         │         │            │    *                         
└─────┬──────┘  │        │    │    │       │ └──────┬─────┘    *                         
      │                  │         │                │          *                         
      │         │        │    │    │       │        │          *                         
      │                  │         │                │          *                         
┌─────▼─────────▼─┐  ┌───▼────▼────▼───┐ ┌─▼────────▼──────┐   *                         
│                 │  │                 │ │                 │ ***                         
│      App1       │  │      App1       │ │      App1       │                             
│                 │  │                 │ │                 │ ***  
└─────────────────┘  └────────▲────────┘ └─────────────────┘   *  Level 1                 
                              │                                *  Simple              
                              │                                *  App level          
                       ┌──────┴─────┐                          *  collaboration       
                       │            │                          *  where an Account        
                       │  Account2  │                          *  has access to a
                       │            │                          *  single App
                       └────────────┘                       ****                         
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
* CAN NOT invite other collaborators
* CAN NOT scale an App
* CAN NOT delete an App
* CAN NOT change settings of an App in the Dashboard

### App collaboration use cases

* Simple App sharing to get started with collaboration
* Sharing the wekend project code with colleague to help debugging
* Freelancer coworker get's access on one project
* Contract worker to help out with a project



### Inviting an App collaborator

1. In the [Dashboard](/dashboard) overview: Click the "Invite someone" button
2. First enter name and e-mail of your colleague
3. When you are part of multiple Companies, you will be asked for the Company to invite to
4. Next you will be asked, which kind Level of Collaboration you want, choose "App collaboration" here now
5. In the last step you can choose between Apps you want your colleague to be invited to

This will send an invitation e-mail. Your colleague can then deny by just ignoring the e-mail or accept the invitation by logging in or signing up.

You can start this procedure also from your Company or from your App. This will shorten the steps, as you have already made a selection by chosing a start for it.


### Removing an App collaborator

You can reach the User App colllaborator access form in two ways:

1. **From the Company:**  In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role.
2. **From the App:** In the Dashboard: Navigate to the App the Collaborator is part of, under Team access click on the collaborators access role. 

Within this form you can add or remove Apps for this Collaborator.


### Promoting an App collaborator

When you already have a Company collaboration going on you can promote a Collaborator to become Admin or Owner. In the Dashboard: Navigate to the Company the App is part of. Under App collaborators, click on the collaborators access role. This will open a form where you can change the role.

You can also reach the Collaborators profile page from your App overview.


### Assigning more Apps to the same App collaborator

You can of course also give the same Account collaborating on App level access to multiple of your Apps. In the Dashboard: Navigate to the Company the Collaborator is part of, under App collaborators, click on the collaborators access role. There you can select all Apps with enabled collboration for this collaborator.


### App collaboration availability

App collaboration may or may not be available for your App:

* limited App collaborators for different Universal stack Apps, see [specs page](https://www.fortrabbit.com/specs).
* unlimited App collaborators for all Professional stack Apps
* unlimited App collaborators for all Apps when a Company level plan is booked








## Company level collaboration

Life is complicated and professional life even more so. [Above](#toc-app-collaboration) we have seen how App collaboration helps to collaborate on per-App-basis. Collaboration on Company level goes a step further. 

Just like in real live the owners have full access, full control, full responsability and need to take care about the financials. While the admins have all the fun and can act on behalf of the Company. 

### Use cases

* Map real world business relationships
* Project based workflows with delegation
* Multi-cient capable workflows
* Agency and startup style collaboration

### Creating a Company

You can create as many Comapanies you want — free of charge. In the the Dashboard > Your Account > Companies > "Create a Company" button.

In the following steps you will create a Company with a first Billing Contact. Each Company needs at least one Owner, you are the one at first.

* [dashboard.fortrabbit.com//account/company/new](https://dashboard.fortrabbit.com//account/company/new) < direct link


### Deleting a Company

As an Owner you can also decide to delete a Company. This is a huge step, as all Apps owned by the Company will be deleted and all the team will loose access. To do so: In the Dashboard > Your Account > Companies > {{ Your Company }} > Hit the "Delete Company" button and follow the instructions.



### Booking a Company collaboration plan

Company collaboration is a paid feature, see our [fancy marketing page](https://www.fortrabbit.com/collaboration) for plans and prices.

Collaboration plans — like [professional support plans](https://www.fortrabbit.com/collaboration) — are optional and mapped to a Company. Like any other resource with fortrabbit, they are billed on a precise daily settlement. 

To book a Company collaboration plan: In the Dashboard > navigate to your Account > Companies > {{ Your Company }} > and there to "Company collaboration", this will show you a screen to book a plan. This way you can also upgrade and [downgrade](#toc-downgrading-a-company-level-plan) the Company collaboration plan.


### Roles

A unique selling point with Company collaboration are role based permissions. Delegation is the key. Only with Company collaboration members with the according roles will get automatic access to newly created Apps and members with according rights can create Apps on behalf of the Company. Each Company can have multiple Accounts associated with it:

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

The Collaborator role can be modified by Owners and Admins. It's the same as with App collaboration and has the same permissions (see [above](#toc-app-collabortors-permissions)). But when a Company collaboration plan is booked, all Universal stack Apps have unlimited App collaborators.


### Downgrading a Company level plan

To disable Company collaboration, visit the Company collaboration booking page (see [above](#toc-booking-a-company-collaboration-plan)) again and disable it. This will have the following consequences: 

* All Owners except the one who has created the Company or the one who has been the longest time in the Company will immediatly loose access
* All Admins will immdediatly loose accees
* All App collaborators that are not part of the included App collaboratoes in the standard App plans will loose access. The order of loosing access here again is by join date, so when an App includes three App collaborators by App plan, and four App collaborators are part of the App, the App collaborator who has joined last will loose access.



### Company collaboratorion in the Dashboard

The steps to invite a Company collaborator are the same as described [above](#toc-inviting-an-app-collaborator). The difference now is: you can also choose a role.



## Billing Contact

<!-- TODO: check with billing article: how much infos should be displayed here? -->

Sometimes you have different billing preferences within one Company. That's what Billing Contacts are for. Billing Contacts are managed by the Company Owners, they consist of:

* A payment method < credit card or SEPA direct debit
* A billing address < written on the invoice
* A billing e-mail address < where the invoices should be send to

Each App (expect trials) has to be associated with a Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing FAQ](/billing#toc-faq).

### Creating a Billing Contact

You can create any number of Billing Contact you want free of charge: In the Dashboard > Your Account > Companies > {{ Your Company }} > "New Billing Contact" button

### Deleting a Billing Contact

Sorry, it is not possible to delete a Billing Contact. You can of course delete or move all Apps away from the Billing Contact so that it get's intactive and no more bills willl be sent to it. But as the Billing Contact also contains the invoice archive you should always access to that, it's not a good idea to delete the Billing Contact.


## App alerts

Each Company can has one or many technical contacts which will be used for automatic alerting when App resources are about to exceed and in special cases like where we to interact manually need to contact you personally quickly — for instance when we detect abnormal patterns.


### Setting a technical contact

In the Dashboard under your Account > Companies > {{ Company }} > App alerts you can set one or more contacts. This can be any Account associated with the Company or any e-mail address you'll like.

### Automatic resource consumption alerts

Each App with fortrabbit has limited resources, most of them are scalable or extendable. We advice to start small and only scale when needed. Some resources scale by usage, when your App has users who are uploading images, the web space or the Object Storage space will get fuller and fuller.

You will see those perfomance metrics with the App overview in the Dashboard. When a resource is about to exceed, it will be shown in a yellow color to alert you. But of course nobody is monitoiring it's Apps in the Dashboard all the time. That's where the App alert e-mails come in.

<!--  TODO: decide what about Play App plan, which originally does not include Performance Metrics at all - does that really makes sense? If really yes, describe here that not all Apps include Performance metrics. -->

The technical contact(s) will get an e-mail, once a metric is about to exceed or already exceeded it's limit. Please see our [specs page](https://www.fortrabbit.com/specs#limits) to learn about the resources with App alerts and how the limits are handled.

<!-- TODO: resources, App overwrites … -->




## Advanced collaboration usage

Still reading? Cool go on to dive even deeper.

### Working with the same person in different Companies

When you have multiple Companies and you want to collaborate with same person across them: Invite them to each Company. The same person then will have access to all Companies under one Account.

### Moving an App from one Billing Contact to another

In case you want the App to be billed from another credit card or bank account but still want to keep the team of the App the same: You simply change the Billing Contact of the App. You first need to have at least two Billing Contacts with your Company. 

<!-- TODO: link on how to create a Billing Contact above/below/billing —— alternative inline verison: In the Dashboard > Your Account > Companies > {{ Company }} > New Billing Contact. This will open a multi-step form to create a Billing Contact. -->

To change the Billing Contact of an App:

2. Go to the App in the Dashboard, click the "Change ownership" button
3. This will open a form, where you choose a different Billing Contact

This action can only be done by an Owner of a Company. The App will then be billed on the old Billing Contact until that day and from the next day on from the new Billing Contact.


### Moving an App from one Company to another

In some cases you might want to move the App to a different Company. This will change billing AND team. Of course you first need to be at least Admin in two Companies.

To change the Billing Contact of an App:

2. Go to the App in the Dashboard, click the "Change ownership" button
3. This will open a form, where you choose a different Company (and Billing Contact)

<!--  TODO: Confirm/test this: is it REALLY only possible to move an App to another Company, when I am only Collab in the other Compnay?  -->


### Moving a Company member to another Company

Well, that's not what you do, actually. What you'll do is: invite the same person again to the another Company. The Account will then be able to see and access Apps from both Companies under one login. The Account or yourself revoke any access from the first Company.


### When people leave

The Account who leaves a Comoany will loose the ability to see and edit the App in the Dashboard and also will loose all personal code access to the App. There are two directions of leaving a Company: 

#### Active: An Account leaves a Company

You might want to leave the Company when the project you have collaborated on has ended or you are actually leaving the Company in real live as well. Each Account can leave a Company at any time. To activly leave a Company:

In the Dashboard > Your Account > Companies > {{ Company Name }} > "Leave Company" button

There is one special case in which you can not leave a Company and that is when you are the last Owner. A Company must have at least one Owner.


#### Passive: An Owner or Admin terminates Company access from another Account

Let's say a project phase has ended, so that an App Collaborator does not need have access any more. You as an Owner or Admin can now revoke access like so:

<!-- TODO. write about how to revoke access  -->

Please mind that only a higher role can make a lower role leave:

* An Owner can make Admins and App collaboratores leave
* An Admin can make App collaborators leave
* An App Collaborator can make nobody leave
* No one can make an Owner leave


### A note on security when people leave

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


