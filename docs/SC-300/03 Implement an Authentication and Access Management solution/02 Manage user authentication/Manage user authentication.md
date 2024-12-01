### How each authentication method works
| **Method** | **Primary authentication** | **Secondary authentication** |
| ---- | ---- | ---- |
| Windows Hello for Business | Yes | MFA |
| Microsoft Authenticator app | Yes (preview) | MFA and SSPR |
| FIDO2 security key | Yes | MFA |
| OATH hardware tokens (preview) | No | MFA and SSPR |
| OATH software tokens | No | MFA and SSPR |
| SMS | Yes (preview) | MFA and SSPR |
| Voice call | No | MFA and SSPR |
| Password | Yes |  |

## What is FIDO2 (Fast IDentity Online)

- FIDO  is an open specification for passwordless authentication
- FIDO2 is the latest specification that incorporates the web authentication (WebAuthn) specification
- FIDO2 security keys are an unphishable specification-based passwordless authentication method that can come in any form factor

## Enable FIDO2 security key method

Entra admin -> Identity -> Protection -> Authentication methods -> Policy

### Manage user registration and FIDO2 security keys

go to aka.ms/mfasetup

-----
# Explore Authenticator app and OATH tokens

## Open Authentication (OATH) tokens
- OATH TOTP (Time-based One Time Password) is an open standard that specifies how one-time password (OTP) codes are generated
- Microsoft Entra ID doesn't support OATH HOTP (Hash based), a different code generation standard
- OATH TOTP can be implemented using either software or hardware to generate the codes

---
# Implement an authentication solution based on Windows Hello for Business

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/manage-user-authentication/media/authentication-flow-d3e33dbf.png">


## How Windows Hello for Business works: key points

- Windows Hello credentials are based on certificate or asymmetrical key pair
- Windows Hello credentials can be bound to the device, and the token that is obtained using the credential is also bound to the device.
- Identity provider (such as Active Directory, Microsoft Entra ID, or a Microsoft account) validates user identity and maps the Windows Hello public key to a user account during the registration step.
- Keys can be generated in hardware (TPM (Trusted Platform Module) 1.2 or 2.0 for enterprises, and TPM 2.0 for consumers) or software, based on the policy.
- Two-factor authentication is the combination of a key or certificate tied to a device and something that the person knows (a PIN) or something that the person is (biometrics)
- The Windows Hello gesture doesn't roam between devices and isn't shared with the server. Biometrics templates are stored locally on a device. The PIN is never stored or shared.
- The private key never leaves a device when using TPM. The authenticating server has a public key that is mapped to the user account during the registration process.
- PIN entry and biometric gesture both trigger Windows 10 to use the private key to cryptographically sign data that is sent to the identity provider. The identity provider verifies the user's identity and authenticates the user.
- Personal (Microsoft account) and corporate (Active Directory or Microsoft Entra ID) accounts use a single container for keys. All keys are separated by identity providers' domains to help ensure user privacy.
- Certificate private keys can be protected by the Windows Hello container and the Windows Hello gesture.

## Creating security groups

#Note If your environment has one or more Windows Server 2016 domain controllers in the domain to which you are deploying Windows Hello for Business, then skip the Create the KeyCredentials Admins Security Group. Domains that include Windows Server 2016 domain controllers use the **KeyAdmins** group, which is created during the installation of the first Windows Server 2016 domain controller.

### Create the KeyCredential Admins security group

Microsoft Entra Connect synchronizes the public key on the user object created during provisioning. You assign write and read permission to this group to the Active Directory attribute. This will ensure the Microsoft Entra Connect service can add and remove keys as part of its normal workflow.

1. Sign in a domain controller or management workstation with _Domain Admin_ equivalent credentials.
2. Open **Active Directory Users and Computers**.
3. Select **View** and select **Advance Features**.
4. Expand the domain node from the navigation pane.
5. Right-select the **Users** container. Select **New**. Select **Group**.
6. Type **KeyCredential Admins** in the **Group Name** text box.
7. Select **OK**.

### Create the Windows Hello for Business Users security group

The Windows Hello for Business Users group is used to make it easy to deploy Windows Hello for Business in phases. You assign Group Policy and Certificate template permissions to this group to simplify the deployment by adding the users to the group. This provides users with the proper permissions to configure Windows Hello for Business and to enroll in the Windows Hello for Business authentication certificate.

1. Sign in a domain controller or management workstation with _Domain Admin_ equivalent credentials.
2. Open **Active Directory Users and Computers**.
3. Select **View** and select **Advanced Features**.
4. Expand the domain node from the navigation pane.
5. Right-select the **Users** container. Select **New**. Select **Group**.
6. Type **Windows Hello for Business Users** in the **Group Name** text box.
7. Select **OK**.

-----
# SSPR

License required: 

- Cloud based accounts
	- Microsoft Entra ID Premium P1 or P2 license or
	- Microsoft 365 Business Standard
- On-premises accounts
	- Microsoft Entra ID Premium P1 or P2 license
	- Microsoft 365 Business Premium

## Enable self-service password reset

Entra admin -> Identity -> Protection -> Password reset -> Manage -> Properties

## Register for self-service password reset

go to aka.ms/ssprsetup

## Test self-service password reset

go to aka.ms/sspr

----
# Deploy and manage password protection

Microsoft Entra Password Protection provides a global and custom banned password list

## How Microsoft Entra Password Protection works

The on-premises Microsoft Entra Password Protection components work as follows:

1. Each Microsoft Entra Password Protection proxy-service-instance advertises itself to the DCs in the forest by creating a _serviceConnectionPoint_ object in Active Directory.
2. Each DC Agent service for Microsoft Entra Password Protection also creates a _serviceConnectionPoint_ object in Active Directory. This object is used primarily for reporting and diagnostics.
3. The DC Agent service is responsible for initiating the download of a new password policy from Microsoft Entra. The first step is to locate a Microsoft Entra Password Protection proxy-service by querying the forest for proxy _serviceConnectionPoint_ objects.
4. When an available proxy service is found, the DC Agent sends a password policy download request to the proxy service. The proxy service in turn sends the request to Microsoft Entra, and then returns the response to the DC Agent service.
5. After the DC Agent service receives a new password policy from Microsoft Entra, the service stores the policy in a dedicated folder at the root of its domain _sysvol_ folder share. The DC Agent service also monitors this folder in case newer policies replicate in from other DC Agent services in the domain.
6. The DC Agent service always requests a new policy at service startup. After the DC Agent service is started, it checks the age of the current locally available policy **hourly**. If the policy is older than one hour, the DC Agent requests a new policy from Microsoft Entra via the proxy service, as described previously. If the current policy isn't older than one hour, the DC Agent continues to use that policy.
7. When password change events are received by a DC, **the cached policy is used** to determine if the new password is accepted or rejected.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/manage-user-authentication/media/azure-active-directory-password-protection-e783cbb6.png">
#Note Microsoft Entra Password Protection can only validate passwords during password change or set operations. Passwords that were accepted and stored in Active Directory prior to the deployment of Microsoft Entra Password Protection will never be validated and will continue working as is. Over time, all users and accounts will eventually start using Microsoft Entra Password Protection-validated passwords as their existing passwords expire. Accounts configured with "password never expires" are exempt from this.

### Multiple forest considerations

- no additional requirements to deploy Microsoft Entra Password Protection across multiple forests
- Each forest is independently configured
- Each Microsoft Entra Password Protection proxy can only support domain controllers from the forest that it's joined to.
- The Microsoft Entra Password Protection software in any forest is unaware of password protection software that's deployed in other forests, regardless of Active Directory trust configurations.

### Read-only domain controller considerations

- Password change or set events aren't processed and persisted on read-only domain controllers (RODCs)
- Instead, they're forwarded to writable domain controllers.
- You don't have to install the Microsoft Entra Password Protection DC agent software on RODCs.
- It's not supported to run the Microsoft Entra Password Protection proxy-service on a read-only domain controller.

### High availability considerations

- Each Microsoft Entra Password Protection DC agent uses a simple round-robin-style algorithm when deciding which proxy server to call. The agent skips proxy servers that aren't responding.
- For most fully connected Active Directory deployments that have healthy replication of both directory and sysvol folder state, two Microsoft Entra Password Protection proxy servers is enough to ensure availability
- The design of the Microsoft Entra Password Protection DC agent software mitigates the usual problems that are associated with high availability. The Microsoft Entra Password Protection DC agent maintains a local cache of the most recently downloaded password policy. Even if all registered proxy servers become unavailable, the Microsoft Entra Password Protection DC agents continue to enforce their cached password policy.
- A reasonable update frequency for password policies in a large deployment is usually days, not hours or less. So, brief outages of the proxy servers don't cause problems for Microsoft Entra Password Protection.

## Deployment requirements

|**Users**|**Microsoft Entra Password Protection with global banned password list**|**Microsoft Entra Password Protection with custom banned password list**|
|---|---|---|
|Cloud-only users|Microsoft Entra Free|Microsoft Entra Premium P1 or P2|
|Users synchronized from on-premises AD DS|Microsoft Entra Premium P1 or P2|Microsoft Entra Premium P1 or P2|
- You need an account that has Active Directory domain administrator privileges in the forest root domain to register the Windows Server Active Directory forest with Microsoft Entra.
- The Key Distribution Service must be enabled on all domain controllers in the domain that run Windows Server 2012. By default, this service is enabled via manual trigger start.
- Network connectivity must exist between at least one domain controller in each domain and at least one server that hosts the proxy service for Microsoft Entra Password Protection. This connectivity must allow the domain controller to access RPC endpoint mapper port 135 and the RPC server port on the proxy service.

    - By default, the RPC server port is a dynamic RPC port, but it can be configured to use a static port.
- All machines where the Microsoft Entra Password Protection proxy-service will be installed must have network access to the following endpoints:

|**Endpoint**|**Purpose**|
|---|---|
|`https://login.microsoftonline.com`|Authentication requests|
|`https://enterpriseregistration.windows.net`|Microsoft Entra Password Protection functionality|

### Microsoft Entra Password Protection DC agent requirements

- All machines where the Microsoft Entra Password Protection DC agent software will be installed must run Windows Server 2012 or later.
- All machines that run the Microsoft Entra Password Protection DC agent must have .NET 4.5 installed.
- Any Active Directory domain that runs the Microsoft Entra Password Protection DC agent service must use Distributed File System Replication (DFSR) for sysvol replication.

### Microsoft Entra Password Protection proxy service requirements

- All machines where the Microsoft Entra Password Protection proxy-service will be installed must run Windows Server 2012 R2 or later.
- All machines where the Microsoft Entra Password Protection proxy-service will be installed must have .NET 4.7 installed.
- All machines that host the Microsoft Entra Password Protection proxy-service must be configured to grant domain controllers the ability to sign into the proxy service. This ability is controlled via the "Access this computer from the network" privilege assignment.
- All machines that host the Microsoft Entra Password Protection proxy-service must be configured to allow outbound TLS 1.2 HTTP traffic.
- A _Global Administrator_ or _Security Administrator_ account is required to register the Microsoft Entra Password Protection proxy-service and forest with Microsoft Entra.
- Network access must be enabled for the set of ports and URLs specified in the Application Proxy environment setup procedures.

#WARNING You should never install Microsoft Entra Password Protection Proxy and Application Proxy on the same machine

## Download required software

- Microsoft Entra Password Protection DC agent (_AzureADPasswordProtectionDCAgentSetup.msi_)
- Microsoft Entra Password Protection proxy (_AzureADPasswordProtectionProxySetup.exe_)

## Install and configure the proxy service

The Microsoft Entra Password Protection proxy-service is typically on a member server in your on-premises AD DS environment. Once installed, the Microsoft Entra Password Protection proxy-service communicates with Microsoft Entra to maintain a copy of the global and customer banned password lists for your Microsoft Entra tenant.

The software installation, or uninstallation, requires a restart. This requirement is because password filter DLLs are only loaded or unloaded by a restart.

----

# Configure smart lockout thresholds

- By default, smart lockout locks the account from sign-in attempts for one minute after 10 failed attempts.
- The account locks again after each subsequent failed sign-in attempt, for one minute at first and then longer in subsequent attempts
- Smart lockout tracks the last three bad-password hashes to avoid incrementing the lockout counter for the same password. If someone enters the same bad password multiple times, this behavior won't cause the account to lock out.
- Smart lockout is always on, for all Microsoft Entra ID customers
- Entra ID P1 or higher is required for customization

The following considerations apply:
* Each Microsoft Entra data center tracks lockouts independently
* Smart lockout uses _familiar location_ versus _unfamiliar location_ to differentiate between a bad actor and the genuine user. 
* Unfamiliar and familiar locations both have separate lockout counters.

When the admin configures pass-through authentication, the following considerations apply:

- The Microsoft Entra lockout threshold is less than the AD DS account lockout threshold. Set the values so that the AD DS account lockout threshold is at least two or three times greater than the Microsoft Entra lockout threshold.
- The Microsoft Entra lockout duration must be set longer than the AD DS account lockout duration. The duration is set in seconds, while the AD duration is set in minutes.

---
# Implement Kerberos and certificate-based authentication in Microsoft Entra ID

- You can provide single sign-on for on-premises applications published through Application Proxy
- Application Proxy uses Kerberos Constrained Delegation (KCD) to support these applications.
- The apps are secured with integrated Windows authentication.

### Kerberos authentication process flow

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/manage-user-authentication/media/kerberos-authentication-cdf4cd35.png">
1. The user enters the URL to access the on premises application through Application Proxy.
2. Application Proxy redirects the request to Microsoft Entra authentication services to preauthenticate. At this point, Microsoft Entra ID applies any applicable authentication and authorization policies, such as multifactor authentication. If the user is validated, Microsoft Entra ID creates a token and sends it to the user.
3. The user passes the token to Application Proxy.
4. Application Proxy validates the token and retrieves the User Principal Name (UPN) from it, and then the Connector pulls the UPN, and the Service Principal Name (SPN) through a dually authenticated secure channel.
5. The Connector performs Kerberos Constrained Delegation (KCD) negotiation with the on premises AD, impersonating the user to get a Kerberos token to the application.
6. Active Directory sends the Kerberos token for the application to the Connector.
7. The Connector sends the original request to the application server, using the Kerberos token it received from AD.
8. The application sends the response to the Connector, which is then returned to the Application Proxy service and finally to the user.

----
# Configure Microsoft Entra user authentication for virtual machines

