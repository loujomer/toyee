* Discover and control the use of Shadow IT
* Protect your sensitive information anywhere in the cloud
* Protect against cyberthreats and anomalies
* Assess the compliance of your cloud apps

## Defender for Cloud Apps architecture

### Cloud Discovery
* uses an organization's traffic logs to dynamically discover and analyze the cloud apps that it's using

### Sanctioning and unsanctioning an app
* Microsoft ranked and scored the apps based on industry standards
* Microsoft Defender for Cloud Apps lets an organization know how risky an app is

### App connectors
* facilitate the integration between the Cloud App Security service and cloud applications
* the app administrator authorizes Microsoft Defender for Cloud Apps to access the app
* Then, Microsoft Defender for Cloud Apps queries the app for activity logs, and it scans data, accounts, and cloud content
* Microsoft Defender for Cloud Apps can enforce policies, detects threats, and provides governance actions for resolving issues.

### Conditional Access App Control protection
* uses reverse proxy architecture
* Organizations typically deploy the reverse proxy server in the same network segment as the application server. 
	* The proxy server routes incoming traffic to the appropriate backend server based on the requested URL. 
	* For Microsoft Defender for Cloud Apps, organizations configure the reverse proxy server to redirect traffic to the Microsoft Defender proxy server for inspection and policy enforcement before forwarding it to the application server.

### Policy control
* detect and remediate

-----------
# Deploy Microsoft Defender for Cloud Apps

## Pre-requisites
* must obtain a license for every user protected
* you'll receive an email with activation information and a link to the MDCA portal
* must be a Global admin or Security admin 

## Deployment steps

###### Step 1. Set instant visibility, protection, and governance actions for your apps (REQUIRED) 

* System -> Settings -> Cloud apps -> Connected Apps -> App Connectors -> Connect an app 
###### Step 2.  Protect sensitive information with DLP policies (Recommended)

* System -> Settings -> Cloud apps -> Information Protection  
* Files -> Enable File monitoring
* Microsoft Information Protection

###### Step 3. Control cloud apps with policies (REQUIRED)

*  Cloud apps -> Policies -> Policy management
* use policies to help monitor trends, see security threats, and generate customized reports and alerts
* create governance actions, and set data loss prevention and file-sharing controls

###### Step 4. Set up Cloud Discovery (REQUIRED)

* Integrate with Microsoft Defender for Endpoint or Zscaler
* System -> Settings -> Cloud apps  -> Cloud Discovery -> Automatic log upload -> Add data source
* Snapshot reports
* Continuous reports

###### Step 5. Deploy Conditional Access App Control for catalog apps (RECOMMENDED)

* If you have Microsoft Entra ID, you can use inline controls such as **Monitor only** and **Block downloads**.
* System -> Settings -> Cloud apps -> Connected Apps -> Conditional Access App Control apps 

###### Step 6. Personalize your experience (RECOMMENDED)

* System -> Settings -> Cloud apps -> System -> Mail settings
* System -> Settings -> Cloud apps -> My account -> My email notifications
* System -> Settings -> Cloud apps  -> Cloud Discovery ->Score metrics

###### Step 7. Organize the data according to your needs (RECOMMENDED)

To create IP address tags
*  it's easier to create policies that fit an organization's needs, accurately filter data, and create continuous reports.
* System -> Settings -> System -> IP address ranges

To add domains
* System -> Settings -> System -> Organization details

-----------------

# File policies in Cloud Apps
* can monitor any file type based on more than 20 metadata filters (for example, access level, file type, and so on)


The Microsoft Defender for Cloud Apps engine combines the following three aspects under each policy:
1. Content scan based on preset templates or custom expressions.
2. Context filters
	* User roles
	* File metadata
	* Sharing level
	* Organizational group integration
	* Collaboration context
	* Other customizable attributes
3. Automated actions for governance and remediation.

**Note**
> The first triggered policy guarantees the ==only== governance action the system applies. For example, if a file policy has already applied a sensitivity label to a file, a second file policy can't apply another sensitivity label to it.

----------------
## Policy types

1. Activity
	* uses app APIs to monitor specific activities by users or follow unexpectedly high rates of a certain type of activity 

2. File
	* scan cloud apps for:
		* specified file types - shared, shared with external domains
		* data - proprietary info, personal data, credit card info, other types of data

3. Malware detection
	* identify malicious files in the cloud storage
	* automatically approve or revoke it

4. Anomaly detection
	* look for unusual activities on the organization's cloud
	* detection is based on risk factors you set
	* risk factors alerts you when something happens that's outside the org's baseline or from user's activity

5. App discovery
	* set alerts to notify when new app is detected

6. Access
	* real time monitoring and control over user logins to its cloud apps

7. Session
	* real time monitoring and control oevr users activity in its cloud apps

8. OAuth app
	* enable orgs to investiagte which permissions each OAuth app requested, and then automatically approve or revoke them

9. OAuth app anomaly detection

------
## Identifying Risk

* you can configure any policy and alert to be associated with one of the risks below

1. Threat detection
	* Are there suspicious activities threatening your cloud environment?	
	* detect behavior that could indicate that a user is misusing data
2. Privileged accounts
	* Do you need to monitor admin accounts?	
3. Compliance
	* Are your compliance requirements breached?	
	* Catalog and identify sensitive or regulated data stored in file-sync services, such as sharing permissions for each file.
4. DLP
	* Are proprietary files shared publicly?
	* Do you need to quarantine files?
5. Cloud discovery
	* Are new apps used in your organization?	
6. Sharing control
	* How is data shared in your cloud environment?	
7. Access control
	* Who accesses what from where?	
	* Continuously monitor behavior and detect anomalous activities, including high-risk insider and external attacks
	* Detect suspicious sign-in events, including multifactor authentication failures, disabled account sign-in failures, and impersonation events.
	* apply a policy to alert, block, or require identity verification for any app or specific action within an app.
	* Enables on-premises and mobile access control policies based on user, device, and geography with coarse blocking and granular view, edit, and block.
9. Configuration control
	* Are unauthorized changes made to your configuration?	

When you create a new policy, the system automatically enables it.

----
# Manage and respond to alerts in Microsoft Defender for Cloud Apps

Alerts ->  Add filter -> Status, Service/detection sources -> Microsoft Defender for Cloud Apps


There are three types of violations you must deal with when investigating alerts:
1. Serious violations. Require immediate response. 
2. Questionable violations. Require further investigation. 
3. Authorized violations or anomalous behavior. Can result from legitimate use.

## Alert types

1. Activity policy violation
2. File policy violation	
3. Compromised account
4. Inactive account	
5. New admin user	
6. New admin location	
7. New location	
8. New discovered service	
9. Suspicious activity	
10. Use of personal account	
