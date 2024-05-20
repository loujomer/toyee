# Distribution Lists
###### who can be members of a DL?
>1. User and shared mailboxes.
>2. Mail-enabled public folders.
>3. Other distribution lists, including dynamic distribution lists.
>4. Mail contacts. You can include a Teams channel in a distribution list by creating a mail contact using the channel’s email address.
>5. Mail users (including those created for guest accounts).
>6. Microsoft 365 Groups (these objects can only be added using PowerShell).

* Groups created by administrators are not subject to the organizational group naming policy as this only applies to user-created groups.

###### Blocking BCC Delivery to Distribution Lists
`[PS] C:\> Set-DistributionGroup -Identity "Message Board Posting" -BccBlocked $True

Inbox rules can process TO and CC recipients, but not BCC recipients because these recipients are not present in the message header

###### Reporting Distribution List Membership
`[PS] C:\> Get-DistributionGroupMember –Identity "Executive Committee" | Select-Object Name, DisplayName, PrimarySmtpAddress | Export-CSV "c:\temp\ExecutiveCommittee.csv" -NoTypeInformation

###### Updating Distribution List Membership with PowerShell

*overwrite existing DL membership:*
`[PS] C:\> Update-DistributionGroupMember -Identity "Blog Writers" -Members @("TRedmond", "Bowens")

*add/remove a single object*
`[PS] C:\> Add-DistributionGroupMember –Identity "Blog Writers" –Member VanHybrid
`Remove-DistributionGroupMember –Identity "Blog Writers" –Member VanHybrid –Confirm:$False

*add member to multiple DLs:*
first, create a one column CSV file that holds the alias of the target DL
![[Pasted image 20240506102017.png]]

###### [How to Move Distribution List Membership from One Mailbox to Another](https://office365itpros.com/2021/08/04/transfer-distribution-list-mailbox/)


###### Add a Microsoft 365 Group in a Distribution List
`[PS] C:\> Add-DistributionGroupMember -Identity P365.Authors -Member ExchangeMVPs


###### Add a Teams Channel in a Distribution List
To add a team channel email address to a distribution list, create a new mail contact with the address and add the mail contact to the distribution list.


###### Add a Guest Account in a Distribution List
`[PS] C:\> Add-DistributionGroupMember -Identity MyDL -Member JohnSmith_outlook.com#EXT#


###### Discovering Distribution Lists and Groups Someone Belongs to
can be done in EAC, Others -> Group membership

`[PS] C:\> $Dn = (Get-ExoMailbox –Identity Kim.Akers).DistinguishedName

`Get-ExoRecipient -Filter "Members -eq '$Dn'" -Properties RecipientTypeDetails | Sort RecipientTypeDetails | Format-Table DisplayName, RecipientTypeDetails

or for specific group type:
`[PS] C:\> Get-Recipient -Filter "Members -eq '$Dn'" -RecipientTypeDetails GroupMailbox | ft DisplayName


###### Remove a Distribution List
`[PS] C:\> Remove-DistributionGroup -Identity P365.Authors -Confirm:$False

###### [How to Find and Report Inactive Distribution Lists](https://office365itpros.com/2018/11/15/find-inactive-distribution-lists/)


###### Preventing users from creating a distribution list

Option 1: uncheck the My Distribution Groups (will hide the UI of owned DLs)
* uncheck the MyDistributionGroups
	* EAC -> Roles -> User Roles, either create a new role assignment policy or edit the default policy, then uncheck the 

Option 2: disable the creation of new DLs but still access the existing DLs
	1: create a new user role assignment policy
	EAC -> Roles -> User Roles -> New role assignment policy


###### Distribution List Naming Policy

Exchange applies the naming policy to distribution lists belonging to the on-premises organization when they synchronize with Azure AD objects with AADConnect. 

Thus, an on-premises distribution list might have a different name in the on-premises directory than that shown in Azure AD unless you make sure to apply the same naming policy in both environments. 


view group naming policy: 
`Get-OrganizationConfig | Format-Table Distr* -AutoSize

edit the group naming policy:
`Set-OrganizationConfig -DistributionGroupNamingPolicy "DL-<Department> <GroupName>"

remove the group naming policy
`Set-OrganizationConfig -DistributionGroupNamingPolicy $Null


###### Migrating On-Premises Distribution Lists to Exchange Online
* in hybrid deployment, the suggested method is to remove the distribution list from on-premises and recreate it as a brand-new object in the cloud
* an on-premises user cannot manage a cloud-based distribution list



# Dynamic Distribution lists

* Dynamic distribution lists are EXODS objects and do not exist in Azure AD
* in hybrid deployment, synchronization utilities do not process dynamic distribution lists


## Recipient filters
- OPATH queries executed against the directory to return a set of objects

### How Recipient Filters Work
2 types of recipient filters:
1. Precanned filters
2. Custom filters

###### view DDL filter using PowerShell
`Get-DynamicDistributionGroup –Identity "Microsoft 365 Gurus" | Select RecipientFilter, RecipientFilterType

###### Checking the Effectiveness of a Recipient Filter
`Get-Recipient -RecipientPreviewFilter (Get-DynamicDistributionGroup -Identity "testddl").RecipientFilter | select displayname

###### Finding the set of dynamic distribution lists a user belongs to
`$Dn = (Get-ExoMailbox –Identity dario).DistinguishedName

`Get-DynamicDistributionGroup | Where-Object {(Get-DynamicDistributionGroupMember -Identity $_.PrimarySMTPAddress | Where-Object {$_.DistinguishedName -eq $Dn})}

###### Membership Calculation
- At least once daily.
- After the recipient filter changes.
- Following the creation of a new dynamic distribution group.