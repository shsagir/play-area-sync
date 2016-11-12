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

1. **[App level collaboration](/app-collaboration)**: Collaborators have access on per-App basis
2. **[Company level collaboration](company-collaboration)**: Company members act on behalf of the Company

Both levels work side by side and can be combined. Also check out our [fancy explaining video](http://help.fortrabbit.dev/teamwork-video).



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
