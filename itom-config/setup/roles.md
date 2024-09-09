# Roles
ITOM roles are collections of rights that provide access to features and functionality within ITOM. System roles are provided with preset rights that are required for users to perform common job roles. You can also create custom roles.

The [ITOM Application Administrator role](/itom-config/setup/roles#itom-application-administrator) has rights to administer the application, manage application settings, and manage Site Integration Services.

The [ITOM Application User role](/itom-config/setup/roles#itom-application-administrator) has rights to use the application; manage, create, update, and delete Scheduled Jobs; and manage Workflows, Autotasks, Lifecycle Processes, Progressive Capture, KeySafe, Site Integration Services, Runbooks, Automation Jobs, and Inventory. 


## Topics covered
* The roles that are included with ITOM at installation.
* How to create custom roles

## Before you begin
* Understand how [roles are managed](/esp-config/organizational-data/roles).
::: note
 When you assign a role with a privilege level of `user` or higher, a Service Manager subscription license is automatically allocated to that user.
:::

## ITOM Application Administrator

This role provides access to the ITOM settings within Configuration (i.e. to set SIS Servers or Application Settings).

|Application rights|System rights|
|-|-|
|`canAdministerITOM` |**Configuration:**<br />Manage Application Settings<br /> Manage Site Integration Services |

## ITOM Application User
Assign to those users that will be using the ITOM application. You need this role to make the ITOM icon visible on the left sidebar.

|Application rights|System rights|
|-|-|
|`canUseITOM` |**Data:** <br />Managed Scheduled Jobs <br /> Create Scheduled Jobs <br /> Update Scheduled Jobs <br /> Delete Scheduled Jobs <br /> <br /> **Configuration:** <br /> Manage Workflows, Autotasks, Lifecycle Processes <br /> Manage Progressive Capture <br /> manageKeysafe <br /> Manage Site Integration Services <br /> Manage Runbooks <br /> Manage Automation Jobs <br /> Manage Inventory |

## Rights
|Right|Description|
|-|-|
|Manage Automation Jobs|Enables access to the job queue where IT Automation and Discovery Jobs can be initiated and monitored|
|Manage Inventory|Enables access to the inventory where devices can be organized and there managed status set|
|Manage Runbooks|Enables access to the Runbook orchestration features|
|Manage Site Integration Services|Enables the creation and management Site Integration Connectors|
|Manage Site Integration Packages|Enables the ability to create and manage custom packages|

## System roles
These additional system roles are required for users that need to create workflows or KeySafe entries.
|Role|Description|
|-|-|
|Business Process Manager|Required for access to the orchestration features provided by Runbooks (powered by the BPM)|
|manageKeysafe|Required for access to the Keysafe features in order to apply credentilas to IT Automations|

<!-- https://wiki.hornbill.com/index.php?title=ITOM_Roles_and_Rights-->