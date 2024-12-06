# Plan your line of business application registration strategy

2 representations of applications in Microsoft Entra ID:
- [application objects](https://learn.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)
- and service principals

## What are application objects?

- Managed in Entra ID portal under App registrations
- Application objects define and describe the application to Microsoft Entra ID, enabling your identity provider to know how to issue tokens to the application based on its settings.
- The application object will only exist in its home directory, even if it's a multitenant application supporting service principals in other directories.
-  Any given application can have at most one application object (which is registered in a "home" directory)

The application object includes any of the following among others:
- Name, logo, and publisher
- Redirect URIs
- Secrets (symmetric and/or asymmetric keys used to authenticate the application)
- API dependencies (OAuth)
- Published APIs/resources/scopes (OAuth)
- App roles (RBAC)
- SSO metadata and configuration
- User provisioning metadata and configuration
- Proxy metadata and configuration

## How application objects are created?

- Application registrations in the Azure portal.
- Creating a new application using Visual Studio and configuring it to use Microsoft Entra authentication.
- When an admin adds an application from the app gallery (which will also create a service principal).
- Using the Microsoft Graph API or PowerShell to create a new application.
- Many other pathways, including various developer experiences in Azure and in API explorer experiences across developer centers.


## What are service principals?

- Managed in Entra ID portal under Enterprise applications
- Service principals govern an application connecting to Microsoft Entra ID
- can be considered the instance of the application in your directory
- any given application can have one or more service principal objects representing instances of the application in every directory in which it acts.

The service principal can include:
- A reference back to an application object through the application ID property.
- Records of local user and group application-role assignments.
- Records of local user and admin permissions granted to the application.
    - For example: permission for the application to access a particular user's email.
- Records of local policies including Conditional Access policy.
- Records of alternate local settings for an application.
    - Claims transformation rules.
    - Attribute mappings (User provisioning).
    - Directory-specific app roles (if the application supports custom roles).
    - Directory-specific name or logo.

## How are service principals created?

- When users sign in to a third-party application integrated with Microsoft Entra ID.
- When users sign in to Microsoft online services like Microsoft 365.
- When an admin adds an application from the app gallery (this will also create an underlying app object).
- Add an application to use the Microsoft Entra Application Proxy.
- Connect an application for SSO using SAML or password SSO.
- Programmatically via the Microsoft Graph API or PowerShell.


## How are application objects and service principals related to each other?

An application has one application object in its home directory that's referenced by one or more service principals in each of the directories where it operates (including the application's home directory).

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-app-registration/media/how-apps-added-azure-active-directory-12fae69b.png">

1. Microsoft maintains two directories internally (shown on the left) that it uses to publish applications:
	- One for Microsoft Apps (Microsoft services directory).
	- One for preintegrated third-party applications (App gallery directory).

Application publishers/vendors who integrate with Microsoft Entra ID are required to have a publishing directory (shown on the right as "Some SaaS directory").

Applications that you add (represented as "App (yours)" in the diagram) include:

- Apps you developed (integrated with Microsoft Entra ID).
- Apps you connected for SSO.
- Apps you published using the Microsoft Entra Application Proxy.

### Notes and exceptions to service principals

- Not all service principals point back to an application object
- it's still possible to create service principals through different pathways, such as using PowerShell, without first creating an application object.

## Adding a new app registration

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-app-registration/media/register-new-app-b56ce83d.png">

**Process flow of the diagram**

1. User requests to register an application – a request token is issued.
2. Authorization endpoint sends back an Authentication.
3. User consents to have the application registration.
4. Service is created from the application
5. Token returned to the user.

----
# Implement application registration

- Each application you want the Microsoft identity platform to perform identity and access management (IAM) for must be registered.
- registering it establishes a trust relationship between your application and the identity provider, the Microsoft identity platform
---
# Register an application

Registering your application establishes a trust relationship between your app and the Microsoft identity platform.

The trust is **unidirectional**: Your app trusts the Microsoft identity platform—not the other way around.

## Add a redirect URI

A redirect URI is the location where the Microsoft identity platform redirects a user's client and sends security tokens after authentication

## Configure platform settings

To configure application settings based on the platform or device you're targeting:

1. Select your application in **App registrations** in the Azure portal.
2. Under **Manage**, select **Authentication**.
3. Under **Platform configurations**, select **Add a platform**.
4. In **Configure platforms**, select the tile for your application type (platform) to configure its settings.

## Add credentials

- Credentials are used by confidential client applications that access a web API.
- Examples of confidential clients:
	- web apps
	- other web APIs
	- service-type applications
	- daemon-type applications
- Credentials allow your application to **authenticate as itself**, requiring no interaction from a user at runtime.

- You can add both **certificates** and **client secrets** (a string) as credentials to your confidential client app registration.

## Add a certificate
- certificates are the recommended credential type
- Your certificate must be one of the following file types: `.cer`, `.pem`, `.crt`.

## Add a client secret

- also known as an _application password_, is a string value your app can use in place of a certificate to identity itself
- It's often used during development, but is considered less secure than a certificate.

## Add a scope

The code in a client application requests permission to perform operations defined by your web API by passing an access token along with its requests to the protected resource (the web API).

First, follow these steps to create an example scope named Employees.Read.All:

- Select **Microsoft Entra ID**, then **App registrations**, and then select your API's app registration.
- Select **Expose an API**, then **Add a scope**.
- You're prompted to set an **Application ID URI** if you haven't yet configured one. The App ID URI acts as the prefix for the scopes you'll reference in your API's code, and it must be globally unique. You can use the default value provided, which is in the form `api://` , or specify a more readable URI like `https://contoso.com/api`.
- Next, specify the scope's attributes in the **Add a scope pane**.

-----
# Configure permission for an application

## Scopes and permissions

Any web-hosted resource (both Microsoft and 3rd party resources) that integrates with the Microsoft identity platform has a resource identifier, or _Application ID URI_

In OAuth 2.0, permissions are also called _scopes_
- represented in the Microsoft identity platform as a string value
- examples;
	- Read a user's calendar by using Calendars.Read
	- Write to a user's calendar by using Calendars.ReadWrite
	- Send mail as a user using by Mail.Send

certain high-privilege permissions can only be granted through administrator consent and requested/granted using the administrator consent endpoint.

## Permission types

| Permission type | Effective permission | Description |
| ---- | ---- | ---- |
| delegated permissions | - the least privileged intersection of the delegated permissions the app has been granted (via consent) and the privileges of the currently signed-in user<br>- the app can never have more privileges than the signed-in user  | - used by apps that have a signed-in user present<br>- the app is delegated permission to act as the signed-in user when making calls to the target resource |
| application permissions | - the full level of privileges implied by the permission<br>- For example, an app that has the _User.ReadWrite.All_ application permission can update the profile of every user in the organization. | - used by apps that run without a signed-in user present<br>- for example, apps that run as background services or daemons<br>- only an administrator can consent to application permissions |

## OpenID Connect Scopes

| Scope | Description |
| ---- | ---- |
| Openid | - used by apps who performs sign-in by using OpenID Connect<br>- an app can receive a unique identifier for the user in the form of the sub claim<br>- also gives the app access to the **UserInfo** endpoint |
| email | - can be used with the openid scope and any others<br>- gives the app access to the user's primary email address in the form of the email claim<br>- email claim is included in a token only if an email address is associated with the user account, which isn't always the case |
| profile | - can be used with the openid scope and any others<br>- gives the app access to user information such as user's given name, surname, preferred username, and object ID |
| offline_access | - gives your app access to resources on behalf of the user for an extended time<br>- your app can receive refresh tokens from the Microsoft identity platform token endpoint<br>- Refresh tokens are long-lived<br>- Your app can get new access tokens as older ones expire |
## Requesting individual user consent
- In an OpenID Connect or OAuth 2.0 authorization request, an app can request the permissions it needs **by using the scope query parameter**

## Requesting consent for an entire tenant

-  applications must use the admin consent endpoint to request application permissions.