---

template:    article
reviewed:    2018-03-13
title:       Some words on security
naviTitle:   Security
lead:        fortrabbit security concepts and how you can help.
group:       platform
stack:       all

keywords:
    - beginner
    - dashboard

---


»We take security very seriously.« Isn't that what everybody is saying? It's our business to keep your business online. Our clients trust us with that. We know what we do and we have been doing hosting for over ten years now. To help paint a picture of our experience, have a look at our: [history](http://www.fortrabbit.com/about), [blog](http://blog.fortrabbit.com/), [tweets](https://twitter.com/fortrabbit), [past incidents](http://status.fortrabbit.com) or ask our customers directly.

## Our responsibilities

### Physical infrastructure

fortrabbit utilizes Amazon Web Services (AWS) data centers. Amazon data centers are compliant with and certified under a number of security standards, including ISO 27001. AWS enforces a high level of physical security to safeguard their data centers. Among other features, they enforce two-factor authentication for all their authorized staff members, military grade perimeter controls and security staff at all points of ingress.

As for environmental protection, AWS has sophisticated fire detection and suppression equipment, fully redundant power infrastructure with integrated UPS units and high-end climate control systems to guarantee an optimal working environment for the hardware.

For a more in-depth view, we refer you to the [AWS Security Center](https://aws.amazon.com/security).

### SysOp level

fortrabbit employs a multi-tier security strategy.

On the inside, each Node is built around a hardened Linux kernel, which enforces strong privilege and resource separation mechanisms at an OS level. All operating systems and software components are kept up-to-date by our maintenance staff and we pride ourselves in reacting quickly to all [Poodles](http://blog.fortrabbit.com/ssl-v3-disabled-poodle-vulnerability/), [Heartbleeds](http://blog.fortrabbit.com/heartbleed-openssl-vulnerability/), Shellshocks and [Ghosts](https://twitter.com/fortrabbit/status/560478509475577856) that have and will come up.


At the next tier, each node exists within isolated virtual containers, which guarantee complete logical separation of Apps on fortrabbit. In addition, the container technology allows for hard resource capping, which reduces the bad neighbor effect of shared environments to a bare minimum.

On the outside, we utilize network firewalling and hardened TCP/IP stacks to mitigate resource exhaustion attempts. Sniffing and spoofing attacks are prevented through the underlying infrastructure. Our setup is flexible and we are able to isolate or boost resources quickly.

### Credit card security

We use Wirecard — a PCI Level 1 compliant provider — for processing credit card payments.

## Your responsibilities

We are in it together! You are responsible for the code you write and use.

### Check your code

Make sure to follow [security guidelines](http://www.phptherightway.com/#security). It's good practice to perform a security check against the most common attack vectors before going live. Also mind the [OWASP Cheat Sheets](https://www.owasp.org/index.php/OWASP_Cheat_Sheet_Series) to negate attacks before they can start.

### Frameworks & CMS systems

Frequently update the frameworks and CMSs you use. When security vulnerabilities are discovered and patched, it is your responsibility to stay up-to-date. Having an outdated WordPress installation puts your site at great risk of being hacked. Composer makes updating easy for modern frameworks.

## Dashboard access

The password you use to login with the [fortrabbit Dashboard](https://dashboard.fortrabbit.com) is your master password. We recommend using a [pass-phrase](http://xkcd.com/936/) kind of password. Use something that is easy to remember for you, but hard for anyone else to guess and long enough to stand against brute force attacks.

### Two-factor authentication

We highly recommend enabling two-factor authentication (2FA) with your fortrabbit Account. You can so do in the Dashboard — you'll be guided setting it up. Our 2FA is a software implementation, which means that you'll need an extra device such as a smart phone and an extra 2FA software to generate your TOTP (time-based one time passwords).

### Session time

It's always a balance between usability and security. By default we'll log you out after some time of inactivity. You can modify that timing in the Dashboard under your Account settings.

### Re-enter Account password for critical actions

For "dangerous actions" in the Dashboard you need to re-authenticate with your Account password (and 2FA if enabled). As the fortrabbit [Dashboard](dashboard) is all about administrative tasks, this includes many tasks. So in short: we do [SUDO](http://en.wikipedia.org/wiki/Sudo). You can also modify how often you'll have to enter your SUDO password.

## Code access

We recommend storing your [public SSH](/ssh-keys) key with your fortrabbit Account. You can also install multiple keys with your Account. For instance, you could have two separate keys for your desktop and laptop. fortrabbit automatically installs your up-to-date key(s) on each App you have access to.

### Check your SSH keys

Please revisit your list of SSH keys from time to time and keep it as short as possible. Only keep those keys you are really using.

### Password reset

You can reset the fortrabbit service passwords for MySQL and Object Storage in the Dashboard with your Apps. It is recommended to reset those passwords periodically and when a Company member leaves for each App.

## Firewalling

By default all outgoing traffic on all ports, except for [standard ones](https://www.fortrabbit.com/specs#firewall), is blocked. You can request to whitelist a port or port range via the Dashboard in the settings of your App.

## Vulnerability reporting

> You can't defend. You can't prevent. The only thing you can do is detect and respond.

Bruce Schneier

Have you discovered a security issue related to fortrabbit? Please disclose it in a responsible manner. We have high regard for white hat hacking culture and will work with you to understand the scope of the issue.

To report a possible security issue, please mail: [security@fortrabbit.com](mailto:security@fortrabbit.com) using our [PGP key](/fortrabbit.pgp.asc).

**Thanks for keeping fortrabbit secure!**  Mayank Bhatodra, Salman Khan Champion [dezignburg.com](http://www.dezignburg.com), YOU?
