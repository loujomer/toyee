
* The scope set for a label establishes its use
* Applications use the defined scope to decide if they should reveal a label to users.

**information protection**
* Assigning sensitivity labels to items for visual marking and protection through rights management-based encryption

**container management**
* Applying labels to containers


##### Scopes
1. Information protection only (file, email, meetings).
2. Container management only (site, unifiedgroup).
3. Both information protection and container management


* Sensitivity labels support manual (explicit assignment) by users or automatic assignment by policy (implicit assignment). 
* A label assigned by an automatic process cannot replace a label assigned by a user.

* Once applied, the labels are persistent and remain in place for the lifetime of items. 
* Only administrators or people with author (including co-owner) rights can remove sensitivity labels from items


#### Retention and Sensitivity

**Retention labels**

* only valid within the Microsoft 365 tenant that owns the label. 
* can be the default for a container like a SharePoint site, but they have no management function.

**Sensitivity labels**
* persistent and remains with an item until removed by someone with the right to do so.
* both retention and sensitivity labels can be applied **manually** or **automatically**


#### Sensitivity labels support:
1. Items
2. Containers
	* Applying labels to containers does not protect the items stored in the containers
	* the label settings protect content by controlling access to the container where you can store content.
	1. Teams
	2. Microsoft 365 Groups
	3. SharePoint Online sites
4. Schematized data assets (preview)




# Apply sensitivity labels automatically

[🆕 Service Side Auto-labeling - Microsoft Purview Customer Experience Engineering (CxE)](https://microsoft.github.io/ComplianceCxE/playbooks/service-side-auto-labeling/)

##### Client-side labeling when users edit documents or compose (also reply or forward) emails.
1. recommend a label, or
2. automatically apply a label




## Requirements for configuring Service-side Auto-labeling

1. Auditing for Microsoft 365 must be turned on.. [Turn audit log search on or off](https://docs.microsoft.com/en-us/microsoft-365/compliance/turn-audit-log-search-on-or-off?view=o365-worldwide).
2. To view file or email contents in the source view, you must have the **Data Classification Content Viewer role**, which is included in the Content Explorer Content Viewer role group, or Information Protection and Information Protection Investigators role groups. Without the required role, you don't see the preview pane when you select an item from the Items to review tab. Global admins don't have this role by default.

### To auto-label files in SharePoint and OneDrive:
1. You have [enabled sensitivity labels for Office files in SharePoint and OneDrive](https://docs.microsoft.com/en-us/microsoft-365/compliance/sensitivity-labels-sharepoint-onedrive-files?view=o365-worldwide).
2. At the time the auto-labeling policy runs, the file mustn't be open by another process or user. A file that's checked out for editing falls into this category.