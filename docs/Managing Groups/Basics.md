1. is created from client of choice, e.g. Outlook, Teams
2. new group object exists in both Azure AD and Exchange Online
	1. . Azure AD is the system of record and is responsible for synchronizing information about the new group to other workloads across Microsoft 365 (for example, Planner and SharePoint Online) to make those applications aware that a new group is available.
	2. a workload must recognize Groups as a valid security mechanism before Groups can control access to resources.
	3. The Exchange Online directory holds email-related information about Microsoft 365 Groups, including their proxy addresses.
	4. When you use Exchange clients or cmdlets from the Exchange Online PowerShell module to update Groups, a dual-write mechanism commits the updates simultaneously in Azure AD and Exchange Online. Following a successful update, Azure AD synchronizes the changes with other application directories


# Applications that use Microsoft 365 Groups

* Outlook
* Microsoft Teams
* Microsoft Planner
* Viva Engage (Yammer) Communities
* SharePoint sites 
* Stream
* Power BI


# Core Principles
![[Pasted image 20240509193533.png]]

#### 1. Azure AD is the directory of record
* when a new group is created, it is first created in Azure AD
* then it is created in EXODS to create the group mailbox
* EXO then updates the object in AAD with information such as its email address
* provisioning process creates other components like SPO site, notebook, etc
* Azure AD use "forward sync"

#### 2. Federated resources are drawn from multiple applications
#### 3. Loose coupling with groups and applications


# Groups Basics

###### A focus on self-service
###### Simple permissions
###### Sharing is easy
###### A shared memory
###### A growing experience


# Group Components

### Members and Owners

* Owners
* Members
* Subscribers

Azure AD supports two types of group membership:
* Dynamic user
* Assigned

>[!tip] [Printing a Report of Microsoft 365 Group (Team) Membership - Office 365 for IT Pros (office365itpros.com)](https://office365itpros.com/2021/02/19/printing-microsoft365-group-membership/)


# Ownerless Groups Policy

**Check ownerless groups:** 
`[PS] C:\> [array]$Groups = (Get-UnifiedGroup -ResultSize Unlimited | Select-Object DisplayName, ManagedBy) $Groups | Where-Object {$_.ManagedBy.Count -eq 0


# Group Privacy (Access Type)

**Set the default group access type**
`[PS] C:\> Set-OrganizationConfig –DefaultGroupAccessType Public

**Set a group's privacy**
`[PS] C:\> Set-UnifiedGroup –Identity TestGroup –AccessType Private


# Conversations

###### Access to Other Folders in the Group Mailbox

* clients usually only expose the Inbox and Calendar folders
* Sent Items folder stores copies of messages generated by clients, such as notifications to users that someone has assigned them a task using Planner
# Assigning Send As and Send On Behalf Of Permissions to Groups

`[PS] C:\> Add-RecipientPermission Microsoft365Gurus@Office365ITPros.com -AccessRights SendAs -Trustee Kim.Akers@Office365ITPros.com

`[PS] C:\> Set-UnifiedGroup -Identity "Microsoft 365 Gurus" -GrantSendOnBehalfTo @{Add="John.Smith@Office365ITPros.com"}


# Auto-Reply for Group Mailboxes

`[PS] C:\> Set-MailboxAutoReplyConfiguration -Identity "Office 365 for IT Pros" -ExternalMessage "Sorry, this mailbox doesn't accept email" -AutoReplyState Enabled -InternalMessage 'Please! We use Teams for communication, so send your message to [Teams](mailto: 943a5091.office365itpros.com@emea.teams.ms) and it will be dealt with there.'


# Group Document Libraries

* When a file is in a Group’s document library, the group controls its permissions. 
* All documents in the library inherit the permissions to allow group members **equal access** to the content.
* SharePoint checks permissions when a user requests access to a file by checking the group membership
* It is also important to remember that the **group owns the content in the group** document library rather than the original authors. 

**Regional settings:**
* The regional settings for document libraries created for Groups inherit their time zone and locale (country) from the account which creates the group. 
* Group members see date and timestamps for documents as if they were in that time zone.
* You can change the site’s time zone and locale (country) by updating the Regional Settings (in Site Information). 
* Sites created in the SharePoint admin center use the regional settings defined in the Site creation settings


# Implementing Groups

## Group Controls

group creation is available at these levels:
1. Azure AD (directory)
2. Microsoft 365 (all workloads)
3. App-level
	1. : Outlook clients use a setting in the OWA mailbox policy to define if a user can create a new group through Outlook (including OWA and Outlook mobile). This control does not require any additional licenses.


### Azure AD Policy for Groups

#### Licensing Groups Functionality

#### Guest Access Licenses
* included in all Microsoft 365 Business Standard and Premium plans and Enterprise subscriptions, which means that you do not need to assign licenses to guest users to access applications like Groups, Teams, and Planner


#### Creating and Updating the Groups Policy
* By default, a tenant does not have an active Groups policy, and applications use default settings. 
* To change any settings, we first create a directory setting object to hold the policy settings from the correct template. 
* Azure AD loads default values into the new policy and you can then update those settings


# Container Management for Groups, Sites, and Teams

* A classification is a text-only marker (like “Secret” or “Confidential”) assigned to a group and displayed by applications
* A tenant can choose to use classifications or sensitivity labels to mark groups. They cannot use both

## Creating Group Classifications

## Switching from Classifications to Sensitivity Labels


# Controlling Group Creation

check if group creation is enabled: 

`Get-OrganizationConfig | Format-List Group*,Data*



## Groups and Transport

* clients can post new conversations and reply to existing conversations
* Outlook handles a message posted to a group conversation much like any other message.
* By default, the Exchange Online transport service does not deliver copies of messages posted by a user to their Inbox.



# Managing Groups with Outlook Clients



# Guest Access to Microsoft 365 Groups

* there is no specific limit on the number of guests who can join a group (or team)