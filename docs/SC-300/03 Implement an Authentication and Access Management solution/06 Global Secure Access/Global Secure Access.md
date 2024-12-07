
Security Service Edge (SSE) - identity-aware perimeter

The Microsoft SSE solution includes (Global Secure Access):
- Microsoft Entra Internet Access
- Microsoft Entra Private Access

Licensing:

- Microsoft Entra P1 or P2 license.
- Microsoft Entra Internet Access license and / or 
- Microsoft Entra Private Access license.

Roles:

- Global Secure Access Administrator role assigned to at least one administrator.

# Explore Global Secure Access

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/deploy-configure-microsoft-entra-global-secure-access/media/global-secure-access-diagram.png">

## Microsoft Entra Internet Access

- secures access to Microsoft services, SaaS, and public internet apps through Secure Web Gateway (SWG)
### Key features

- Prevent stolen tokens replays with the compliant network check-in Conditional Access.
- Apply universal tenant restrictions to prevent data exfiltration.
- Enriched logs with network and device signals.
- Improve the precision of risk assessments on users, locations, and devices.
- Acquire network traffic from the desktop client or from a remote network.
- Dedicated public internet traffic forwarding profile.
- Protect user access to the public internet while using Microsoft secure web gateway.
- Regulate access to websites based on their content categories and domain names.
- Apply universal Conditional Access policies for all internet destinations.
## Microsoft Entra Private Access

- secure access to your private, corporate resources
- builds on the capabilities of Microsoft Entra application proxy and extends access to any private resource, port, and protocol.
### Key features

- Zero Trust based access to a range of IP addresses and/or Fully Qualified Domain Names (FQDNs) without requiring a legacy VPN.
- Modernize legacy app authentication with Conditional Access.
- Provide a seamless end-user experience by deploying side-by-side with your existing non-Microsoft SSE solutions.

----
----
# Deploy and configure Microsoft Entra Internet Access

|Steps|Description|
|---|---|
|1. Enable the Microsoft traffic forwarding profile.|With the Microsoft profile enabled, Microsoft Entra Internet Access acquires the traffic going to Microsoft services, like Exchange Online and SharePoint Online.|
|2. Install the Global Secure Access Client on end-user devices.|Download and install the client app to capture and control access from the client.|
|3. Enable tenant restrictions.|Configure which tenants / organizations are allowed to blocked|
|4. Enable enhanced Global Secure Access signaling and Conditional Access.|Use Conditional Access and Global Secure Access to prevent attacks.|

### ==Step 1:== 
Enable the Microsoft traffic forwarding profile.

1. Sign in to the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Connect > Traffic forwarding.
3. Enable the Microsoft traffic profile.

Turns on Microsoft traffic forwarding and create the following configurations in Microsoft Entra:


|Configuration Setup|Description|
|---|---|
|Policies (network routing)|1. **Exchange Online**, 2. **SharePoint Online and OneDrive for Business**, and 3. **Entra ID and MSGraph** - These use fully qualified domain names or IP subnets to manage network traffic.|
|Conditional Access Policy|**Linked Conditional Access policies** - Captures all traffic to Microsoft Services, routes to the network policies defined earlier if conditions are met.|
|User and Group|Specify specific users or groups that this traffic forward applies to.|
### ==Step 2:==
 Download the client
1. Sign in to the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Connect > Client download.
3. Select Download Client.

### ==Step 3:==
Configure the Tenant Restrictions:
- Tenant restrictions move policy management from network proxies to a cloud-based portal
- Allow internal identities, such as employees, to a**ccess specific external tenants** on your managed network. 
- Block access to nonallowed tenants for internal identities.
- Block external identities, such as contractors and vendors, from accessing all external tenants.

Set up Tenant Restrictions

- Browse to Identity > External Identities > Cross-tenant access settings, then select Organizational settings.
- Select **Add organization**.
- On the **Add organization** pane, type the full domain name (or tenant ID) for the organization.
- Select the organization in the search results, and then select Add.
- Modify the organization's settings.

##### Enable Global Secure Access

1. Sign in to the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Global Settings > Session Management > Tenant Restrictions.
3. Select the toggle to Enable tagging to enforce tenant restrictions on your network.
4. Select Save.

##### How it works

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/deploy-configure-microsoft-entra-global-secure-access/media/tenant-restrictions-flow.png">

|Steps|Description|
|---|---|
|1.|Contoso configures a **tenant restrictions v2 ** policy in their cross-tenant access settings to block all external accounts and external apps. Contoso enforces the policy using Global Secure Access universal tenant restrictions.|
|2.|A user with a Contoso-managed device tries to access a Microsoft Entra integrated app with an unsanctioned external identity.|
|3.|Authentication plane protection: Microsoft Entra ID, with Contoso's policy, blocks unsanctioned external accounts from accessing external tenants.|
|4.|Data plane protection: If the user again tries to access an external unsanctioned application by copying an authentication response token they obtained outside of Contoso's network and pasting it into the device, are blocked. The token mismatch triggers reauthentication and blocks access. For SharePoint Online, any attempt at anonymously accessing resources is blocked.|

#### ==Step 4:==
Enable enhanced Global Secure Access signaling and Conditional Access

- Global Secure Access introduces the concept of a compliant network within Conditional Access

#### Enable Global Secure Access signaling

1. Sign into the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Global settings > Session management Adaptive access.
3. Select the toggle to Enable Global Secure Access signaling in Conditional Access.
4. Browse to Protection > Conditional Access > Named locations.

	Confirm you have a location called All Compliant Network locations with location type Network Access. Organizations can optionally mark this location as trusted.

#### Build your Conditional Access policy for networks
1. Sign into the Microsoft Entra admin center as at least a Conditional Access Administrator.
2. Browse to Protection > Conditional Access.
3. Select Create new policy.
4. Give your policy a name. We recommend that organizations create a meaningful standard for the names of their policies.
5. Look at Assignments, then select Users or workload identities.
    - Under Include, select All users.
    - Under Exclude, select Users and groups and choose your organization's emergency access or break-glass accounts.
6. Review Target resources > Include, and select Select apps.
    - Choose Office 365 Exchange Online, and/or Office 365 SharePoint Online, and/or any of your SaaS apps.
    - The specific Office 365 cloud app in the app picker is currently NOT supported, so don't select this cloud app.
7. ==Review the Conditions > Location.==
    - ==Set Configure to Yes.==
    - ==Under Include, select Any location.==
    - ==Under Exclude, select Selected locations.==
        - ==Select the All Compliant Network locations.==
    - ==Choose Select.==
8. Explore Access controls:
    - Grant, select Block Access, and select Select.
9. Confirm your settings and set Enable policy to On.
10. Select the Create button to create to enable your policy.

----
----
# Deploy and configure Microsoft Entra Private Access

|Steps|Description|
|---|---|
|1. Configure a Microsoft Entra private network connector and connector group.|Create connection between an on-premises server and Global Secure Access.|
|2. Configure Quick Access to your private resources.|Define specific fully qualified domain names (FQDNs) or IP addresses of private resources to include in Microsoft Entra Private Access.|
|3. Enable the Private Access traffic forwarding profile.|Turn on Private Access and link from on-premises router to remote networks.|
|4. Install and configure the Global Secure Access Client on end-user devices.|Deploy the client software onto devices, so they can access the traffic flow.|
## ==Step 1:==

Configure a Microsoft Entra private network connector and connect groups

##### Connectors
- lightweight agents that sit on a server in a private network
- must be installed on a Windows Server that has access to the backend resources and applications
- can be organized into connector groups, with each group handling traffic to specific applications.
- facilitate the outbound connection to the Global Secure Access service.

##### Configuring the Windows Server for connectors
- The Microsoft Entra private network connector requires a server running Windows Server 2012 R2 or later
- This connector server needs to connect to the following:
	- Microsoft Entra Private Access service or application proxy service
	- the private resources or applications that you plan to publish.

- The minimum .NET version required for the connector is v4.7.1+.
- Requires Transport Layer Security (TLS) 1.2 be enabled on Windows Server.

Open ports for **outbound**

|Port number|What the port is used for|
|---|---|
|80|Downloading certificate revocation lists (CRLs) while validating the TLS/SSL certificate|
|443|All outbound communication with the Application Proxy service|

Allow access to some URLs


|URL|Port|What the port is used for|
|---|---|---|
|`site`.msappproxy.net, and `site`.servicebus.windows.net|443/HTTPS|Communication between the connector and the Application Proxy cloud service|
|crl3.digicert.com, crl4.digicert.com, ocsp.digicert.com, crl.microsoft.com, oneocsp.microsoft.com, and ocsp.msocsp.com|80/HTTP|The connector uses these URLs to verify certificates.|
|login.windows.net, secure.aadcdn.microsoftonline-p.com, `site`.microsoftonline.com, `site`.microsoftonline-p.com, `site`.msauth.net, `site`.msauthimages.net, `site`.msecnd.net, `site`.msftauth.net, `site`.msftauthimages.net, `site`.phonefactor.net, enterpriseregistration.windows.net, management.azure.com, policykeyservice.dc.ad.msft.net, ctldl.windowsupdate.com, and [www.microsoft.com/pkiops](https://www.microsoft.com/pkiops)|443/HTTPS|The connector uses these URLs during the registration process.|
|ctldl.windowsupdate.com, and [www.microsoft.com/pkiops](https://www.microsoft.com/pkiops)|80/HTTP|The connector uses these URLs during the registration process.|

#### Install the connector using Microsoft Entra

1. Sign into the Microsoft Entra admin center as a Global Administrator of the directory that uses Application Proxy.
2. Select your username in the upper-right corner. Verify sign-in to a directory that uses Application Proxy. If you need to change directories, select Switch directory and choose a directory that uses Application Proxy.
3. Browse to Global Secure Access > Connect > Connectors.
4. Select Download connector service.

#### Verify the connector installed

**On Windows Server:**
1. Select the Windows key and enter services.msc to open the Windows Services Manager.
2. Check to see if the status for the following services Running.
    - Microsoft Entra private network connector enables connectivity.
    - Microsoft Entra private network connector Updater is an automated update service.
    - The updater checks for new versions of the connector and updates the connector as needed.
3. If the status for the services isn't Running, right-click to select each service and choose Start.

**In Microsoft Entra:**
1. Sign in to the Microsoft Entra admin center as a Global Administrator of the directory that uses Application Proxy.
2. Browse to Global Secure Access > Connect > Connectors.
    - All of your connectors and connector groups appear on this page.
3. Verify the details by viewing the connector.
	- Expand the connector to view the details.
	- An active green label indicates that your connector can connect to the service. However, even though the label is green, a network issue could still block the connector from receiving messages.

#### Create groups of connectors

1. For quicker assignments, you can group different connectors together.
2. Browse to Global Secure Access > Connect > Connectors.
3. Select New connector group.


## ==Step 2:==

Configure Quick Access for Global Secure Access

- With Global Secure Access, you can define specific fully qualified domain names (FQDNs) or IP addresses ==of private resources to include in the traffic== for Microsoft Entra Private Access.
-  Your organization's employees can then access the apps and sites that you specify.

#### Set up Quick Access name and connector group
1. Sign in to the Microsoft Entra admin center with the appropriate roles.
2. Browse to Global Secure Access > Applications > Quick access.
3. Enter a name. We recommend using the name Quick Access.
4. Select a Connector group from the dropdown menu. Existing connector groups appear in the dropdown menu.
    - Created in the previous step.
5. Select the Save button at the bottom of the page to create your "Quick Access" app without FQDNs and IP addresses.

#### Add an application segment
- is where you define the FQDNs and IP addresses that you want to include in the traffic for Microsoft Entra Private Access

1. Sign in to the Microsoft Entra admin center.
2. Browse to Global Secure Access > Applications > Quick Access.
3. Select **Add Quick Access** application segment.
4. In the **Create application segment** panel, select a Destination type.
5. Enter the appropriate details for the selected destination type. Depending on what you select, the subsequent fields change accordingly.
6. Enter the appropriate details for the selected destination type. Depending on what you select, the subsequent fields change accordingly.

- IP address:
    
    - Internet Protocol version 4 (IPv4) address, such as 192.0.2.1, that identifies a device on the network.
    - Provide the ports that you want to include.
- Fully qualified domain name (including wildcard FQDNs):
    
    - Domain name that specifies the exact location of a computer or a host in the Domain Name System (DNS).
    - Provide the ports to include.
    - NetBIOS isn't supported. For example, use contoso.local/app1 instead of contoso/app1.
- IP address range (CIDR):
    
    - Classless Inter-Domain Routing (CIDR) represents a range of IP addresses. An IP address is followed by a suffix indicating the number of network bits in the subnet mask.
    - For example, 192.0.2.0/24 indicates that the first 24 bits of the IP address represent the network address, while the remaining 8 bits represents the host address.
    - Provide the starting address, network mask, and ports.
- IP address range (IP to IP):
    
    - Range of IP addresses from start IP (such as 192.0.2.1) to end IP (such as 192.0.2.10).
    - Provide the IP address start, end, and ports.
- Enter the ports and select the Apply button.
    
    - Separate multiple ports with a comma.
    - Specify port ranges with a hyphen.
    - Spaces between values are removed when you apply the changes.
    - For example, 400-500, 80, 443.
    

The following table provides the most commonly used ports and their associated networking protocols:

|Port|Protocol|
|---|---|
|22|Secure Shell (SSH)|
|80|Hypertext Transfer Protocol (HTTP)|
|443|Hypertext Transfer Protocol Secure (HTTPS)|
|445|Server Message Block (SMB) file sharing|
|3389|Remote Desktop Protocol (RDP)|
#### Assign users and groups for Quick Access

1. Sign in to the Microsoft Entra admin center.
2. Browse to Global Secure Access > Applications > Quick Access.
3. Select the Edit application settings button from Quick Access.
4. Select Users and groups from the side menu.
5. Add users and groups as needed.

You can enable specific Conditional Access policies as needed.

## ==Step 3==

Enable Traffic forwarding - Microsoft Entra Private Access

- The Private Access traffic forwarding profile **routes traffic to your private network** through the Global Secure Access Client.
- Enabling this traffic forwarding profile allows remote workers to connect to internal resources without a VPN.
- With the features of Microsoft Entra Private Access, you can control which private resources to tunnel through the service and apply Conditional Access policies to secure access to those services.

1. Sign in to the Microsoft Entra admin center.
2. Browse to Global Secure Access > Connect > Traffic forwarding.
3. Select the checkbox for Private access profile.

## ==Step 4==

Deploy Global Secure Access client for Windows (or Android)

1. Sign in to the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Connect > Client download.
3. Select Download Client.

--- 
# Explore how to use the Dashboard to drive Global Secure Access


- provides you with visualizations of the network traffic acquired by the Microsoft Entra Private and Microsoft Entra Internet Access services.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/deploy-configure-microsoft-entra-global-secure-access/media/dashboard-global-secure-access.png">
## Global Secure Access snapshot

- This widget provides a summary of how many users and devices are using the service and how many applications were secured through the service

## Alerts and notifications (preview)

- This widget shows what is happening in the network and helps identify suspicious activities or trends identified by the network data

## Usage profiling (preview)

- displays usage patterns over a selected period of time.

## Top used destinations

- shows all types of ==traffic== and sorts by the number of transactions.

## Cross-tenant access

- Global Secure Access provides visibility into the number of users and devices that are accessing other tenants.

## Web category filtering

- displays the top categories of web content that are blocked or allowed

## Device status

- display the active and inactive devices that you deployed

----
---
# Create remote networks for use with Global Secure Access

- Remote networks are remote locations, such as a branch office, or networks that require internet connectivity
- Setting up remote networks connects your users in remote locations to Global Secure Access.
- Once a remote network is configured, you can assign a traffic forwarding profile to manage your corporate network traffic.

- There are multiple ways to connect remote networks to Global Secure Access. 
- In a nutshell, you're creating an **Internet Protocol Security (IPSec) tunnel** between:
	- a core router at your remote network and 
	- the nearest Global Secure Access endpoint.
- All internet-bound traffic is routed through the core router of the remote network for security policy evaluation in the cloud
- Installation of a client isn't required on individual devices

|Steps|Description|
|---|---|
|1. Basics |Define the name of your remote network and the region where you want to connect.|
|2. Connectivity |Enter the data about your on-premises router, where the signal comes from.|
|3. Traffic Forwarding |Add a traffic forwarding profile to define the type traffic network traffic to allow through.|
|4. Review Configuration |In the step your confirm the setup of the remote network and gather settings you need to configure in the on-premises router.|
|5 etup on-premises router |Use the management console of your on-premises router to enter the Microsoft connectivity setting from previous step.|

---
---
# Use Conditional Access with Global Secure Access

new types of checks introduced into Conditional Access with Global Secure Access:

- Compliant network check
- Private Access apps
- Source IP restoration

To use the **Compliant network check** and the **Source IP restoration** capabilities, you need to have **Global Secure Access signaling for Conditional Access** enabled.

## Enable Global Secure Access signaling for Conditional Access

To enable the required setting to allow the compliant network check, an administrator must take the following steps.

1. Sign in to the Microsoft Entra admin center as a Global Secure Access Administrator.
2. Browse to Global Secure Access > Global settings > Session management Adaptive access.
3. Select the toggle to Enable CA Signaling for Microsoft Entra ID (covering all cloud apps). Continuous Access Evaluation (CAE) signaling is automatically enabled for Office 365 (preview).
4. Browse to Protection > Conditional Access > Named locations.
    - Confirm you have a location called All Compliant Network locations with location type Network Access. Organizations can optionally mark this location as trusted.

## Compliant Network Check

- Compliant network enforcement occurs at both the authentication and data planes. Microsoft Entra ID handles enforcement during user authentication, while data plane enforcement works with services supporting Continuous Access Evaluation (CAE), currently limited to Exchange Online and SharePoint Online. CAE provides defense-in-depth with token theft replay protection.

#### Protect your resources behind the compliant network
- The compliant network Conditional Access policy can be used to protect your Microsoft and other applications. A typical policy will 'Block' access for all network locations except Compliant Network.

1. Look in Target resources > Include, and choose Select apps.
    - Choose Office 365 Exchange Online, and/or Office 365 SharePoint Online, and/or any of your SaaS apps.
    - The specific Office 365 cloud app in the app picker is currently NOT supported, so don't select this cloud app.
2. Explore Conditions > Location.
    - Set Configure to Yes.
    - Under Include, select Any location.
    - Under Exclude, choose Selected locations.
        - Select the ==All Compliant Network locations.==
    - Choose Select.
3. Under Access controls:
    - Grant, select Block Access, and choose Select.

## Conditional Access for Private Access apps


## Enriched Office 365 logs (preview)

#### Enable the log enrichment

To enable the Enriched Microsoft 365 logs:

1. Sign in to the Microsoft Entra admin center as a Global Administrator.
2. Browse to Global Secure Access > Global settings > Logging.
3. Select the type of Microsoft 365 logs you want to enable.
4. Select Save.