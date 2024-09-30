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




