---

template:      article
reviewed:      2016-08-04
title:         All about billing, pricing, payments, invoices & taxes
naviTitle:     Billing
lead:          Our consumption based pricing model explained.
group:         Kitchen_sink

seeAlsoLinks:
    - collaboration
    - scaling
    - terminology

tags:
    - beginner

---

## Free trial

You can test fortrabbit before you pay a dime. All you need to do is: sign up to fortrabbit and try it for free.  Of course: You don't need to opt-out, but you can, of course, opt-in.

## Pay after usage

No upfront costs nor payment! **First you book a component, then you'll use it, then we'll invoice you** for what you have actually used.

## Cancel anything at any time

You can always quit. Scale down or kill certain Apps or even cancel your Account completely.

## Monthly invoices

Within the first days of every new month you will get the invoice for your last month's consumption. You can also download all your previous invoices in the Dashboard.

## Daily billing cycle

When you book a Component on the 16th day of a month and the month has 30 days you only pay for the 15 days you're using the product. The minimum billing period is one day. For paid support this rule does not apply! Support plans are billed in full calendar month increments.

## Plans & prices

The service structure is modular. Most Components come in different size and flavors, you can combine anything as needed. Please see our **[pricing page](https://www.fortrabbit.com/pricing)** for an up-to-date pricing & product table.

## Storage & traffic limits

Each App includes a fair amount of web storage and traffic. These are soft limits, small exceeding and peaks will be tolerated. See our [specs page](https://www.fortrabbit.com/specs) for over-traffic costs.

## Costs monitoring

Log in to the Dashboard > Billing Contact. There you'll find a cost preview for the ongoing month, covering all of your Apps.

## Payment methods

You can pay by **credit card** (Visa & Master, no debit cards) or **SEPA direct debit** (EU clients only). For "enterprise clients" (large volume) we also offer to pay by **bill** (on account, after invoice).


## Billing mail address

You can setup a different e-mail address for each Billing Contact.

## Taxes

fortrabbit is a B2B hosting solution, it's for entrepreneurs only, we expect you to use it professionally. While that can mean anything and nothing it has some fiscal implications:

* Our prices are shown as net prices without Value Added Tax (VAT).
* If you are a non-VAT registered company in a EU country, we add VAT.
* If you are a VAT-registered company in a EU country and add your valid VAT IN, we don't charge you VAT.
* If you are a VAT-registered company in Germany, we add VAT, but you will get it back from your tax-office.
* If you are a company from a NON-EU country, we don't charge you VAT.

### VAT-IN from clients in EU countries

We recommend to enter your VAT-IN with your Billing Contact. This way you save upfront costs for VAT — reverse charge makes this possible.


## Billing and collaboration

```nohighlight
┌─────────────────────────────────────────────────────┐
│                       Company                       │
│                                                     │
│    Apps           Users           Billing Contacts  │
│  ┌─────────────┐ ┌──────────────┐ ┌──────────────┐  │
│  │ {{app-one}} │ │  {{owner1}}  │ │ {{Billing1}} │  │
│  ├─────────────┤ ├──────────────┤ ├──────────────┤  │
│  │ {{app-two}} │ │  {{owner2}}  │ │ {{Billing1}} │  │
│  ├─────────────┤ ├──────────────┤ └──────────────┘  │
│  │{{app-three}}│ │{{developer1}}│                   │
│  └─────────────┘ ├──────────────┤                   │
│                  │  {{admin1}}  │                   │
│                  └──────────────┘                   │
│                                                     │
└─────────────────────────────────────────────────────┘
```

The above illustrates the hierarchy/structure of billing and collaboration features. Your Company owns the Apps. The Company can have multiple users (employees) to collaborate on the Apps. The Company also have multiple Billing Contacts. Each App must be associated to one Billing Contact.


##  FAQ

### Why am I still charged for this?

We get a few e-mails each month after invoicing. People are confused why they still see Apps on the invoice that have been deleted.

Usually you pay upfront for stuff you book online. You want something, you pay for it. Due to the above described fair pricing, upfront payments are not possible with fortrabbit.

fortrabbit has a consumption based billing. The minimum billing period is one day. That means we charge after usage. Just like with your old telephone bill. At the end of the month you'll get an invoice for what you have used in that month.

So, when you start using an App on the 15th of Jan, you'll be invoiced at the end of Jan for using the App for half a month.

So at the start of each month you'll get an invoice for the stuff you have booked last month. Also look at the invoice period.


### What if I cancel my Account?

We will delete all your Account data. But it is required by law to keep your billing related data, that is basically the invoices and infos around it. The same applies to your credit card informations, but those are stored with our credit card service merchant — not with us. 

And yes, if you cancel the service while still having open invoices with us, they stay open and we might take further legal steps to finally get that money.


### What is this €15 pending transaction?

We do a debit test on your credit card, when you enter the informations with us. This should not show up on your credit card statements, as it just a test. We have heard from clients that it sometimes shows up. Please don't worry, it will not be processed.



### What if my credit card has not enough credits?

Then it will bounce — which costs us a lot of money each month. We might pass those costs to the clients in the future. We will inform you about bounced payments. We also try to debit again.


### What if my credit card expires?

We will inform you. Please add your new credit card details to your Billing Contact.


### What if I have open invoices with fortrabbit?

You can pay your open invoices in the Dashboard yourself like so: Login to the Dashboard > follow the instructions on the "open invoices" warning. Credit card payments are done "live". SEPA direct payments are delayed. When your credit card didn't had enough balance at first, we might try again during the month.

Please contact us when you are aware that you habe open invoices with us. Otherwise we might delete your Apps. See [this story](https://blog.fortrabbit.com/bounced-payment).


### Can I pay yearly?

Sorry, as there are no upfront payments, this is not possible.


### Where can I see my current costs?

You can like so: Dashboard > Your Account > Your Company Name > Billing Contact / Current costs. This overview shows you what will be printed on the next invoice. Please mind that it will show the current costs of the day of the month so far. 



### How can I create a new Company?

In the Dashboard > Companies > {{your-company}} > "Add a new Company" > Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Company, no costs, as long as you don't assign an App to the Company. You will be Owner of that Company, hence you created it.

Do this when you want certain Apps to have a different team and probably different billing.


### How can I create a new Billing Contact?

In the Dashboard > Companies > {{your-company}} > "Add a new Billing Contact" > Follow the steps. You will be asked for an invoice address and a payment method. This will give you an empty Billing Contact, no costs, as long as you don't assign an App to the Billing Contact. You must be Owner of the Company for this.

Do this when you want certain Apps to be charged from a different credit card but want to keep team and access settings for the App.

### How can I change the Billing Contact of an App?

In the [Dashboard](dashboard), go to your [App](app) and hit the "Change ownership" button. Here you can change the Company (and the [team](collaboration)), as well as the Billing Contact (only affects billing). You must be Owner of the Company for this.


### How can I edit the Billing Contact?

When you only have one App or you want to have all your Apps under the same Billing Contact and you want to change that, you can also edit the Billing Contact. In the Dashboard > Companies > {{your-company}} > either edit the Payment method or the Invoice address.



