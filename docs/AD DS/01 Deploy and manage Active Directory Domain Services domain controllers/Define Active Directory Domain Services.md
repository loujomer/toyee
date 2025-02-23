
The AD DS database is the central store of all the domain objects, such as user accounts, computer accounts, and groups.

## What are the logical components?

are structures that you use to implement an AD DS design that is appropriate for an organization

| **Logical component** | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Partition             | A partition, or naming context, is a portion of the AD DS database. Although the database consists of one file named Ntds.dit, different partitions contain different data. <br><br>For example:<br>**schema partition** contains a copy of the Active Directory schema<br>**configuration partition** contains the configuration objects for the forest<br>**domain partition** contains the users, computers, groups, and other objects specific to the domain<br><br>Active Directory stores copies of partitions on multiple domain controllers and updates them through directory replication. |
| Schema                | A schema is the set of definitions of the object types and attributes that you use to define the objects created in AD DS.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Domain                | A domain is a logical administrative container for objects such as users and computers. .                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Domain tree           | A domain tree is a hierarchical collection of domains that share a common root domain and a contiguous Domain Name System (DNS) namespace.                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Forest                | A forest is a collection of one or more domains that have a common AD DS root, a common schema, and a common global catalog.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| OU                    | An OU is a container object for users, groups, and computers that provides a framework for delegating administrative rights and administration by linking Group Policy Objects (GPOs).                                                                                                                                                                                                                                                                                                                                                                                                              |
| Container             | A container is an object that provides an organizational framework for use in AD DS. You can use the default containers, or you can create custom containers. You can't link GPOs to containers.                                                                                                                                                                                                                                                                                                                                                                                                    |

# AD DS forests and domains

Forest -  a collection of one or more AD DS trees  that contain one or more AD DS domains
Domain - a logical administrative container for objects

Domains in a forest share:
- A common root.
- A common schema.
- A global catalog.

<img src="https://learn.microsoft.com/en-us/training/wwl-azure/deploy-manage-active-directory-domain-services-domain-controllers/media/forests-domain-trees-7b35ff29.jpg" alt="forest" />
## What is an AD DS forest?

The forest **root domain** is the first domain that you create in the forest.
The forest root domain contains the following objects that don't exist in other domains in the forest: 

- schema master role (can be moved to other dc in the forest)
- domain naming master role (can be moved to other dc in the forest)
- Enterprise Admins group
- Schema Admins group

An AD DS forest is often described as:
- **A security boundary.** 
	- By default, no users from outside the forest can access any resources inside the forest
	- All the domains in a forest automatically trust the other domains in the forest
- **A replication boundary.**
	- An AD DS forest is the replication boundary for the configuration and schema partitions in the AD DS database.
	- Therefore, organizations that want to deploy ==applications with incompatible schemas must deploy additional forests==
	- The forest is also the replication boundary for the global catalog. The global catalog makes it possible to find objects from any domain in the forest.
<img src="https://learn.microsoft.com/en-us/training/wwl-azure/deploy-manage-active-directory-domain-services-domain-controllers/media/forests-ee54e583.png" />
## What is an AD DS domain?

An AD DS domain is a logical container for managing user, computer, group, and other objects.

The AD DS database stores all domain objects, and each domain controller stores a copy of the database.

The following objects exist in each domain (including the forest root):

- Relative ID (RID) master role.
- Infrastructure master role.
- Primary domain controller (PDC) emulator master role.
- Domain Admins group.

<img src="https://learn.microsoft.com/en-us/training/wwl-azure/deploy-manage-active-directory-domain-services-domain-controllers/media/domain-9e029d0c.png" />
An AD DS domain is often described as:

- **A replication boundary**
	- When you make changes to any object in the domain, the domain controller where the change occurred replicates that change to all other domain controllers in the domain
	- 
- **An administrative unit.**


An AD DS domain provides:

- Authentication
- Authorization.

## What are trust relationships?

The trusts that create automatically in the forest are all transitive trusts, which means that if domain A trusts domain B, and domain B trusts domain C, then domain A trusts domain C.

| **Trust type**                 | **Description**               | **Direction**        | **Description**                                                                                                                                                                                                                           |
| ------------------------------ | ----------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Parent and child               | Transitive                    | Two-way              | When you add a new AD DS domain to an existing AD DS tree, you create new parent and child trusts.                                                                                                                                        |
| Tree-root                      | Transitive                    | Two-way              | When you create a new AD DS tree in an existing AD DS forest, you automatically create a new tree-root trust.                                                                                                                             |
| *External*                     | *Nontransitive*               | *One-way or two-way* | *External trusts enable resource access with a Windows NT 4.0 domain or an AD DS domain in another forest. You also can set these up to provide a framework for a migration.*                                                             |
| *Realm*                        | *Transitive or nontransitive* | *One-way or two-way* | *Realm trusts establish an authentication path between a Windows Server AD DS domain and a Kerberos version 5 (v5) protocol realm that implements by using a directory service other than AD DS.*                                         |
| Forest (complete or selective) | Transitive                    | One-way or two-way   | Trusts between AD DS forests allow two forests to share resources.                                                                                                                                                                        |
| Shortcut                       | Nontransitive                 | One-way or two-way   | Configure shortcut trusts to reduce the time taken to authenticate between AD DS domains that are in different parts of an AD DS forest. No shortcut trusts exist by default, and an administrator must create them if they are required. |


# Deploy AD DS domain controllers

Domain controllers authenticate all users and computers in a domain.

## Deploy AD DS domain controllers in an on-premises environment


2 step process of domain controller deployment:
1. install the binaries necessary to implement the domain controller role
2. configure AD DS role


# Manage AD DS operations masters

Active Directory Domain Services (AD DS) uses a **multiple-master** process to copy data between domain controllers, and automatically implements a conflict resolution algorithm that remediates simultaneous, conflicting updates.

However, certain operations can be performed only by a specific role, on a specific domain controller.

## What are AD DS operations masters?

AD DS operation master roles are responsible for performing operations that are not suitable for a multiple-master model.

A domain controller that has one of these roles is an **operations master**. An operations master role is also known as a Flexible Single Master Operation (FSMO) role.

There are five operations master roles:

- Schema master
- Domain-naming master
- Infrastructure master
- RID master
- PDC emulator master

By default, the first domain controller installed in a forest **hosts all five roles**.

However, you can transfer these roles after deploying additional domain controllers. 

==When performing operations master-specific changes, you must connect to the domain controller with the role.==

The five operations master roles have the following distribution:

- Each forest has one schema master and one domain naming master.
- Each AD DS domain has one RID master, one infrastructure master, and one primary domain controller (PDC) emulator.

### Forest operations masters

A forest has the following operations master roles - **Get-ADForesst**

* **Domain naming master**. - This is the domain controller that you must contact when you add or remove a domain or make domain name changes.
* **Schema master.** - This is the domain controller in which you make all schema changes.

### Domain operations masters

A domain has the following operations master roles - **Get-ADDomain**

- RID master
	- Whenever you create a security principal such as a user, computer, or group in AD DS, the domain controller where you created the object assigns the object a unique identifying number known as a **security ID (SID)**. 
	- To ensure that no two domain controllers assign the same SID to two different objects, the ==RID master allocates blocks of RIDs to each domain controller within the domain to use when building SIDs.==
	- If the RID master is unavailable, you might experience difficulties adding security principals to the domain. 
	- Also, as domain controllers use their existing RIDs, they eventually run out of them and are unable to create new objects.
- Infrastructure master
	- This role maintains interdomain object references, such as when a group in one domain has a member from another domain. In this situation, the infrastructure master manages maintaining the integrity of this reference. 
	- For example, when you review an object's Security tab, the system references the listed SIDs and translates them into names. In a multiple-domain forest, the infrastructure master updates references to SIDs from other domains with the corresponding security principal names.
	- If the infrastructure master is unavailable, domain controllers that are not global catalogs will not be able to perform translation of SIDs security principal names.
	- The ==infrastructure master role should not reside on the domain controller that's hosting the global catalog role== unless every domain controller in the forest is configured to serve as a global catalog. In this case, the infrastructure master role is not necessary because every domain controller knows about every object in the forest.
- PDC emulator master
	- The domain controller that is the PDC emulator master serves as the time source for the domain.
	- The PDC emulator master in each domain in a forest synchronizes their time with the PDC emulator master in the forest root domain.
	- You set the PDC emulator master in the forest root domain to synchronize with a reliable external time source
	- Additionally, by default, changes to Group Policy Objects (GPOs) are by default written to the PDC Emulator master.
	- The PDC emulator master is also the domain controller that receives urgent password changes
	- If a user's password changes, the domain controller with the PDC emulator master role receives this information immediately.
	- This means that if the user tries to sign in, the domain controller in the user's current location will contact the domain controller with the PDC emulator master role to check for recent changes
	- This will occur even if a domain controller in a different location that had not yet received the new password information authenticated the user.
	- If the PDC emulator master is unavailable, users might have trouble signing in until their password changes have replicated to all the domain controllers.

## Manage AD DS operations masters

transferring the role - planned transfer
seizing the role - emergency transfer

### Transfer operations master roles

==You can transfer operations master roles by using the AD DS snap-ins== that the following table lists. You cannot use AD DS snap-ins to seize operations master roles. Instead, you must use either the **ntdsutil.exe** command-line tool or Windows PowerShell to seize roles.

|**Role**|**Snap-in**|
|---|---|
|Schema master|Active Directory Schema|
|Domain-naming master|Active Directory Domains and Trusts|
|Infrastructure master|Active Directory Users and Computers|
|RID master|Active Directory Users and Computers|
|PDC emulator master|Active Directory Users and Computers|

Seizing a role using PowerShell:

`Move-ADDirectoryServerOperationsMasterRole -Identity "<servername>" -OperationsMasterRole "<rolenamelist>" -Force

**servername**: The name of the target domain controller to which you are transferring one or more roles.
**rolenamelist**: A comma-separated list of AD DS role names to move to the target server.
**-==Force**==: An optional parameter that you include to seize a role instead of transferring it.

Video here: [Manage Active Directory Domain Services operations masters - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/deploy-manage-active-directory-domain-services-domain-controllers/6-operations-masters)

