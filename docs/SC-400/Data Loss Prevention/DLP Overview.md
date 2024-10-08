
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

