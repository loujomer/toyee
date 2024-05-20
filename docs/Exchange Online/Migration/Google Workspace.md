>[!faq] what can be migrated?
- Mail & Rules
- Calendar
- Contacts

>[Overview of the G Suite migration process in Exchange Online | Microsoft Learn](https://learn.microsoft.com/en-us/exchange/mailbox-migration/how-it-all-works-in-the-backend)
# prerequisites

1. Must have a **project creator role** in Google Workspace
2. Create a subdomain for mail routing to Microsoft 365 or Office 365
3. Create a subdomain for mail routing to your Google Workspace domain
4. Provision users in Microsoft 365 or Office 365

### Check Google Cloud platform permissions

>[!info] for Automated migration
>required roles:
>- Projector Creator
>- Service Accounts Creator

Required permissions for the Google Migration admin:
1. Create a Google Workspace project.
2. Create a Google Workspace service account in the project.
3. Create a service key.
4. Enable all APIs - Gmail, Calendar, and Contacts.

### Create a subdomain for mail routing to Microsoft 365 or Office 365

>performed in Google Workspace Admin page

1. add subdomain (routing domain) 
	recommended:
	* A subdomain of your primary domain (for example, "==o365==.fabrikaminc.net", when "fabrikaminc.net" is your primary domain) so that it will be automatically verified.
	
	not recommended:
	* If another domain (such as "fabrikaminc.onmicrosoft.com") is set, Google will send emails to each individual address with a link to verify the permission to route mail. Migration won't complete until the verification is completed.

2. add this subdomain (routing domain) as an accepted domain in M365

3. add an MX record of this subdomain that will point to M365

### Create a subdomain for mail routing to your Google Workspace domain

>performed in Google Workspace Admin page

1. add domain
	recommended:
	- A subdomain of your primary domain (for example, "==gsuite==.fabrikaminc.net", when "fabrikaminc.net" is your primary domain)

2. add an MX record of this subdomain that will point to Google

>[!info] Remote domain
>Remote domain must have Automatic forwarding enabled -  this will allow mail flow from o365 to gw


### Provision users in Microsoft 365 or Office 365

- create [mail users](https://learn.microsoft.com/en-us/exchange/recipients-in-exchange-online/manage-mail-users#use-the-exchange-admin-center-to-manage-mail-users)
- the primary address for each user should be at the primary domain
	- this means that the email address should match between M365 and Gmail
	- if any user has a different domain for their primary address, then that user should have a proxy address at the primary domain
- each user should have their `ExternalEmailAddress` point to the user in their Google Workspace routing domain ("will@gsuite.fabrikaminc.net").