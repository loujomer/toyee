### Email Address Policies
* only available in on-premise Exchange
* although EXO has a default address policy
	` Get-EmailAddressPolicy | fl

* can be used with O365 groups [only](https://learn.microsoft.com/en-us/microsoft-365/solutions/choose-domain-to-create-groups?view=o365-worldwide&redirectSourcePath=%252fen-us%252farticle%252fMulti-domain-support-for-Office-365-Groups-7cf5655d-e523-4bc3-a93b-3ccebf44a01a)
	`[PS] C:\> New-EmailAddressPolicy –Name MarketingGroups –IncludeUnifiedGroupRecipients –EnabledEmailAddressTemplates "SMTP:@Marketing.Office365ITPros.com", "smtp:@anotherdomain.com" - ManagedByFilter {Department –eq "Marketing"} –Priority

* catch all policy to assign groups with a specific domain
	`[PS] C:\> New-EmailAddressPolicy -Name AllOtherGroups -IncludeUnifiedGroupRecipients -EnabledPrimarySMTPAddressTemplate "SMTP:@Office365ITPros.com" -Priority 3

* update email address policies
	`[PS] C:\> Set-EmailAddressPolicy -Identity AllOtherGroups -EnabledPrimarySMTPAddressTemplate "@Office365ITPros.com"


### SMTP Addresses and Mail-Enabled Objects

* add a new smtp address
	`[PS] C:\> Set-Mailbox –Identity TRedmond –EmailAddresses @{Add="Tony.Redmond@Office365ITPros.com"}

* set the primary SMTP and others (will replace existing smtp)
	`Set-Mailbox -Identity "Alexander Hart" -EmailAddresses SMTP:Alexander.Hart@gwapahon.onmicrosoft.com,Alexander@mstestavaceblobor.onmicrosoft.com,Alexander@mgashufa.onmicrosoft.com

* change the primary SMTP by changing `WindowsEmailAddress
	`Set-Mailbox –Identity "Alexander Hart" –WindowsEmailAddress Alex@mstestavaceblobor.onmicrosoft.com

* for M365 groups, you can't use `WindowsEmailAddress`, so you have to use `PrimarySmtpAddress
	`Set-UnifiedGroup -Identity fdfd -PrimarySmtpAddress wayolark@mstestavaceblobor.onmicrosoft.com

* check recipients with more than 400 email proxy
	`Get-Recipient | Where {($_.EmailAddresses).count -gt 400} | Format-Table DisplayName, Alias, RecipientType


### The SPO Proxy Address

This is an address that is automatically created by SharePoint Online when the mailbox participates in sharing operations involving SharePoint Online or OneDrive for Business sites, including those used with Office 365 Groups.

It allow [SharePoint Online]((https://support.microsoft.com/en-us/office/information-about-changes-to-the-address-that-is-used-to-send-notification-email-messages-from-sharepoint-ae01f2a0-acca-499b-ab96-df0e996d367a?ui=en-us&rs=en-us&ad=us)) to use Exchange Web Services (EWS) to impersonate the user to send messages on their behalf.

These messages will have the same display name (such as the person who is doing the sharing), but will always use no-reply@sharepointonline.com as the **From** address.


### Address Lists

###### Global Address List (GAL)
* includes all mail-enabled recipients

###### EXO standard address lists:
1. All rooms: All room mailboxes.
2. All users: All mailboxes – including, shared, and resource (room and equipment) mailboxes (but not Office 365 group mailboxes).
3. All distribution lists: All standard and dynamic distribution groups, including Office 365 Groups.
4. All contacts: All mail-enabled contacts.
5. All groups: All Office 365 groups. The recipient filter still refers to these objects as “group mailboxes”.
6. Offline Global Address List: The GAL as downloaded and made available for offline use by Outlook clients. Mail-enabled public folders are included in this address list. 
7. Public Folders (an address list generated on-premises when in a hybrid deployment)

* creating new Address lists has to be done with PowerShell

* qurery used to locate the desired objects in an Address list
	`Get-AddressList –Identity "Offline Global Address List"

###### RBAC permission
“Address Lists” management role

>[!tip] New address list not updated

Because Exchange Online does not support the Update-AddressList cmdlet, objects that already exist might not show up in the new address list. 

The reason is that address list membership is evaluated when an object changes, so any recipients that should be in the list already exist in the directory will not appear until the next time that their object is updated (by EAC, Office 365 Admin, or PowerShell). 

Microsoft says that this is “by design” and it’s understandable in some respects because Microsoft clearly wants to avoid processing operations that could be resource-intensive and impact multiple tenants.

If you want to be sure that a new address list is correctly populated, you have to force this by updating all of the objects that should be in the list.

**Sample Script:**

`[PS] C:\> $Filter = (Get-AddressList –Identity "Ireland Users").RecipientFilter 

`[PS] C:\> Get-Recipient –RecipientPreviewFilter $Filter | Set-Mailbox –CustomAttribute6 "Updated"

>[!tip] Create a recipient filter

One workaround is to create a dynamic distribution group with a filter for the same recipient set that you’d like to use in an address list and then reuse the filter. You can extract the filter for a dynamic group to a variable with a command like:

`[PS] C:\> $Filter = (Get-DynamicDistributionGroup –Identity "Dynamic User").RecipientFilter

Then, use the $Filter variable as input to either the `New-AddressList` or `Set-AddressList` cmdlets to create a new address list or modify an existing address list


### Offline Address Book (OAB)

* Outlook clients configured to use cached Exchange mode use the OAB for address validation and directory lookup
* an Office 365 tenant cannot force the generation of the OAB 
	* must wait for Exchange Online to run the OAB generation process, which is sometimes delayed due to server load
	* Offline address books are **generated every 8 hours.**
* Users can force an OAB update by selecting Download Address Book from Outlook’s Send/Receive menu, but the client can only download the available OAB updates, ==which might not include new recipients.==
* Exchange Online does not allow you to assign a different OAB to a mailbox by running the Set-Mailbox cmdlet. 
* In addition, you cannot make a new OAB the default OAB. 
* The only way that a customized OAB can be provided to a user is through Address Book Policies.


### User Photos and the OAB

`[PS] C:\> Set-UserPhoto "Ben Owens" -PictureData ([System.IO.File]::ReadAllBytes("C:\Temp\BenOwens.jpg")


### Hierarchical Address Book (HAB)
* Because the feature is based on the OAB, Outlook is the only supported client


### Address Book Policies
* used to control what directory objects are shown to users


