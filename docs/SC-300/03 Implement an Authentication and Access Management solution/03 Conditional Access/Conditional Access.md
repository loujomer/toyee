
### Access token issuance

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-implement-administer-conditional-access/media/access-policy-token-issuance-516d80f1-982d356c.png">

#Note If no assignment is required, and no CA policy is in effect, the default behavior is to issue an access token.

## License requirements

- Free Microsoft Entra ID - No Conditional Access
- Free Office 365 subscription - No Conditional Access
- Microsoft Entra ID Premium 1 (or Microsoft 365 E3 and up) - Conditional access work based on standard rules
- Microsoft Entra ID Premium 2 - Conditional Access, and you get the ability to use Risky sign-in, Risky Users, and risk-based sign-in options as well (from Identity Protection)
----
## Sign-in risk-based Conditional Access

- A sign-in risk represents the probability that a given authentication request isn't authorized by the identity owner.
- assigned either through Conditional Access itself or through Microsoft Entra Identity Protection
- Requires Entra ID Premium P2 license
## User risk-based Conditional Access

- detects leaked username and password pairs
- assigned either through Conditional Access itself or through Microsoft Entra Identity Protection
- Requires Entra ID Premium P2 license

## Create a policy to require registration from a trusted location

1. Under **Cloud apps or actions**, select **User actions**, check **Register security information**.
2. Under **Conditions**, select **Locations**.
    1. Configure **Yes**.
    2. Include **Any location**.
    3. Exclude **All trusted locations**.
    4. Select **Done** on the **Locations** screen.
    5. Select **Done** on the **Conditions** screen.
3. Under **Conditions**, in **Client apps (Preview)**, set **Configure** to **Yes**, and select **Done**.
4. Under **Access controls**, select **Grant**.
    
    1. Select **Block access**.
    2. Then use the **Select** option.

## Block access by location

The location condition is commonly used to block access from countries/regions where your organization knows traffic should not come from.


## Require compliant devices

Organizations that have deployed Microsoft Intune can use the information returned from their devices to identify devices that meet compliance requirements, such as:

- Requiring a PIN to unlock.
- Requiring device encryption.
- Requiring a minimum or maximum operating system version.
- Requiring a device is not jailbroken or rooted.

This policy compliance information is forwarded to Microsoft Entra ID where Conditional Access can make decisions to grant or block access to resources.

#note You can enroll your new devices to Intune even if you select Require device to be marked as compliant for All users and All cloud apps using the steps above. Require device to be marked as compliant control does not block Intune enrollment.

----

## Conditional Access App Control

- Conditional Access App Control enables user app access and sessions to be monitored and controlled in real time based on **access and session policies.**
- Access and session policies are used within the Microsoft Defender for Cloud Apps portal to further refine filters and set actions to be taken on a user.
- uses a reverse proxy architecture
- After you’ve determined the conditions, you can **route users to Microsoft Defender for Cloud Apps** where you can protect data with Conditional Access App Control **by applying access and session controls.**

Access and session policies:
* **Prevent data exfiltration**
* **Protect on download**
* **Prevent upload of unlabeled files**
* **Monitor user sessions for compliance**
* **Block access**
* **Block custom activities**

## How to: Require app protection policy and an approved client app for cloud app access with Conditional Access

#note In order to require approved client apps for iOS and Android devices, these devices must first register in Microsoft Entra ID.

### Scenario 1: Microsoft 365 apps require an approved client app

**Step 1: Policy for Android and iOS based modern authentication clients requiring the use of an approved client application when accessing Exchange Online.**

- Under **Conditions**, select **Device platforms**.
    - Set **Configure** to **Yes**.
    - Include **Android** and **iOS**.

- Under **Conditions**, select **Client apps (preview)**.
    - Set **Configure** to **Yes**.
    - Select **Mobile apps and desktop clients** and **Modern authentication clients**  and **Exchange ActiveSync clients**.

- Under **Access controls**, then **Grant**, select **Grant access**, **Require approved client app**, and select **Select**

**Step 2: Configure Intune app protection policy for iOS and Android client applications.**


### Scenario 2: Exchange Online and SharePoint Online require an approved client app

**Step 1: Policy for Android and iOS based modern authentication clients requiring the use of an approved client application when accessing Exchange Online and SharePoint Online.**

1. Under **Cloud apps or actions**, then **Include**, select **Office 365 Exchange Online** and **Office 365 SharePoint Online**.

1. Under **Conditions**, select **Device platforms**.
    1. Set **Configure** to **Yes**.
    2. Include **Android** and **iOS**.

1. Under **Conditions**, select **Client apps (preview)**.
    1. Set **Configure** to **Yes**.
    2. Select **Mobile apps and desktop clients** and **Modern authentication clients** and **Exchange ActiveSync clients**.

1. Under **Access controls**, then **Grant**, select **Grant access**, **Require approved client app**, and select **Select**

**Step 2: Configure Intune app protection policy for iOS and Android client applications.**

------
## App protection policies overview

- **App protection policies (APP)** are rules that ensure an organization's data remains safe or contained in a managed app.
- A managed app has app protection policies applied to it, and it can be managed by Intune.

- **Mobile Application Management (MAM)** app protection policies allow you to manage and protect your organization's data within an application.

- You can use Intune app protection policies **independent of any mobile-device management (MDM) solution**. This independence helps you protect your company's data with or without enrolling devices in a device management solution. By implementing **app-level policies**, you can restrict access to company resources and keep data within the purview of your IT department.

### MDM vs. MAM

- **MDM (Mobile Device Management):** Focuses on managing devices. It involves enrolling a device into a management solution, enabling IT admins to control the entire device, including enforcing policies like requiring device encryption, pushing device-wide updates, or remotely wiping data.
    
- **MAM (Mobile Application Management):** Focuses on managing apps and the data within those apps, without requiring full control of the device. MAM applies policies to specific apps (e.g., preventing copy-paste or requiring a PIN to access a managed app), which is especially useful in BYOD (Bring Your Own Device) scenarios where users don’t want their entire device controlled by IT.

### APP on devices

APP can be configured for apps that run on devices that are:

- Enrolled in Microsoft Intune:
- Enrolled in a third-party MDM solution
- Not enrolled in any mobile device management solution

### Benefits of using APP

- Protecting your company data at the app level
- End-user productivity isn't affected and policies don't apply when using the app in a personal context
- APP ensure that the app-layer protections are in place
- MDM, in addition to MAM, ensures that the device is protected

---
***
# Implement session management

## User sign-in frequency

- default configuration for user sign-in frequency is a rolling window of 90 days.
- The sign-in frequency setting works with apps that have implemented **OAUTH2** or **OIDC** protocols
- The sign-in frequency setting works with SAML applications as well, as long as they don't drop their own cookies and are redirected back to Microsoft Entra ID for authentication on a regular basis.

### User sign-in frequency and device identities

- If you have Microsoft Entra joined, hybrid Microsoft Entra joined, or Microsoft Entra registered devices, when a user unlocks their device or signs in interactively, this event will satisfy the sign-in frequency policy as well

----

# Implement continuous access evaluation (CAE)

Continuous Access Evaluation (CAE) is a mechanism that enables real-time communication between a token issuer and a relying application to ensure timely enforcement of policy changes. It allows the application to detect changes in user properties, such as network location, and enables the token issuer to revoke or stop recognizing tokens due to security issues, account compromise, or other policy concerns.
### Evaluation and revocation process flow

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-implement-administer-conditional-access/media/user-revocation-event-flow-8219101e-b5213408.png">

1. A continuous access evaluation (CAE)-capable client presents credentials **or a refresh token** to Microsoft Entra ID asking for an access token for some resource.
2. An access token is returned along with other artifacts to the client.
3. An Administrator explicitly revokes all refresh tokens for the user. A revocation event will be sent to the resource provider from Microsoft Entra ID.
4. An access token is presented to the resource provider. The resource provider evaluates the validity of the token and checks whether there's any revocation event for the user. The resource provider uses this information to decide to grant access to the resource or not.
5. In the case of the diagram, the resource provider denies access, and sends a 401+ claim challenge back to the client.
6. The CAE-capable client understands the 401+ claim challenge. It bypasses the caches and goes back to step 1, sending its refresh token along with the claim challenge back to Microsoft Entra ID. Microsoft Entra ID will then reevaluate all the conditions and prompt the user to reauthenticate in this case.