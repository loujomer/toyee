I PASSED!!!
## Elements of DLP

- Where to protect the content
- When and how to protect the content
	- Conditions
	- Actions

## Identify content to protect

- use the Data Classification solution to find what sensitive information will need to be protected
- use Content explorer and Activity explorer

Content explorer

<img src="https://learn.microsoft.com/en-us/training/wwl/m365-compliance-information-prevent-data-loss/media/data-classification.png">

Activity explorer

<img src="https://learn.microsoft.com/en-us/training/wwl/m365-compliance-information-prevent-data-loss/media/activity-explorer.png">

---
## Identify sensitive data with optical character recognition (preview)

- enables Microsoft Purview to scan content in images for sensitive information
- supports more than 150 languages
- optional feature that is first turned on at the tenant level
- after it's enabled, you choose the locations where you want to scan images
	- Exchange -for outgoing emails only
	- SharePoint
	- OneDrive
	- Teams
	- Windows devices
- Once set up, the following policies will be applied:
	- data loss prevention (DLP)
	- records management
	- insider risk management (IRM)

### Workflow at a glance

Phase 1: Create [Azure subscription](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/initial-subscriptions) if needed
Phase 2: Set up [==pay-as-you-go billing==](https://learn.microsoft.com/en-us/purview/ocr-learn-about#phase-2-configure-billing) to enable OCR.	
Phase 3: Configure OCR scanning settings	

#### Configure your OCR settings

1. In the Microsoft Purview compliance portal, go to Settings.
2. Select **Optical character recognition (OCR) (preview)** to enter your OCR configuration settings.
3. Select the locations you want to scan images. Then, for each location and solution, define the scope (users/groups/sites) for the OCR. 

OCR settings generally take effect about an hour after being turned on.

### Supported file types

- JPEG, JPG, PNG, BMP, TIFF, and PDF (image only)
- File size < 20 MB (Exchange and Teams)
- File size < 50 MB (SharePoint, OneDrive, Windows Endpoints)
- Resolution - min: 50x50, max: 16000 x 16000

### Limitations

- Only images with machine-typed text are supported.
- Only images uploaded after OCR has been enabled are scanned.
- Only stand-alone images are scanned.
- SharePoint and OneDrive support only the following file types: JPEG, JPG, PNG, and BMP.
- Data loss prevention policy tips aren't supported for images in Exchange.
- Scanning images in compressed/archive files isn't supported.
- If you exclude a path in the endpoint data loss prevention settings, OCR doesn't scan images in those folders.
- When OCR is turned on for Windows devices, the devices start sending messages to the cloud for scanning. The default bandwidth limit is 1024 MB of data per device per day. OCR stops scanning images once this daily limit is reached. If you want to continue scanning images, you can increase the bandwidth limit.

### Help protect sensitive images with Microsoft Purview interactive guide
[Interactive guide](https://mslearn.cloudguides.com/guides/Help%20protect%20sensitive%20images%20with%20Microsoft%20Purview)

---
## Define policy settings for your DLP policy
https://learn.microsoft.com/en-us/training/modules/m365-compliance-information-prevent-data-loss/define-policy-settings

[Interactive guide](https://learn.microsoft.com/en-us/training/modules/m365-compliance-information-prevent-data-loss/define-policy-settings)

2 types of workflows when defining a DLP pollicy:

1. Simple workflow
2. Advanced workflow

### Customize advanced DLP rules

- **Two rules** are defined by default when you use a policy template
	- Low volume
	- High volume
- If you choose the **Custom** option, you must define your own rules

#### Rules includes the following:

- Conditions 
	- Content contains
	- Content is shared from Microsoft 365 
- Exceptions 
- Actions 
	- Restrict access or encrypt the content in Microsoft 365 locations
	- Audit or restrict activities on Windows devices
- User notifications
- User overrides 
- Incident reports
- Additional options

----
## Prepare Endpoint DLP
https://learn.microsoft.com/en-us/training/modules/m365-compliance-information-prevent-data-loss/prepare-endpoint-dlp

### License required: 
- Microsoft 365 E5
- Microsoft 365 A5 (EDU)
- Microsoft 365 E5 compliance
- Microsoft 365 A5 compliance
- Microsoft 365 E5 information protection and governance
- Microsoft 365 A5 information protection and governance

### Onboard devices
Purview -> **Settings** -> **Device Onboarding**

Roles required for onborder:
- Global admin
- Security admin
- Compliance admin

### Onboarding options for Windows 10/11

- Local script
- Group policy
- Microsoft Endpoint Configuration Manager
- Mobile Device Management/Microsoft Intune
- VDI onboarding scripts for non-persistent machines

### Onboarding options for Mac

- Microsoft Intune
- Intune for Microsoft Defender for Endpoint customers
- JAMF Pro
- JAMF Pro for Microsoft Defender for Endpoint customers

### Onboarding using local script

- up to 10 machines only
- local script is meant for testing purposes

Steps:

1. Get the configuration package .zip file (_DeviceComplianceOnboardingPackage.zip_) package from Microsoft Purview compliance portal
2. In the navigation pane, select **Settings > Device** onboarding.
3. In the **Deployment method** field, select **Local Script**.
4. Select **Download package** and save the .zip file.
5. Extract the contents of the configuration package to a location on the device you want to onboard (for example, the Desktop). You should have a file named _DeviceOnboardingScript.cmd_.
6. Open an elevated command-line prompt on the device and run the script:
7. Go to **Start** and type **cmd**.
8. Right-click **Command prompt** and select **Run as administrator**.
9. Type the location of the script file. If you copied the file to the desktop, type: _%userprofile%\Desktop\WindowsDefenderATPOnboardingScript.cmd_
10. Press the **Enter** key or select **OK**.

### Configure global Endpoint DLP settings

Data loss prevention -> Data loss prevention settings -> Endpoint settings
- apply to all existing and new DLP policies that protect content on Windows devices.
- only apply to content impacted by DLP policies

#### Settings:
- Restricted app groups
- Unallowed Bluetooth apps
- Browser and domain restrictions to sensitive items
- Additional settings for Endpoint DLP
- Always audit file activity for devices
- Auto-quarantine file from unallowed apps
- Advanced classification
- Business justification in policy tips

