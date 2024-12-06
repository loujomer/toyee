# Implement token customizations

- .In Microsoft Entra ID, a **policy object** represents a set of rules that are enforced on individual applications or on all applications in an organization
- Each policy type has a unique structure, with a set of properties that are applied to objects to which they're assigned
- You can designate a policy as the default policy for your organization.
- The policy is applied to any application in the organization, as long as it isn't overridden by a policy with a higher priority.
-  You also can assign a policy to specific applications. 
- The order of priority varies by policy type.

## Configure authentication session management with Conditional Access

## Customize Tokens for Microsoft Entra ID

#### Token lifetime definitions

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/token-customize-timeline-4817b4d6.png">

**Lifetime length (days)** - After time period elapses, the user is forced to reauthenticate
**Access and ID token lifetime** - The lifetime of the OAuth 2.0 bearer token and ID token
**Refresh token sliding windows lifetime** - The refresh token sliding window type
**Refresh token lifetime (days)** - The maximum time period before which a refresh token can be used to acquire a new access token
## Configure "optional claims" as part of your token

Optional claims in Microsoft Entra ID applications allow application developers to specify which claims they want included in the tokens sent to their application. 

They can be used to:
- select additional claims
- modify the behavior of certain claims returned by the Microsoft identity platform
- dd custom access claims

While optional claims are supported in both v1.0 and v2.0 format tokens, they are particularly valuable when transitioning from v1.0 to v2.0, as the latter aims for smaller token sizes for optimal client performance.

-----
# Implement and configure consent settings

You can integrate your applications with the Microsoft identity platform to allow users to sign in with their work or school account and access the organization's data to deliver rich data-driven experiences.

## User consent settings

- Disable user consent
- Users can consent to apps from [verified publisher](https://learn.microsoft.com/en-us/azure/active-directory/develop/publisher-verification-overview) or your organization, but only for permissions you choose
- Users can consent to all apps
- Custom app consent policy -  [create custom app consent policies](https://learn.microsoft.com/en-us/azure/active-directory/manage-apps/manage-app-consent-policies)

## Risk-based step-up consent

- helps reduce user exposure to malicious apps that make illicit consent requests
-  If Microsoft detects a risky end-user consent request, the request will require a step-up to admin consent instead
-  This capability is enabled by default, but it will only result in a behavior change when end-user consent is enabled.

---
# Integrate on-premises apps with Microsoft Entra application proxy


**Application Proxy**
- enables users to access on-premises web applications from a remote client.
-  includes both the **Application Proxy service** that runs in the cloud, and the **Application Proxy connector** that runs on an on-premises server
- Application proxy service and application proxy connector work together to securely pass the user sign-on token from Microsoft Entra ID to the web application.
- is recommended for giving remote users access to internal resources.
- ==replaces the need for a virtual private network (VPN) or reverse proxy==
- is not intended for internal users on the corporate network.

## How Application Proxy works

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/azure-app-proxxy-1e86d179.png">

1. After the user has accessed the application through an endpoint, the user is directed to the Microsoft Entra sign-in page.
2. After a successful sign-in, Microsoft Entra ID sends a token to the user's client device.
3. The client sends the token to the Application Proxy service, which retrieves the user principal name (UPN) and security principal name (SPN) from the token. Application Proxy then sends the request to the Application Proxy connector.
4. If you have configured single sign-on, the connector performs any additional authentication required on behalf of the user.
5. The connector sends the request to the on-premises application.
6. The response is sent through the connector and Application Proxy service to the user.

----
# Integrate custom SaaS apps for single sign-on

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/app-single-sign-on-79005b35.png">
- You can use Microsoft Entra ID as your identity system for just about any app. Many apps are already pre-configured and can be set up with minimal effort. These pre-configured apps are published in the Microsoft Entra ID App Gallery.
- You can manually configure most apps for single-sign-on if they aren't already in the gallery. 
- Microsoft Entra ID provides several SSO options. 
	- SAML-based SSO
	- OIDC-based SSO

 Comparison of the various protocols used by Microsoft identity platform:

**OAuth vs. OpenID Connect**
- OAuth is used for authorization
- OpenID Connect (OIDC) is used for authentication.

**OAuth vs. SAML**
- OAuth is used for authorization
- Security Assertion Markup Language (SAML) is used for authentication.

**OpenID Connect vs. SAML**
- Both OpenID Connect and SAML are used to authenticate a user and are used to enable single-sign-on.
- SAML authentication is commonly used with identity providers such as ADFS federated to Microsoft Entra ID and is therefore frequently used in enterprise applications.
- OpenID Connect is commonly used for apps that are **purely in the cloud**, such as mobile apps, web sites, and web APIs.

If you have an application that you want to integrate with Microsoft Entra ID to provide the single-sign-on experience for your users, please see the article ClaimsXRay in Microsoft Entra ID with Directory Extension, linked below:

[ClaimsXRay in Microsoft Entra ID with Directory Extension](https://techcommunity.microsoft.com/t5/core-infrastructure-and-security/claimsxray-in-azuread-with-directory-extension/ba-p/1505737)


---
# Implement application-based user provisioning

- In Microsoft Entra ID, the term **app provisioning** refers to **automatically creating user identities and roles in the cloud**
- automatic provisioning includes the maintenance and removal of user identities as status or roles change
-  A common scenario is provisioning a Microsoft Entra user into applications like [Dropbox](https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial), [Salesforce](https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/salesforce-provisioning-tutorial), [ServiceNow](https://learn.microsoft.com/en-us/azure/active-directory/saas-apps/servicenow-provisioning-tutorial), and more.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/provision-overview-676ba683.png">



| Features | Actions |
| ---- | ---- |
| Automate provisioning | Automatically create new accounts in the right systems for new people when they join a team or organization. |
| Automate deprovisioning | Automatically deactivate accounts in the right systems when people leave a team or organization. |
| Synchronize data between systems | Ensure that the identities in the apps and systems are kept up to date based on changes in the directory or the human resources system |
| Provision groups: | Provisioning a group to the applications that supports them. |
| Govern access | Monitor and audit who has been provisioned into the applications |
| Seamlessly deploy in brown field scenarios | Match existing identities between systems and allow for easy integration, even when users already exist in the target system. |
| Use rich customization | Take advantage of customizable attribute mappings that define what user data should flow from the source system to the target system |
| Get alerts for critical events | The provisioning service provides alerts for critical events and allows for Log Analytics integration where you can define custom alerts to suit your business needs. |
## Manual vs. automatic provisioning

**Manual provisioning**
- means there's no automatic Microsoft Entra provisioning connector for the app yet
-  User accounts must be created manually
- Examples of this include adding users directly into the administrative portal of the app or uploading a spreadsheet with user account details

**Automatic provisioning** 
- means that a Microsoft Entra provisioning connector has been developed for this application

In the Microsoft Entra ID gallery, applications that support automatic provisioning are designated by a **Provisioning** icon.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/browse-gallery-1eb29b09.png">

## System for Cross-domain Identity Management

-  provides a common user schema to help users move into, out of, and around apps
- is becoming the standard for provisioning and, when used in conjunction with federation standards like SAML or OpenID Connect, provides administrators an end-to-end, **standards-based** solution for access management.

## Build a System for Cross-domain Identity Management endpoint and configure user provisioning with Microsoft Entra ID

- an application developer can use SCIM user management API to enable automatic provisioning of users and groups between their application and Microsoft Entra ID
- The SCIM specification provides a common user schema for provisioning.
- SCIM is a standardized definition of two endpoints:
	- Users endpoint
	- Groups endpoint
- uses common Representational state transfer (REST) verbs to:
	- create
	- update, and 
	- delete objects
- and a pre-defined schema for common attributes like:
	- group name
	- username
	- first name
	- last name
	- email

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-monitor-integration-of-enterprise-apps-for-sso/media/system-for-cross-domain-identity-management-provision-overview-9d92fdf9.png">
Application developers that build a SCIM endpoint can integrate with any SCIM-compliant client without having to do custom work, rather than starting from scratch and building the implementation completely on your own, you can rely on a number of open source SCIM libraries published by the SCIM community.

----
# Monitor and audit access to Microsoft Entra integrated enterprise applications

-----
# Create and manage application collections

- Your users can use the My Apps portal to view and start the cloud-based applications they have access to.
- By default, all the applications a user can access are listed together on a single page.

1. Open the Microsoft Entra admin center and sign in as an admin.
    
2. Go to **Identity**, next open the **Applications** menu, then select **Enterprise Applications**.
    
3. Under **Manage**, select **App Launchers**.
    
4. Select **New collection**.

## My apps portal

My Apps is separate from the Azure portal and doesn't require users to have an Azure subscription or Microsoft 365 subscription.

Follow these steps to create a collection.

1. Open the **[My Apps](https://myapps.microsoft.com/) portal**.
2. Select the ellipsis (...) on the apps screen.
3. Choose **Manage collections.**
4. Select **Create collection.**
5. Select the **+ Add apps** option to add all the apps you want in the collection.
6. After picking your apps, select the **Add selected apps** button.
7. Give the collection a name and choose **Create collection**.