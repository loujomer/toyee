## Setup autoreply for all users

> `#Import the required module for Exchange Online`
> 
> `Import-Module ExchangeOnlineManagement`
> 
> `# Connect to Exchange Online`
> 
> `Connect-ExchangeOnline -UserPrincipalName admin@example.com -ShowProgress $true`
> 
> 
> `# Get all mailboxes`
> 
> `$mailboxes = Get-Mailbox -ResultSize Unlimited`
> 
> 
> `# Loop through each mailbox and set the auto reply configuration`
> 
> `foreach ($mailbox in $mailboxes) {`
> 
> `Set-MailboxAutoReplyConfiguration -Identity $mailbox.UserPrincipalName -AutoReplyState Enabled -ExternalMessage "Your message has been received. We will get back to you soon." -InternalMessage "Your message has been received. We will get back to you soon."`
> `}`
> 
> `# Disconnect from Exchange Online`
> `Disconnect-ExchangeOnline -Confirm:$false`
> 
## Setup email forwarding for all users

> `$forwardingAddresses = Import-Csv -Path "C:\path\to\your\file.csv" 
> 
> \# Loop through each row in the CSV file and set forwarding 
> 
> `foreach ($row in $forwardingAddresses) {`    
> `Set-Mailbox -Identity $row.UserPrincipalName -ForwardingSmtpAddress $row.ForwardingAddress -DeliverToMailboxAndForward $true }`


## find the oldest email

> `$user = "admin@mstestavaceblobor.onmicrosoft.com"`
> 
> `$stats = Get-MailboxFolderStatistics -IncludeOldestAndNewestItems -Identity $user`
> 
> `$oldestemail = $stats | Where-Object {$_.oldestitemreceiveddate -and $_.containerclass -eq 'ipf.note'} | sort-object oldestitemreceiveddate | select-object containerclass, oldestitemreceiveddate, folderpath -first 1`
> 
> `$oldestemail`


## recreate distro

> `#Connect-ExchangeOnline`
> 
> `#Get-DistributionGroupMember -Identity distro1 | select primarysmtpaddress | Export-Csv -Path C:\Users\VMASUser\Downloads\test\distromember.csv`
> 
> `#Update-DistributionGroupMember -Identity distro1 -Members dario@mstestavaceblobor.onmicrosoft.com`
> 
> `Import-Csv C:\Users\VMASUser\Downloads\newmembers.csv | ForEach-Object {`
> 
>     `Add-DistributionGroupMember -Identity distro1 -Member $_.upn`
> `}`


## search unified audit log - email 

> `Search-UnifiedAuditLog -StartDate 01/01/2024 -EndDate 03/15/2024 -UserIds [shaw@mstestavacebshsua.onmicrosoft.com](mailto:shaw@mstestavacebshsua.onmicrosoft.com) -RecordType ExchangeItem -SessionCommand ReturnLargeSet | Export-CSV -Path C:\Users\ShaSha\Documents\audit.csv`



## Test SMTP Client Auth Submission 

> `$cred= Get-Credential`
> 
> `$mailParams = @{`
> 
>     smtpServer = "smtp.office365.com"
> 
>     port = "587"
> 
>     useSSL = $true
> 
>     credential = $cred
> 
>     from = baks@kangkongan.store
> 
>     to = admin@kangkongan.store, loujomer@gmail.com
> 
>     subject = "Test Email from PowerShell"
> 
>     body = "This is a test email from Powershell"
> 
>     deliveryNotificationOption = "onFailure", "onSuccess"
> 
> }
> 
> send-mailmessage @mailParams
> 
> Requirements:
> 
> 1. disable Security defaults (security defaults will disable smtp authentication)
> 2. enable smtp authentication
> 3. enforce mfa to use app password
> 4. m365 mailbox
> 5. tls 1.2 or higher


## Test Direct Send

> `$mailParams = @{`
> 
>     smtpServer = "kangkongan-store.mail.protection.outlook.com"
> 
>     port = "25"
> 
>     useSSL = $true
> 
>     from = harvey@kangkongan.store
> 
>     to = admin@kangkongan.store
> 
>     subject = "Test Email from PowerShell"
> 
>     body = "This is a test email from Powershell"
> 
>     deliveryNotificationOption = "onFailure", "onSuccess"
> 
> }
> 
> send-mailmessage @mailParams
> 
> note: no need to have a mailbox


## get user's manager

> $userlist = Get-AzureADUser -All $true
> $output = @()
> 
>  
> `foreach ($item in $userlist) {`
>     `$manager = Get-AzureADUserManager -ObjectId $item.ObjectId`
> 
>     $data = New-Object -TypeName psobject
>     $data | Add-Member -MemberType NoteProperty -Name UsersObjectId -Value $item.ObjectId
>     $data | Add-Member -MemberType NoteProperty -Name UserUPN -Value $item.UserPrincipalName
>     $data | Add-Member -MemberType NoteProperty -Name UserType -Value $item.UserType
>     $data | Add-Member -MemberType NoteProperty -Name ManagersObjectId -Value $manager.ObjectId
>     $data | Add-Member -MemberType NoteProperty -Name ManagerUPN -Value $manager.UserPrincipalName
>     $data | Add-Member -MemberType NoteProperty -Name ManagerUserType -Value $manager.UserType
> 
> 
>     $output += $data
> } 
> 
> `$output | Export-Csv -Path C:\Temp\UsersAndManagersReport.csv -NoTypeInformation`


## archive storage for all users

> `Get-Mailbox -ResultSize Unlimited -RecipientTypeDetails UserMailbox | Select DisplayName, ArchiveStatus, @{Name="ArchiveSize"; Expression={(Get-MailboxStatistics $_.DistinguishedName -Archive).TotalItemSize}} | Export-Csv "C:\Users\VMASUser\Downloads\test\ArchiveMailboxes.csv" -NoTypeInformation -Encoding UTF8`


## dynamic distribution list

> `#create recipient filter based from Title`
> `$Filter = "((Title -eq 'CRM') -and (ExchangeUserAccountControl -ne 'AccountDisabled'))"`
> 
> `#check matching recipients - optional cmdlet to run`
> `Get-Recipient -RecipientPreviewFilter $Filter | ft displayname, title`
> 
> 
> `New-DynamicDistributionGroup -Name "My New DDL" -DisplayName "CRM Employees" -Alias CRMEmployees -PrimarySmtpAddress crm@mstestavaceblobor.onmicrosoft.com -RecipientFilter $Filter`
> 
> `Set-DynamicDistributionGroup -Identity CRMEmployees -ManagedBy admin@mstestavaceblobor.onmicrosoft.com -MailTip "Distribution List for anyone with CRM in the job title`
> 

## check user's MFA

`#Import the AzureAD module`
`Import-Module AzureAD`

`#Connect to Azure AD`
`Connect-AzureAD`

`#Specify the user's UPN (User Principal Name) or Object ID`

`$userUPN = "<User's UPN or Object ID>"`

`#Get the user's MFA information`
`$user = Get-AzureADUser -ObjectId $userUPN`

`#Check if the user has MFA configured`
`if ($user.StrongAuthenticationMethods.Count -gt 0) {`

    # Loop through the MFA methods to find the phone number

    foreach ($method in $user.StrongAuthenticationMethods) {

        if ($method.MethodType -eq "Phone") {

            Write-Host "MFA Phone Number: $($method.PhoneNumber)"
            break
        }
    }
`}`

`else {`
    `Write-Host "MFA is not configured for this user."`
`}

`#Disconnect from Azure AD`

`Disconnect-AzureAD`


## enable archive mailbox for all users

`Get-Mailbox -Filter {ArchiveStatus -Eq "None" -AND RecipientTypeDetails -Eq "UserMailbox"} | Enable-Mailbox -Archive`


### find sharepoint sites with no group

 `Get-SPOSite -IncludePersonalSite:$false | Where-Object -Property groupid -eq '00000000-0000-0000-0000-000000000000'`


