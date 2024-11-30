
Microsoft Entra ID defines users in three ways:
- **Cloud identities**
	- These users exist only in Microsoft Entra ID.
	- Their source is **Microsoft Entra ID** or **External Microsoft Entra directory**
- **Directory-synchronized identities**
	-  These users exist in an on-premises Active Directory
- **Guest users**
	-  These users exist outside Azure

## Key properties of the Microsoft Entra B2B collaboration user

### UserType
- The UserType has no relation to how the user signs in, the directory role of the user, and so on. This property simply indicates the user's relationship to the host organization and allows the organization to enforce policies that depend on this property.
	- **Member**
	- **Guest**

### Identities
|**Identities property value**|**Sign-in state**|
|---|---|
|External Microsoft Entra tenant|This user is homed in an external organization and authenticates by using a Microsoft Entra account that belongs to the other organization.|
|Microsoft account|This user is homed in a Microsoft account and authenticates by using a Microsoft account.|
|{host’s domain}|This user authenticates by using a Microsoft Entra account that belongs to this organization.|
|google.com|This user has a Gmail account and has signed up by using self-service to the other organization.|
|facebook.com|This user has a Facebook account and has signed up by using self-service to the other organization.|
|mail|This user has signed up by using Microsoft Entra Email one-time passcode (OTP).|
|{issuer URI}|This user is homed in an external organization that doesn't use Microsoft Entra ID as their identity provider, but instead uses a SAML/WS-Fed-based identity provider.|



Microsoft Entra ID group types:
- **Security groups**
- **Microsoft 365 groups**


Membership types:
- Assigned
- Dynamic

-----
## Microsoft Entra registered devices
- Registered to Microsoft Entra ID without requiring organizational account to sign in to the device
- Microsoft Entra registered devices are signed in to using a local account like a Microsoft account on a Windows 10 device, but additionally have a Microsoft Entra account attached for access to organizational resources

## Microsoft Entra joined devices
- Joined only to Microsoft Entra ID requiring organizational account to sign in to the device
- Microsoft Entra joined devices are signed in to using an organizational Microsoft Entra account.

## Hybrid Microsoft Entra joined devices
- Joined to on-premises AD and Microsoft Entra ID requiring organizational account to sign in to the device
- These devices are devices that are joined to your on-premises Active Directory and registered with your Microsoft Entra directory.


## Device Writeback
- Device writeback helps you to keep a track of devices registered with Microsoft Entra ID in AD. You will have a copy of the device objects in the container "Registered Devices"


## Find license assignment errors
- Not enough licenses
- Service plans that conflict
- Other products depend on this license
- Usage location isn't allowed
- Duplicate proxy addresses
- Microsoft Entra Mail and ProxyAddresses attribute change
- LicenseAssignmentAttributeConcurrencyException in audit logs
- More than one product license assigned to a group

----
## What is a custom security attribute?
- are business-specific attributes (key-value pairs) that you can define and assign to Microsoft Entra objects.

## Explore automatic user creation
<img src="https://learn.microsoft.com/en-us/training/wwl-sci/create-configure-manage-identities/media/automatic-user-provisioning-c953c4ef-55508669.png">

### Why use SCIM?

- is an open standard protocol for automating the exchange of user identity information between identity domains and IT systems
- SCIM ensures that employees added to the Human Capital Management (HCM) system automatically have accounts created in Microsoft Entra ID or Windows Server Active Directory
- User attributes and profiles are synchronized between the two systems, updating removing users based on the user status or role change.
### Components of SCIM (System for Cross-Domain Identity Management)

- **HCM system** (Human Capital Management)
	-  Applications and technologies that enable Human Capital Management process and practices that support and automate HR processes throughout the employee lifecycle.
- **Microsoft Entra Provisioning Service**
	- Uses the SCIM 2.0 protocol for automatic provisioning. The service connects to the SCIM endpoint for the application, and uses the SCIM user object schema and REST APIs to automate provisioning and de-provisioning of users and groups.
- **Microsoft Entra ID**
	- User repository used to manage the lifecycle of identities and their entitlements.
- **Target system**
	- Application or system that has SCIM endpoint and works with the Microsoft Entra provisioning to enable automatic provisioning of users and groups.


