## Basic SharePoint Online Concepts

### Sites

### Hub Sites
* integrate multiple Communication and Team sites
* a tenant is currently limited to 2,000

### Tenant Home Site
* a particular kind of Communication site
* there is a limitation of having a single Home site per SharePoint tenant
* You can designate a Communication site as the Home site for the tenant

### Modern Information Architectures for SharePoint Online
* based on single-site collections that can be associated with a Hub site


### SharePoint Search
* uses Microsoft Search Service to create content indexes


### Pages


### Apps


### Metadata


### File Versioning
* SharePoint Online and OneDrive for Business can restore document libraries to a point in time within the past 30 days
* SharePoint Online supports the concept of major and minor versions while OneDrive for Business only supports major versions.
* a new version might be generated every ten minutes or so, depending on the volume and type of edits in a document

### AutoSave
* Co-authoring is possible without AutoSave, but users must manually refresh their copies of documents to see changes


### SharePoint Online Quotas and Limits


# Managing SharePoint Online


#### Settings and Permissions for Group-enabled Sites


#### Managing Content Services



# Sharing

* SharePoint Online uses Azure AD B2B Collaboration to control sharing
	* when someone creates a sharing link, SharePoint uses Azure AD Invitation Manager to create a guest account in the tenant
	* sharing link contains an invitation for that person

>[!tip] Site People View
>The SharePoint browser interface keeps track of external people with whom users share resources. Sometimes, errors creep into the list of external people, such as when people enter incorrect email addresses into sharing links. 
>
>To remove these errors and prevent them from showing up as suggested users in sharing dialogs, a site administrator can access the list by adding `/_layouts/people.aspx?MembershipGroupId=0` to the site URL. 


###### [SharePoint and OneDrive integration with Microsoft Entra B2B](https://learn.microsoft.com/en-us/sharepoint/sharepoint-azureb2b-integration)
* provides authentication and management of guests
* Authentication happens via one-time passcode
* Enabling this integration doesn't change your sharing settings. For example, if you have site collections where external sharing is turned off, it remains off.

* Once the integration is enabled you and your users don't have to reshare or do any manual migration for guests previously shared with.
* Instead, when someone outside your organization clicks on a link that was created before Microsoft Entra B2B integration was enabled, SharePoint automatically creates a B2B guest account

* SharePoint and OneDrive integration with the Microsoft Entra B2B one-time passcode feature is enabled by default for new tenants.

* When the integration is enabled, people outside the organization will be invited via the Azure B2B platform when sharing from SharePoin

## Sharing Controls

###### 4 types of sharing links
1. Anyone
2. New and existing guests
3. Existing guests
4. Only people in your organization

###### Expiring Access Policy
* this setting applies only to sharing links, direct permission changes, and SharePoint group membership
* does not apply to guest access to SharePoint Online sites granted through the membership of Microsoft 365 Groups


### Sharing with Microsoft 365 Groups or Teams

* Every group has a SharePoint site with a document library
* The tenant sharing setting prevails over the setting for a site. 
* If you want to use a setting like *ExternalUserAndGuestSharing* for the site belonging to a group, you must first make sure that the organization allows anonymous sharing.

###### Check the sharing capability for a group

`[PS] C:\>$sGroupName=”Office365forITPros” 
`Get-SPOSite -Identity (Get-UnifiedGroup -Identity $sGroupName).SharePointSiteUrl | Select SharingCapability


###### Display guests in people picker (off by default)
`[PS] C:\> Set-SPOTenant -ShowPeoplePickerSuggestionsForGuestUsers $True


###### Find group enabled sites
`Get-SPOSite -Template "GROUP#0" -IncludePersonalSite:$False


###### Tracking Shared Files

`[PS] C:\> Set-SPOTenant –BccExternalSharingInvitations $True –BccExternalSharingInvitationsList administrator@Office365ITPros.com

* this will send an email each time a user shares a file stored in a SharePoint Online or OneDrive for Business document library with an external person