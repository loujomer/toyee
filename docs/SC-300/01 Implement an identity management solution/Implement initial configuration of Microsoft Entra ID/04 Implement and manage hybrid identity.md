Microsoft Entra Connect provides the following capabilities:
- Synchronization
- Password hash synchronization
- Pass-through authentication
- Federation integration
- Health monitoring

## Select an authentication method

- **Cloud authentication**
- **Federated authentication**

## Cloud authentication
- Microsoft Entra ID handles users' sign-in process

2 options:
- **Microsoft Entra password hash synchronization (PHS)**
- **Microsoft Entra pass-through authentication (PTA)**

#### Microsoft Entra password hash synchronization (PHS)
Users can use the same username and password that they use on-premises without having to deploy any more infrastructure.

- **Effort**
	- requires the least effort
	- runs every two minutes.
- **User experience**
	-  Seamless SSO eliminates unnecessary prompts when users are signed in.
- **Advanced scenarios**
	- it's possible to use insights from identities with Microsoft Entra Identity Protection reports
- **Business continuity**
	- is highly available as a cloud service that scales to all Microsoft datacenters
- **Considerations**
	-  doesn't immediately enforce changes in on-premises account states

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/azure-active-directory-authentication-2-a040e3df.png">

#### Microsoft Entra pass-through authentication (PTA)
 Provides a simple password validation for Microsoft Entra authentication services by using a software agent that runs on one or more on-premises servers. The servers validate the users directly with your on-premises Active Directory, which ensures that the password validation doesn't happen in the cloud. Companies with a security requirement to immediately enforce on-premises user account states, password policies, and sign in hours might use this authentication method.

- **Effort**
	- you need one or more (we recommend three) lightweight agents installed on existing servers
	- These agents must have access to your on-premises Active Directory Domain Services, including your on-premises AD domain controllers
	- They need outbound access to the Internet and access to your domain controllers. For this reason, it's not supported to deploy the agents in a perimeter network.
- **User experience**
	- Seamless SSO eliminates unnecessary prompts after users sign in.
- **Advanced scenarios**
	- enforces the on-premises account policy at the time of sign-in.
- **Business continuity**
	- it's recommended to deploy two extra pass-through authentication agents
- **Considerations**
	-  You can use PHS as a backup authentication method for PTA when the agents can't validate a user's credentials due to a significant on-premises failure.
	- Fail over to PHS doesn't happen automatically and you must use Microsoft Entra Connect to switch the sign-in method manually.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/azure-active-directory-authentication-3-5339e622.png">

## Federated authentication
Microsoft Entra ID hands off the authentication process to a separate trusted authentication system such as ADFS to validate the user’s password

- **Effort**
	- relies on an external trusted system to authenticate users
	- It's up to the organization by using the federated system to make sure it's deployed securely and can handle the authentication load.
- **User experience**
	- UX  depends on the implementation of the features, topology, and configuration of the federation farm
- **Advanced scenarios**.
	- A federated authentication solution is required when customers have an authentication requirement that Microsoft Entra ID doesn't support natively.
		- Authentication that requires smartcards or certificates.
		- - On-premises MFA servers or third-party multifactor providers requiring a federated identity provider.
		- Authentication by using third-party authentication solutions.
		- Sign in that requires a sAMAccountName, for example DOMAIN\username, instead of a User Principal Name (UPN), for example, user@domain.com
- **Business continuity**.
	- typically require a load-balanced array of servers, known as a farm.
	- This farm is configured in an internal network and perimeter network topology to ensure high availability for authentication requests.
- **Considerations**
	- typically require a more significant investment in on-premises infrastructure
	- more complex to operate and troubleshoot compared to cloud authentication solutions

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/azure-active-directory-authentication-4-131b824a.png">


## Recommendations

Use or enable **PHS** for whichever authentication method you choose, for the following reasons:

1. **High availability and disaster recovery**.
2. **On-premises outage survival**
3. **Identity protection**.


# Microsoft Entra Connect design concepts

## sourceAnchor
- sourceAnchor attribute is defined as _an attribute immutable during the lifetime of an object
- uniquely identifies an object as being the same object on-premises and in Microsoft Entra ID
- The attribute is also called **immutableId**
- recommended attribute to use as sourceAnchor: **objectGUID**

**Usage**:
- When a new sync engine server is built, or rebuilt, this attribute links existing objects in Microsoft Entra ID with objects on-premises.
- If you move from a cloud-only identity to a synchronized identity model, then this attribute allows objects to "hard match" existing objects in Microsoft Entra ID with on-premises objects.
- If you use federation, then this attribute together with the **userPrincipalName** is used in the claim to uniquely identify a user.

**objectGuid**
- used if you have a single forest on-premises
- used with express settings in Microsoft Entra Connect
- used by DirSync
- used with multiple forests if users aren't moved between forests and domains


## Microsoft Entra sign-in

- Microsoft Entra uses userPrincipalName (UPN) to authenticate the user
- when you synchronize your users, you must choose the attribute to be used for value of userPrincipalName carefully
### Custom domain state and User Principal Name

- If the UPN suffix of a user doesn't match a verified domain in Microsoft Entra ID, then the tool replaces the UPN suffix with tenant.onmicrosoft.com.
- if you're running under contoso.local (non routable), Microsoft Entra Connect suggests you **use custom settings** rather than using express settings
- Using custom settings, you're able to specify the attribute that should be used as UPN to sign in to Azure after the users are synced to Microsoft Entra ID.

## Topologies for Microsoft Entra Connect

1. Single forest, single Microsoft Entra tenant
2. Multiple forests, single Microsoft Entra tenant
3. Multiple forests, single sync server, users are represented in only one directory
4. Multiple forests: full mesh with optional GALSync
5. Multiple forests: account-resource forest
6. Staging server
7. Multiple Microsoft Entra tenants
8. Each object only once in a Microsoft Entra tenant

## Microsoft Entra Connect component factors

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/azure-active-directory-connect-internal-0444044d.png">

**Import** - the process of reading information from each directory
**Export** - updating the directories from the provisioning engine
**Sync** - evaluates the rules of how the objects will flow inside the provisioning engine.


Microsoft Entra Connect uses the following staging areas, rules, and processes to allow the sync from Active Directory to Microsoft Entra ID:

- **Connector Space (CS)**
	- Objects from each connected directory (CD), the actual directories, are staged here first before they can be processed by the provisioning engine.
	- Microsoft Entra ID has its own CS and each forest you connect to will have its own CS.
- **Metaverse (MV)**
	- Objects that need to be synced are created here based on the sync rules.
	- Objects must exist in the MV before they can populate objects and attributes to the other connected directories
- **Sync rules**
	- They decide which objects will be created (projected) or connected (joined) to objects in the MV
	- The sync rules also decide which attribute values will be copied or transformed to and from the directories.
- **Run profiles**
	- Bundles the process steps of copying objects and their attribute values according to the sync rules between the staging areas and connected directories.


## Microsoft Entra cloud sync

- designed to accomplish hybrid identity goals for synchronization of users, groups and contacts to Microsoft Entra ID
- synchronization is accomplished by using the **cloud provisioning agent** instead of the Microsoft Entra Connect application
- can be used alongside Microsoft Entra Connect sync
- provisioning from AD to Microsoft Entra ID is orchestrated in Microsoft Online Services
-  An organization only needs to deploy, in their on-premises or IaaS-hosted environment, a light-weight agent that acts as a bridge between Microsoft Entra ID and AD.
- The provisioning configuration is stored and managed as part of the service. Reminder that the sync runs every 2 minutes.

Benefits:
- Support for synchronizing to a Microsoft Entra tenant from a multi-forest disconnected Active Directory forest environment
- Simplified installation with light-weight provisioning agents: The agents act as a bridge from AD to Microsoft Entra ID, with all the sync configuration managed in the cloud.
- Multiple provisioning agents can be used to simplify high availability deployments
- Support for large groups with up to fifty-thousand members

-----
# Implement manage PHS

## How password hash synchronization works
Microsoft Entra Connect synchronizes a hash, of the hash, of a user's password from an on-premises Active Directory instance to a cloud-based Microsoft Entra instance

* ADDS stores passwords in the form of a hash value representation of the actual user password
* To synchronize your password, Microsoft Entra Connect sync extracts your password hash from the on-premises AD
* Extra security processing is applied to the password hash before it is synchronized to the Microsoft Entra authentication service.
* Passwords are synchronized on a per-user basis and in chronological order
* The password hash synchronization process **runs every 2 minutes**. You cannot modify the frequency of this process.
* When you synchronize a password, it overwrites the existing cloud password.
* The first time you enable the password hash synchronization feature, it performs an initial synchronization of the passwords of all in-scope users
* You cannot explicitly define a subset of user passwords that you want to synchronize during the first synchronization. 
* Once the initial synchronization completes, you can set up a **selective password hash synch** for future synchronizations.
* If there are multiple connectors, it is possible to disable password hash sync for some connectors but not others.
* When you change an on-premises password, the updated password is synchronized, most often in a matter of minutes. 
* The password hash synchronization feature automatically retries failed synchronization attempts. 
* If an error occurs during an attempt to synchronize a password, an error is logged in your event viewer.

## Enable password hash synchronization

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/password-hash-connect-setting-ef874d71.png">

* Express settings - PHS automatically enabled
* Custom settings -  PHS available on the user sign-in page (ss above)

----
# Implement manage pass-through authentication (PTA)

* PTA signs users in by validating their passwords directly against on-premises Active Directory.
- PTA is a tenant level feature

## Enable the feature

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/single-sign-on-fd559a4b.png">
- available in custom installation
- At the **User sign-in** page, choose **Pass-through authentication** as the **Sign On method**
- If you have already installed Microsoft Entra Connect by using the express installation or the custom installation path, select the **Change user sign-in** task on Microsoft Entra Connect

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-manage-hybrid-identity/media/change-user-sign-in-60b54bee.png">

On successful completion, **a PTA agent** is installed on the same server as Microsoft Entra Connect. In addition, the pass-through authentication feature is enabled on your tenant.

## Demo PTA and SSO

<video width=320 height=240 controls>
<source src="https://www.microsoft.com/en-us/videoplayer/embed/RE4Mzo1?postJsllMsg=true" type="video/mp4">
</video>

