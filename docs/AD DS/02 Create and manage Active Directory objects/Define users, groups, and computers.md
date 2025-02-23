## What are managed service accounts?



### Group types

- Security
- Distribution

### Group scopes


| Type             | Description                                                                                                                                                                                                              | Characteristics                                                                                                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Local**        | use this type of group for standalone servers or workstations, on domain-member servers that are not domain controllers, or on domain-member workstations<br><br>available only on the computer where they exist         | - You can assign abilities and permissions on local resources only, meaning on the local computer.<br><br>- Members can be from anywhere in the AD DS forest.                                    |
| **Domain-local** | primarily to manage access to resources or to assign management rights and responsibilities.<br><br>exist on domain controllers in an AD DS domain, and so, the group’s scope is local to the domain in which it resides | - You can assign abilities and permissions on domain-local resources only, which means on all computers in the local domain.<br><br>- Members can be from anywhere in the AD DS forest           |
| **Global**       | primarily to consolidate users who have similar characteristics<br><br>                                                                                                                                                  | - You can assign abilities and permissions anywhere in the forest.<br><br>- Members can be from the local domain only and can include users, computers, and global groups from the local domain. |
| **Universal**    | use this type of group most often in multidomain networks because it combines the characteristics of both domain-local groups and global groups                                                                          | - You can assign abilities and permissions anywhere in the forest similar to how you assign them for global groups.<br><br>- Members can be from anywhere in the AD DS forest.                   |
