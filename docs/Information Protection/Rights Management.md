
## Licensing Requirements for Microsoft Information Protection

* All Microsoft 365 licenses include the ability to consume protected documents


## Enabling Rights Management for a Tenant

### Configuring Rights Management for Exchange Online


###### Viewing the IRM configuration
`Get-IRMConfiguration

**SimplifiedClientAccessEnabled:** 
* Controls if the Protect button is available in OWA. 
* Set to $False if you don’t want users to apply protection to messages. 
* This control is now obsolete as the Protect button has been replaced by the Sensitivity button


