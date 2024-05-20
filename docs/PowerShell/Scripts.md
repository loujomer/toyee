## Setup autoreply for all users

`#Import the required module for Exchange Online`
`Import-Module ExchangeOnlineManagement`

`# Connect to Exchange Online`
`Connect-ExchangeOnline -UserPrincipalName admin@example.com -ShowProgress $true`


`# Get all mailboxes`
`$mailboxes = Get-Mailbox -ResultSize Unlimited`


`# Loop through each mailbox and set the auto reply configuration`

`foreach ($mailbox in $mailboxes) {`

`Set-MailboxAutoReplyConfiguration -Identity $mailbox.UserPrincipalName -AutoReplyState Enabled -ExternalMessage "Your message has been received. We will get back to you soon." -InternalMessage "Your message has been received. We will get back to you soon."`
`}`

`# Disconnect from Exchange Online`
`Disconnect-ExchangeOnline -Confirm:$false`

## Setup email forwarding for all users

`$forwardingAddresses = Import-Csv -Path "C:\path\to\your\file.csv" 

\# Loop through each row in the CSV file and set forwarding 

`foreach ($row in $forwardingAddresses) {`    
`Set-Mailbox -Identity $row.UserPrincipalName -ForwardingSmtpAddress $row.ForwardingAddress -DeliverToMailboxAndForward $true }`

