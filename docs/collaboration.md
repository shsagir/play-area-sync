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

A Company within the fortrabbit platform represents your businesses. It has at one Billing Contact (see below) and one User (see above) associated with. It can, of course, own multiple Apps. Please also see the [billing FAQ](/billing#toc-faq).


## Levels of collaboration

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

### App collabortors permissions

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

Life is complicated and professional life even more so. [Above](#toc-app-collaboration) we have seen how App collaboration helps to collaborate on per-App-basis. Collaboration on company level goes a step further. 

Just like in real live the owners have full access, full control, full responsability and need to take care about the financials. While the admins have all the fun and can act on behalf of the Company. 

### Use cases

* Map real world business relationships
* Project based workflows with delegation
* Multi-cient capable workflows





### Roles

Each Company can have multiple Accounts associated with it:

#### Owner

The Owner role can NOT be modified by someone else, multiple Owners per Company are possible. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company
* code access to all Apps of the Company
* delete, create, change Billing Contacts for the Company
* manage all roles (but other Owners) of the Company
* invite new Users for any role to the Company
* leave the Company (if other owners are present)

#### Admin

The Admin role can be modified by Owners. Accounts with this role within a Company can:

* create, delete, configure & scale all Apps of the Company;
* code access to all Apps of the Company;
* invite new Admins or Developers to the Company;
* leave the Company.

### Collaborator

The Collaborator role can be modified by Owners and Admins. It's the same as with App collaboration and has the same permissions (see [above](#toc-app-collabortors-permissions)).

When a Company collaboration plan is booked, all 


### Booking a Company collaboration plan

<!-- TODO -->


### Downgrading a Company level plan

<!-- TODO -->


### Company collaboratorion in the Dashboard

<!--  TODO: steps to invite and edit roles are the same as decribed above -->



## Billing Contact

<!-- TODO: more details  -->

Sometimes you have different billing preferences within one Company. That's what Billing Contacts are for. Billing Contacts are managed by the Company Owners, they consist of:

* A payment method
* A billing address
* A billing e-mail address

Each App (expect trials) has to be associated with a Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing FAQ](/billing#toc-faq).



## App alerts

<!-- TODO -->



## Advanced usage

### Working with the same person in different Companies

When you have multiple Companies and you want to collaborate with same person across them: Invite them to each Company. The same person then will have access to all Companies under one Account.

### When people leave

<!--  TODO: 

* two ways to leave: self-initiated, by higher hierachy
* remind of concept: a user leaves access is gone automatically
* hint & link to service password reset for extra security

-->


<!--  TODO: more advanced topics here -->



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


