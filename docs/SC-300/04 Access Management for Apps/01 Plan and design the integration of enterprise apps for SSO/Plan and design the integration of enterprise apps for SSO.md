# Discover apps by using Microsoft Defender for Cloud Apps and Active Directory Federation Services app report


**CASB** -  Cloud Access Security Broker
- An on-premises or cloud-based security policy enforcement point, placed between cloud service consumers and cloud service providers to combine and interject enterprise security policies as the cloud-based resources are accessed.

**MDCA** - Microsoft Defender for Cloud Apps
- The Microsoft implementation of a CASB service to protect data, services, and applications with enterprise policies.

## Microsoft Defender for Cloud Apps

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-design-integration-of-enterprise-apps-for-sso/media/proxy-architecture-40b1e71f.png">


### Cloud Discovery

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-design-integration-of-enterprise-apps-for-sso/media/cloud-discovery-screenshot-602dfd9b.png">

## Active Directory Federation Services

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-design-integration-of-enterprise-apps-for-sso/media/app-integration-before-migration-147e043e.png">

To increase application security, your goal is to have a single set of access controls and policies across your on-premises and cloud environments.

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-design-integration-of-enterprise-apps-for-sso/media/app-integration-after-migration-1843eca9.png">


- The AD FS application activity report in the Azure portal enables you to quickly identify which applications you can migrate to Microsoft Entra ID.

### Types of apps to migrate

1. **SaaS applications**, which are procured by the organization.
2. **Line-of-business applications**, which are developed by the organization and not meant to be used by other companies.

### Discover AD FS applications that can be migrated

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/plan-design-integration-of-enterprise-apps-for-sso/media/active-directory-federation-services-application-activity-d02afe1d.png">
Migration status:
- **Ready to migrate**
- **Needs review**
- **Additional steps required**


# Configure connectors to apps

- **App connectors** use the APIs of app providers to enable greater visibility and control by MDCA

## Multi-instance support

- applies only to API connected apps, not to Cloud Discovered apps or Proxy connected apps.
- For example, if you've more than one instance of Salesforce (one for sales, one for marketing) you can connect both to Defender for Cloud Apps.

## Assign application owners

- when a user creates a new application registration, they're automatically added as the first owner


