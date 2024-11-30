- Federation is a collection of domains that have established trust. 
- The level of trust varies, but typically **includes authentication** and almost always **includes authorization**.
- This sign-in method ensures that all user authentication occurs on-premises.
- Federation with AD FS and PingFederate is available.

## Requirement to deploy federation with AD FS and Microsoft Entra Connect

- Local administrator credentials on your federation servers.
- Local administrator credentials on any workgroup servers (not domain-joined) that you intend to deploy the Web Application Proxy role on.
- The machine that you run the wizard on to be able to connect to any other machines that you want to install AD FS or Web Application Proxy on by using Windows Remote Management.

## Set up your federation using Microsoft Entra Connect to connect to an AD FS farm

**Specify the AD FS servers**
- Specify the servers where you want to install AD FS
- Before you set up this configuration, join all AD FS servers to Active Directory
- This step isn't required for the Web Application Proxy servers.

**Specify the Web Application Proxy servers**
- The Web Application Proxy server is deployed in your perimeter network, facing the extranet
-  It supports authentication requests from the extranet

**Specify the service account for the AD FS service**
- The AD FS service requires a domain service account to authenticate users and to look up user information in Active Directory. It can support two types of service accounts:
	- Group managed service account
	- Domain user account
**Select the Microsoft Entra domain that you want to federate**
- Use the Microsoft Entra domain page to set up the federation relationship between AD FS and Microsoft Entra ID


<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/sc300-federation-setup-dialog-f995542d.png">

## Microsoft Entra Connect tools to manage your federation

- **Repair the trust**
- **Federate with Microsoft Entra ID using AlternateID**
- **Add a federated domain**


## Device writeback

- used to enable device-based conditional Access for ADFS-protected devices
- This conditional Access provides extra security and assurance that access to applications is granted only to trusted devices
- Device writeback enables this security by synchronizing all devices registered in Azure back to the on-premises Active Directory
- When configured during setup, the following operations are performed to prepare the AD forest:
	- If they do not exist already, create and configure new containers and objects under: **CN=Device Registration Configuration,CN=Services,CN=Configuration,[forest dn ]**.
	- If they do not exist already, create and configure new containers and objects under: **CN=RegisteredDevices,[domain-dn]**. Device objects will be created in this container.
	- Set necessary permissions on the Microsoft Entra Connector account, to manage devices on your Active Directory.

----

# Trouble-shoot synchronization errors

3 types of operations to keep the directories in sync:
1. Import
2. Synchronization
3. Export

## Errors during export to Microsoft Entra ID

### InvalidSoftMatch

- When Microsoft Entra Connect (sync engine) instructs directory to add or update objects, Microsoft Entra ID matches the incoming object using the **sourceAnchor** attribute to the **immutableId** attribute of objects in Microsoft Entra ID. This match is called a **Hard Match**.

- When Microsoft Entra ID **does not find** any object that matches the **immutableId** attribute with the **sourceAnchor** attribute of the incoming object, before provisioning a new object, **it falls back to use the ProxyAddresses and UserPrincipalName attributes to find a match**. This match is called a **Soft Match**.

* The Soft Match is designed to match objects already present in Microsoft Entra ID with the new objects being added/updated during synchronization that represent the same entity (users, groups) on-premises.

- **InvalidSoftMatch** error occurs when the hard match does not find any matching object **AND** soft match finds a matching object but that object has a different value of _immutableId_ than the incoming object's _SourceAnchor_, suggesting that the matching object was synchronized with another object from on premises Active Directory.

- In other words, in order for the soft match to work, the object to be soft-matched with should not have any value for the _immutableId_. If any object with _immutableId_ set with a value is failing the hard-match but satisfying the soft-match criteria, the operation would result in an InvalidSoftMatch synchronization error.

- Microsoft Entra directory schema does not allow two or more objects to have the same value of the following attributes. (This is not an exhaustive list.)
	- ProxyAddresses
	- UserPrincipalName
	- onPremisesSecurityIdentifier
	- ObjectId

#### Example scenarios for InvalidSoftMatch

- Two or more objects with the same value for the ProxyAddresses attribute exist in on-premises Active Directory. Only one is getting provisioned in Microsoft Entra ID.
- Two or more objects with the same value for the userPrincipalName attribute exist in on-premises Active Directory. Only one is getting provisioned in Microsoft Entra ID.

- An object was added in the on premises Active Directory with the same value of ProxyAddresses attribute as that of an existing object in Microsoft Entra directory. The object added on premises is not getting provisioned in Microsoft Entra directory.
- A synced account was moved from Forest A to Forest B. Microsoft Entra Connect (sync engine) was using ObjectGUID attribute to compute the SourceAnchor. After the forest move, the value of the SourceAnchor is different. The new object (from Forest B) is failing to sync with the existing object in Microsoft Entra ID.

- Microsoft Entra Connect was uninstalled and reinstalled. During the reinstallation, a different attribute was chosen as the SourceAnchor. All the objects that had previously synced stopped syncing with InvalidSoftMatch error.

- Microsoft Entra Connect was uninstalled and reinstalled. During the reinstallation, a different attribute was chosen as the SourceAnchor. All the objects that had previously synced stopped syncing with InvalidSoftMatch error.

#### How to fix InvalidSoftMatch error

The most common reason for the InvalidSoftMatch error is two objects with different SourceAnchor (immutableId) have the same value for the **ProxyAddresses** and/or **UserPrincipalName** attributes, which are used during the soft-match process on Microsoft Entra ID. In order to fix the Invalid Soft-Match:

1. Identify the duplicated proxyAddresses, userPrincipalName, or other attribute value that's causing the error. Also identify which two (or more) objects are involved in the conflict. The report generated by [Microsoft Entra Connect Health for sync](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-sync) can help you identify the two objects.
2. Identify which object should continue to have the duplicated value and which object should not.
3. Remove the duplicated value from the object that should NOT have that value. 
4. You should make the change in the directory where the object is sourced from. 
5. In some cases, you may need to delete one of the objects in conflict.

### ObjectTypeMismatch

- When Microsoft Entra ID attempts to soft match two objects, it is possible that two objects of different "object type" (such as User, Group, Contact etc.) have the same values for the attributes used to perform the soft-match. As duplication of these attributes is not permitted in Microsoft Entra, the operation can result in "ObjectTypeMismatch" synchronization error.

#### Example scenarios for ObjectTypeMismatch error

- A mail enabled security group is created in Microsoft 365. Admin adds a new user or contact in on premises AD (that's not synchronized to Microsoft Entra ID yet) with the same value for the ProxyAddresses attribute as that of the Microsoft 365 group.

#### How to fix ObjectTypeMismatch error

The most common reason for the ObjectTypeMismatch error is two objects of different type (User, Group, Contact etc.) have the same value for the **ProxyAddresses** attribute. In order to fix the ObjectTypeMismatch:

1. Identify the duplicated proxyAddresses (or other attribute) value that's causing the error. Also identify which two (or more) objects are involved in the conflict. The report generated by [Microsoft Entra Connect Health for sync](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-sync) can help you identify the two objects.
2. Identify which object should continue to have the duplicated value and which object should not.
3. Remove the duplicated value from the object that should NOT have that value. You should make the change in the directory where the object is sourced from. In some cases, you may need to delete one of the objects in conflict.

### AttributeValueMustBeUnique
Microsoft Entra schema does not allow two or more objects to have the same value of the following attributes. That is each object in Microsoft Entra ID is forced to have a unique value of these attributes at a given instance.

- ProxyAddresses
- UserPrincipalName
#### Example scenario of AttributeValueMustBeUnique
Duplicate value is assigned to an already synced object, which conflicts with another synced object.
#### How to fix AttributeValueMustBeUnique error
The most common reason for the AttributeValueMustBeUnique error is two objects with different SourceAnchor (immutableId) have the same value for the ProxyAddresses and/or UserPrincipalName attributes. In order to fix AttributeValueMustBeUnique error:

1. Identify the duplicated proxyAddresses, userPrincipalName or other attribute value that's causing the error. Also identify which two (or more) objects are involved in the conflict. The report generated by [Microsoft Entra Connect Health for sync](https://learn.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-health-sync) can help you identify the two objects.
2. Identify which object should continue to have the duplicated value and which object should not.
3. Remove the duplicated value from the object that should NOT have that value. You should make the change in the directory where the object is sourced from. In some cases, you may need to delete one of the objects in conflict.

### IdentityDataValidationFailed
Microsoft Entra ID enforces various restrictions on the data itself before allowing that data to be written into the directory. These restrictions are to ensure that end users get the best possible experiences while using the applications that depend on this data.

#### Example scenarios of IdentityDataValidationFailed
The UserPrincipalName attribute value has invalid/unsupported characters. The UserPrincipalName attribute does not follow the required format.

#### How to fix IdentityDataValidationFailed error
Ensure that the userPrincipalName attribute has supported characters and required format.

### FederatedDomainChangeError
This case results in a **"FederatedDomainChangeError"** sync error when the suffix of a user's UserPrincipalName is changed from one federated domain to another federated domain.

#### Example scenarios of FederatedDomainChangeError
For a synchronized user, the UserPrincipalName suffix was changed from one federated domain to another federated domain on premises. For example, _UserPrincipalName = bob@contoso.com_ was changed to _UserPrincipalName = bob@fabrikam.com_.

1. Bob Smith, an account for Contoso.com, gets added as a new user in Active Directory with the UserPrincipalName bob@contoso.com
2. Bob moves to a different division of Contoso.com called Fabrikam.com and their UserPrincipalName is changed to bob@fabrikam.com
3. Both contoso.com and fabrikam.com domains are federated domains with Microsoft Entra ID.
4. Bob's userPrincipalName does not get updated and results in a "FederatedDomainChangeError" sync error.

### LargeObject
When an attribute exceeds the allowed size limit, length limit or count limit set by Microsoft Entra schema, the synchronization operation results in the **LargeObject** or **ExceededAllowedLength** sync error. Typically this error occurs for the following attributes
- userCertificate
- userSMIMECertificate
- thumbnailPhoto
- proxyAddresses
#### Possible scenarios

1. Bob's userCertificate attribute is storing too many certificates assigned to Bob. These may include older, expired certificates. The hard limit is 15 certificates.
2. Bob's userSMIMECertificate attribute is storing too many certificates assigned to Bob. These may include older, expired certificates. The hard limit is 15 certificates.
3. Bob's thumbnailPhoto set in Active Directory is too large to be synced in Microsoft Entra ID.
4. During automatic population of the ProxyAddresses attribute in Active Directory, an object has too many ProxyAddresses assigned.
#### How to fix LargeObject

Ensure that the attribute causing the error is within the allowed limitation.

### Admin role conflict

An **Existing Admin Role Conflict** will occur on a user object during synchronization when that user object has:
- administrative permissions and
- the same UserPrincipalName as an existing Microsoft Entra object

Microsoft Entra Connect is not allowed to soft-match a user object from on-premises AD with a user object in Microsoft Entra ID that has an administrative role assigned to it.

#### How to fix Existing Admin Role Conflict
1. Remove the Microsoft Entra account (owner) from all admin roles.
2. **Hard Delete** the Quarantined object in the cloud.
3. The next sync cycle will take care of soft-matching the on-premises user to the cloud account (since the cloud user is now no longer a global GA).
4. Restore the role memberships for the owner.


