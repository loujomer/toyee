# Assign Azure roles

Azure role-based access control (**Azure RBAC**) is the authorization system you use to manage access to Azure resources.

Primary steps to follow when assigning an Azure role:

- Who needs access?
	- user
	- group
	- service principal
	- managed identity
- Select the right role.
	Built-in Azure roles:
	- Owner
	- Contributor
	- Reader
	- User Access Administrator
	- Other task specific roles, like Virtual Machine Contributor, can be assigned
- Identify what level to assign the role (the Scope)
	- management group
	- subscription
	- resource group
	- resources
- Confirm the currently logged in user has the rights need to assign the Azure role.
- Assign the role

# Create and configure managed identities

- Managed identities provide an automatically managed identity in Microsoft Entra ID for applications to use when connecting to resources.
- Applications can use managed identities to obtain Microsoft Entra tokens without having to manage any credentials.
- Always remember that **managed identities are assigned to an application**. So, you need to configure and manage the identity within the services they're being used.
- You only assign the minimum privileges that the managed identity needs.

### Benefits of using managed identities
- You don't need to manage credentials. Credentials aren’t even accessible to you.
- You can use managed identities to authenticate to any resource that supports Microsoft Entra authentication, including your own applications. 
- Managed identities can be used without any extra cost.

### Types of managed identity

- **System-assigned**
	- an identity is created in Microsoft Entra ID
	- The identity is tied to the lifecycle of that service instance
- **User-assigned**
	- the identity is managed separately from the resources that use it.

### Managed identity in Azure portal for an App Service

<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-access-management-for-azure-resources/media/managed-identity-azure-portal-87c78e53.png">

---
----
# Access Azure resources with managed identities

### Add access to other resources

After you've enabled managed identity on an Azure resource, you might need to grant access to more resource.

- Navigate to the desired resource on which you want to modify access control.
- Select Access control (IAM).
- Select Add > Add role assignment to open the Add role assignment page.
- Pick the Owner, Contributor, or Reader based on the least privilege rules for your applications needs.
- Select the managed identity you want assigned.

# Analyze Azure role permissions

There are two primary places where permission can be assigned:
- at a user or
- group level. 

However, they all pass down to the user at the final point.

# Configure Azure Key Vault RBAC policies

You can grant access to Azure Key Vault using either:
- role-based access control (RBAC)
- Key Vault access policies

### Assign a Key Vault access policy

- A Key Vault access policy determines whether a user, application, or group, can perform operations on Key Vault secrets, keys, and certificates.
- max of  **1024** access policy entries

1. Open **Key Vault** in the Azure portal.
2. Select your key vault or create a new one.
3. From the menu select **Access policies**, then select **+ Add Access Policy**.
4. Use the dialog to assign the specific permission you want service principal to have.

### Assign a Key Vault access using Role-based access control (RBAC)

1. Enable role-based access control in your key vault.
<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-access-management-for-azure-resources/media/key-vault-role-based-access-fa8cd616.png">

2. Open key vault **Identity and Access (IAM)** from the menu
<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-access-management-for-azure-resources/media/key-vault-assign-role-589cce50.png">

# Retrieve objects from Azure Key Vault

Open your key vault, then open the secret you created. Select the **Show secret value** button.
<img src="https://learn.microsoft.com/en-us/training/wwl-sci/implement-access-management-for-azure-resources/media/key-vault-view-secret-9b36f3cf.png">

# Explore Microsoft Entra Permissions Management

- Microsoft Entra Permissions Management is a **cloud infrastructure entitlement management** (CIEM) solution
- The software detects, automatically right-sizes, and continuously monitors unused and excessive permissions.

### Key use cases for Microsoft Entra Permissions Management

Discover 
Remediate
Monitor