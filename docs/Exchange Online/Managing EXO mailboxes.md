
### The Link Between EXODS and Azure AD

**ExternalDirectoryObjectId**

* identifier that is stamped on EXODS object to **link to Azure AD**
* not found in on-premises Exchange

### Change in naming convention of user’s Name parameter
https://techcommunity.microsoft.com/t5/exchange-team-blog/change-in-naming-convention-of-user-s-name-parameter/ba-p/3284733


## Creating a Mailbox with PowerShell

**remote mailbox**
* is a *cloud mailbox* associated with a mail user object that’s 
stored in an *on-premises* Active Directory
* You’ll create remote mailboxes when you want your users 
anchored in on-premises AD but their mailboxes in Exchange Online

https://learn.microsoft.com/en-us/microsoft-365/enterprise/create-user-accounts-with-microsoft-365-powershell?view=o365-worldwide

### Creating a Cloud Mailbox for an On-Premises User (remote mailbox)


1. Make sure that your PowerShell session loads the necessary modules. You need to load both the **ExchangeOnlineManagement** and **Microsoft Graph PowerShell SDK** modules to create mailboxes and manipulate the underlying Azure AD objects.
2. Run the **New-RemoteMailbox** cmdlet to create a new user account and its Exchange Online mailbox.
3. Run the **Set-User cmdlet** to update the on-premises directory with the organizational and personal settings for the user object.
4. Run the Update-MgUser cmdlet to set the correct country code (location) for the new mailbox in the cloud. You assign a country to a user account to ensure that Microsoft 365 makes the services designated for that country available to the user. 
	For example:
	`[PS] C:\> Update-MgUser -UserId Kim.Akers@Office365itpros.com -PreferredDataLocation DE`
5.  Assign a license to the user.


### Creating a Cloud-Only Mailbox 

[PS] C:\> New-Mailbox -alias paulr -MicrosoftOnlineServicesId paulr@Office365itpros.com 


## Updating Mailbox Attributes

Some of the attributes commonly populated for mailboxes are not set with the New-Mailbox or Set-Mailbox cmdlets. 

These are attributes controlled by Azure AD and are common to all Exchange recipient objects,  whether they have a mailbox, and are updated with the **Set-User** cmdlet. 

Example: 

`[PS] C:\> Set-User –Identity "Kim Akers" –City "Dublin" –CountryOrRegion "Ireland" –Department "Marketing Operations" –Title "VP Marketing (New Products)" –Manager "Paul.Robichaux" –Office "Dublin HQ" –Company "Popular Books"`


## Add User Photos to Mailboxes

