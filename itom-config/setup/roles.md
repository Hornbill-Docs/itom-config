# Roles

ITOM roles are collections of rights that provide access to features and functionality within ITOM. System roles are provided with preset rights that are required for users to perform common job roles. You can also create custom roles.

The [ITOM Application Administrator role](/itom-config/setup/roles#itom-application-administrator) has rights to administer the application, manage application settings, and manage Site Integration Services.

The [ITOM Application User role](/itom-config/setup/roles#itom-application-administrator) has rights to use the application; manage, create, update, and delete Scheduled Jobs; and manage Workflows, Autotasks, Lifecycle Processes, Intelligent Capture, KeySafe, Site Integration Services, Runbooks, Automation Jobs, and Inventory.

## Topics covered

* The roles that are included with ITOM at installation
* How to [create custom roles](/itom-config/setup/roles#adding-custom-roles)

## Before you begin

Understand how [roles are managed](/esp-config/organizational-data/roles).
::: note
 When you assign a role with a privilege level of `user` or higher, a Service Manager subscription license is automatically allocated to that user.
:::

## ITOM Application Administrator

This role provides access to the ITOM settings within Configuration (i.e. to set up Site Integration Services servers or manage application settings).

The ITOM Application Administrator has the `canAdministerITOM` application right, and system rights as follows:

### Configuration

- **Manage Application Settings.**
* **Manage Site Integration Services.**

## ITOM Application User

Assign this role to those users who will use the ITOM application. Users need this role to see the ITOM icon visible on the left sidebar and access the application.

The ITOM Application User has the `canUseITOM` application right, and system rights as follows:

### Data

- **Managed Scheduled Jobs.**
* **Create Scheduled Jobs.**
* **Update Scheduled Jobs.**
* **Delete Scheduled Jobs.**

### Configuration

* **Managed Workflows, Autotasks, Lifecycle Processes.**
* **Manage Intelligent Capture.**
* **manageKeysafe.** Enables access to the KeySafe features for applying credentials to IT automations.
* **Manage Site Integration Services.** Enables the creation and management of Site Integration connectors.
* **Manage Runbooks.** Enables access to the Runbook orchestration features.
* **Manage Automation Jobs.** Enables access to the job queue where IT Automation and Discovery Jobs can be initiated and monitored.
* **Manage Inventory.** Enables access to the inventory where devices can be organized and their managed statuses can be set.

## Adding custom roles

Both the ITOM Application Administrator role and the ITOM Application User role can add user-created roles.

**To add a custom role:**

1. At the bottom of the left-hand menu bar, click the cog icon to open Configuration. (A shortcut is to use CTRL+SHIFT+S on your keyboard.)
1. Select **Hornbill ITOM**.
1. Under *Hornbill ITOM Setup*, select **Roles**.
1. In the list of roles, click **Create New Role**.
1. Give the role a name (Role ID) and description, configure its privilege level and type, then click **Create Role**.
1. In the Application Rights tab, assign administration and user rights, then click **Save Changes**.
1. In the System Rights tab, assign rights to the role, then click **Save**.
1. In the Assigned Users tab, add one or more users to the role.
