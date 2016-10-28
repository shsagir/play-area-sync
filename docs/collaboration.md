---

template:      article
reviewed:      2016-10-28
title:         Teamwork on fortrabbit
naviTitle:     Collaboration
lead:          Learn about the powerful collaboration features and how to map your real world team relationships on fortrabbit.
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


## About Account access

```
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

In classical hosting you have one set of server access credentials for SFTP/SSH which you then have to share. With fortrabbit this is different: each Account has it's own personal access to each App.

So instead of sending an e-mail with the FTP password, you are sending an invitation to create a free Account and to get instant personal access on your Apps. This is more secure, more transparent and easier to handle.

Also see the [access methods article](/access-methods) to learn about the different ways (SSH key or password) to access code.




## Levels of collaboration

```
┌────────────────────────────────────────────────────────┐                               
│                                                        │                               
│                                                        │  ****                         
│                                                        │     *                         
│                        Company                         │     *                         
│                                                        │     *                         
│                                                        │     *                         
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
└─────────────────┘  └────────▲────────┘ └─────────────────┘   *  Simple                 
                              │                                *  App level              
                              │                                *  collaboration          
                       ┌──────┴─────┐                          *  where an Account       
                       │            │                          *  has access to a        
                       │  Account2  │                          *  single App             
                       │            │                          *                         
                       └────────────┘                       ****                         
```

To best map real world scenarios to collaborate on your projects two level of collaboration are available: 

1. [Basic App level collaboration](#toc-basic-app-level-collaboration) to quickly work with other developers on per-App basis
2. [Company level teams](#toc-company-level-collaboration) delegate your crew members to act on behalf of the Company

Both levels work side by side and can be combined.



## Basic App level collaboration

This is 

* Limted App collaborators for Universal stack Apps (see [pricing](https://www.fortrabbit.com/pricing) for details)
* Unlimted App collaborators for Professional stack Apps






## Company level collaboration

Most of our clients run multiple Apps on fortrabbit. But here is the thing: Life is complicated and professional life even more so. Say we have [[App X]], which was once part of [[Project A]] but is now not anymore. And actually you don't want to pay it, but your client [[Acme Inc]] should. And not to mention: [[Kevin Flynn]], the guy your client hired for code review, also should have code access.

Well, you just could set up a new Account for each App and be done with it. But the more Apps you have, the more of a mess you will end up with: no bird's eye view about what is going on, logout/login to switch to another App, manage all kind of stuff twice, ...


The solution is a multi client model that maps to your real world in nearly any context: freelancer, startup, digital agency.


### Understanding Account & Company



An Account in fortrabbit represents a person — You. An Account can login to the Dashboard, manage Apps and Companies. An Account can own or be part of multiple Companies. That is useful for example when you use fortrabbit privately for your week-end projects and for business. Or if you as a freelancer collaborate with various digital agencies.

A Company within the fortrabbit platform represents your businesses. It has at one Billing Contact (see below) and one User (see above) associated with. It can, of course, own multiple Apps. Please also see the [billing FAQ](/billing#toc-faq).




## User

When you sign up with fortrabbit you'll be asked for your first and your last name. You see, we expect that a user account represents a human being, not your company, not even when you are the one-man-army. Don't share your fortrabbit user account, it's your "private" one. Here you'll deposit YOUR own [public SSH keys](/ssh-keys), that's your very own access to the [Dashboard](/dashboard).


## User roles

It also works around the other way, not only you can take part in multiple Companies, each Company can have multiple Users associated with it. With permission based access roles we make sure only the boss can really mess up. Every User has an associated within each Company:

### Owner

The Owner role can NOT be modified by someone else, multiple Owners per Company are possible:

* create, delete, configure & scale all Apps of the Company;
* code access to all Apps of the Company;
* delete, create, change Billing Contacts for the Company;
* manage all roles (but other Owners) of the Company;
* invite new Users for any role to the Company;
* leave the Company (if other owners are present).

### Admin

The Admin role can be modified by Owners, hir can:

* create, delete, configure & scale all Apps of the Company;
* code access to all Apps of the Company;
* invite new Admins or Developers to the Company;
* leave the Company.

### Developer

The Developer role can be modified by Owners and Admins, hir can:

* configure certain Apps of the Company;
* code access certain Apps of the Company.

## Billing Contact

Sometimes you have different billing preferences within one Company. That's what Billing Contacts are for. Billing Contacts are managed by the Company Owners, they consist of:

* A payment method
* A billing address
* A billing e-mail address

Each App (expect trials) has to be associated with a Billing Contact. Each Billing Contact has its own invoice archive. Please also see the [billing FAQ](/billing#toc-faq).


## How to use it

Don't fear. It's not as complicated as it may sounds. All team features are optional, the [Dashboard](dahsboard) makes managing all this easy. See our [teamwork video](teamwork-video)!

## Further readings

* [Introduction post on medium](https://medium.com/@frank_laemmer/our-multi-client-model-3b965d2f1060)

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


