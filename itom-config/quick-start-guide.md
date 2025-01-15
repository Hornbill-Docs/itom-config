# Quick Start Guide
The following guide takes you through the steps required to get up and running with ITOM, including installing the Site Integration Service, discovering devices and package execution.

## Before you begin
To follow this guide, you must have the following
* Access to a Hornbill instance.
* A user with the ITOM Administrator role added.
* A Windows computer with a membership to an Active Directory domain, for the installation of the SIS.
* You will also require access to a Windows Domain Administrator account in order to retrieve inventory details via WMI and deploy and execute Package operations.

## Site integration Service (SIS) Installation

Site Integration Service Installation
The SIS's role is to monitor the ITOM Job Queue, download any Jobs targeted to it for device discovery or package deployment and execution. The service is available for download via the Hornbill Instance and can be installed at any time; however, before you can use it, a Site connector must exist on your Hornbill Instance and the SIS paired with it.

<iframe width="400" height="225" src="https://www.youtube.com/embed/Of4i1UOzt_g" title="Quick Guide -  Site Integration Service Installation" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Minimum requirements for the SIS
* OS: Windows Server (64bit) 2012, 2012 R2, 2016 or 2019
* RAM: 2GB
* Free Disk: 10GB

### Adding an SIS Connector
First setup an SIS connector and generate the Authorization key required for the pairing process.

1. From the ITOM page select Site Integration Services.
1. Click the “Add SIS connector” button.
1. Enter your required details:
    * Name - Unique identifer for the SIS Connector.
    * Group - The default group can be used.
1. Click the Create Site Integration Service button.

    ![Authorization Key](/_books/itom-config/images/authorization-key.png)
1. Make a note of the Authorization Key, required for later use during the pairing process.
1. Return to the Site Integration Services list.
1. Select the Not Paired filter.

:::info
The Authorization Key is temporary and will expire after 1 hour. Once expired, the the SIS record will need recreating.
:::

### Download and Installation of the SIS
The installation software is downloaded from the SIS list within the Admin Portal, and should be executed on the server nominated to host the SIS.

1. From the Site Integration Services list, Click the Download Site Integration Server button.
1. Locate and Execute the Downloaded executable.

    ![Installer Page](/_books/itom-config/images/installer-page.png)

1. Click `Install`.
1. Click `OK` to confirm the installation.
1. Close the install dialog. The service will not be started automatically. You must manually start the process or configure it to be automatically started.
1. Open the Services MMC Console.
1. Start the EspSisService.

    ![Start the SisMmcService](/_books/itom-config//images/start-sis-mmc-service.png)

### Server Pairing
Once the service has been started, the pairing process can be completed via the service web page, using the Authorisation Code generated when creating the SIS connector. If you do not have the code or have forgotten it, you can view it from the SIS Connector properties in the Admin Portal. If the code has expired, you will need to remove the existing connector and create a new one to generate a new code.

1. Switch back to the Browser and refresh the page (http://localhost:11117)

    ![SIS Paring with Instance](/_books/itom-config/images/sis-pair-with-instance.png)
1. Enter the instance ID
1. Enter the Authorization Code recorded earlier
1. Click the `Pair with Instance` button

    ![SIS New Home Page](/_books/itom-config/images/sis-new-home-page.png)

After a successful pairing, the status page appears displaying information related to the SIS service, with additional details available via the `Show more` button.

## ITOM Admin Account Requirements
Before a successful Discovery or Automation can be actioned, one or more windows NT accounts will be required. It is recommended that you create a new admin domain account for Windows NT computers and an account with root privileges for Linux / Unix environments.

### Windows NT Accounts
ITOM Admin Credentials will require a Windows NT Administrator account with the following additional rights to be applied:
* Replace a process-level token. (SeAssignPrimaryTokenPrivilege)
* Act as part of the operating system. (SeTcbPrivilege)
Additional user accounts may also require creation. These are dependent on the package used and the context of security needed. Further information is available within the [ITOM package library](/itom-packages/activedirectorygroupmanagement) documentation for each Package under the section Authentication.

### Linux / Unix
ITOM Admin Credentials will require an account with root user privileges when used for Linux or Unix devices. Accounts that require the use of sudo cannot be used.

### Creating a Hornbill KeySafe entry
Once you have created the OS Accounts with the required rights and permissions the account will need to be added to the Hornbill KeySafe in order for it to be used for discovery and IT Automations. The following will guide you through the process of creating a Keysafe entry on your Hornbill Instance, and can be used for adding both Windows and Linux accounts that require username and password entry:

1. From the Hornbill Administration page navigate to (Home > System > Security > KeySafe)
1. Click the `Create New Key` button.
1. Select Type as Username + Password.
    :::info
    Ensure that the KeySafe type is Username + Password and not Username + Password + Pre-Shared Key, as the entry will not be visible within ITOM. SSH Private Key entry can also be used for devices that utilize ssh and public key authentication has been configured.
    :::

    ![Key Safe User Password Form](/_books/itom-config/images/key-safe-user-password-form.png)
1. Enter the following details:
    * Title: Network Admin
    * Domain Username: (example: DOMAIN\Username or username@domain)
    * Password:
1. Click `Create Key`

## Configuring a Discovery Job
To view the properties of a device or to execute IT Automations on a device, it must exist within the Inventory. The population of the inventory is undertaken by the execution of one or more discovery jobs. 

<iframe width="400" height="225" src="https://www.youtube.com/embed/kVsPDKVqvH0" title="Quick Guide - Configuring a Discovery Job" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

In this example, you will configure a discovery using Windows Active Directory, connecting to devices via WinRM to retrieve the properties.
1. Navigate to the ITOM Job Queue (Home > ITOM > Job Queue).
1. Click the `Create New` button and select Discovery Job.
1. Enter the following details:
    * Name: AD Discovery using WinRM
    * Site Target: SIS Demo
    * Protocol: WinRM
    * Discovery Mode: Active Directory
    * Container: <windows domain> (example:hornbill.qa)
    * Admin Credentials: <your keysearch entry>
1. Click the `Create` button.

![AD Discovery Monitoring](/_books/itom-config/images/monitor-discovery-job.png)

* **Monitor**. You can monitor the progress of the job via the monitor, and once the job has completed, the Console Output and Debug log are displayed. 
* **Console Output**. This provides a view of the raw output from the process execution on the target device. 
* **Debug Log**. If errors are identified during the process execution, the debug log will provide additional information to help diagnose the failure. Success here implies that the Discovery process did not fail and that all devices were detected and were able to be accessed.

## Inventory Viewer
The Inventory View allows you to Browse and Manage all discovered devices; from here, you can remove unwanted devices and modify a device's Managed status. When a device is initially discovered, it will be classified as Un-Managed, and only basic properties will be visible. IT Automations will not be able to be executed on these devices until it is classified as a Managed Device, allowing access to its full properties.

<iframe width="400" height="225" src="https://www.youtube.com/embed/4xIC0sutMNQ" title="Quick Guide - Managing the Inventory" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Registering a Device as Managed
1. Navigate to the ITOM Inventory (Home > ITOM > Inventory Viewer)
1. Select `All Un-Managed Inventory`

    ![Inventory Show Menu](/_books/itom-config/images/inventory-show-menu.png)
1. Click on the Name of an Un-Managed inventory Item. Initially all discovered devices will appear as Un-Managed devices with only basic properties visible.

    ![Un-Managed Inventory](/_books/itom-config/images/un-managed-inventory.png)
    :::info
    Pressing the `Set As Managed` button links a subscription to the device, which is consumed for a minimum of 30 days; as stated in the message provided, once confirmed, the device's properties will become visible.
    :::
1. Use the Breadcrumbs to return to the Inventory Viewer

### Registering Multiple Devices as Managed
Setting devices as Managed individually is not always desirable, a more efficient method is to set multiple devices at the same time.

1. Click the checkbox next to the heading Name to select All Discovered Devices

    ![Select All Inventory](/_books/itom-config/images/select-all-inventory.png)

    Individual devices can be selected / deselected by clicking the check box adjacent to each entry.
1. Click the The Set As Managed button on the toolbar.
1. Click `Yes` to confirm.
1. Select All Managed Inventory

### Inventory Properties
1. Click on the name of a managed inventory item to open the properties.

![Managed Inventory Properties](/_books/itom-config/images/managed-inventory-properties.png)

## Installed Packages
Before any IT Automations can be configured, the required packages will need to be available and listed in the Installed Packages list. There are few ways for Packages to be installed depending on your subscription, including manually uploading or creating your own package from scratch. The Package Library is a more convenient method and contains several packages produced and supported by Hornbill, from which you can install, update or remove.

<iframe width="400" height="225" src="https://www.youtube.com/embed/44fX9QyMRYw" title="Quick Guide Managing Installed Packages" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Package Library
The packages that are available will depend on your subscription, all Windows Management packages will be available as standard. The following steps will take you through the installation of the Active Directory and Windows Management packages, available to all subscription levels:

1. Navigate to (Home > ITOM > Installed Packages)
1. Click the Package Library Package button

    ![Package Libarary](/_books/itom-config/images/package-library-list.png)
1. Click `Install` on both the Active Directory Group and User Management packages
1. Click `Install` on Windows Disk Cleanup
1. Click `Close`

## IT Automation Job
Once packages have been installed, IT Automation jobs can execute specific actions on individual or multiple devices. In the following examples, the steps will guide you through configuring an Automation on both a single device and across multiple devices.

<iframe width="400" height="225" src="https://www.youtube.com/embed/t4MiS0USsB4" title="Quick Guide IT Automation Job" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Single Computer
The following steps will guide you through the process of configuring and executing a IT Automation Job to execute an action from the Windows AD Managemnet package to create a new user within an Active Directory Domain.
1. Navigate to (Home > ITOM > Job Queue)
1. Click the Create New button, and select IT Automation
1. Enter Name: Create User: Andy Smith
1. Click the Installed Packages button
1. Select private:hornbill > Managing Active Directory > Active Directory User Management
1. Click Apply. Operation should be defaulted to Create.
1. Set Site Target to Server and select your SIS Connector
1. Set Target Device to Inventory and select your Domain Controller. You can also select any discovered device that is running the Remote Server Administration Tools (RSAT).
1. Set Admin Credentials to Network Admin
1. Enter the following Details:
    * Given Name  : Andy
    * Surname  : Smith
    * SamAccountName  : AndySmith
    * AccountPassword : Passw0rd
    * Display Name  : Andy Smith
    * Name  : Andy Smith
1. Click Create

The monitor tab shows the raw output from the job. The last entry will display "The job was executed successfully". However, that only confirms that the process executed and not if the action successfully created the user. 

![Monitor-AD User Creation](/_books/itom-config/images/monitor-ad-create-user.png)

The information showing the outcome of the create operation will be within the block of text output in white and will vary depending on the package. In this example, the text {{SISJobOutputParameterStart:outcome}}OK{{SISJobOutputParameterEnd}} shows that the outcome was successful and the action created the user. In many cases, it may difficult to locate the relevant output parameters to identify the outcome. In these cases, the Package Details section provides a list of both input and output parameters in a user-friendly manner:

![Job Package Details](/_books/itom-config/images/job-package-details.png)

### Multiple Computers
You can perform a package operation across several devices using a single Job when the target is specified using an Inventory List. The list must already exist and populated with one or more devices; they are created and managed via the ITOM Inventory. The following steps will guide you through the process of configuring an IT Automation that executes an action from the Windows Management package to restart the print service on multiple Windows devices.

1. Navigate to (Home > ITOM > Job Queue)
    ![Job Queue](/_books/itom-config/images/job-queue.png)
1. Click the `Create New` button, and select IT Automation
1. Enter Name: Restart Print Spooler
1. Click the Installed Packages button
1. Select provate:hornbill > Managing Windows Devices > Windows Management (...)
1. Click Apply
1. Set Operation to: Service - Restart
1. Set Site Target to Server and select an Instance
1. Set Target Device to: Inventory and select a Device
1. Set Admin Credentials to Network Admin
1. Click `Create`
1. Click on a Job Name to view the Individual Child Job
1. Click Parent Link in the Summary to Return to Parent Job

    ![Job Properties Link](/_books/itom-config/images/job-properties-link.png)

## Job Scheduling
The versatile Job scheduler allows you to configure an IT Automation, Discovery or Runbook Process to execute to a specified schedule. It is typically used for Jobs that require execution more than once at specific times and days, such as backups, maintenance, and reporting scripts.

<iframe width="400" height="225" src="https://www.youtube.com/embed/KeC5KNmQajg" title="Quick Guide Job Scheduling" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### Discovery
Device discovery is a good candidate for scheduled jobs, and is usually scheduled to execute daily, following steps will guide you through process of scheduling a Windows AD discovery Job.

1. Navigate to Home > ITOM > Job Scheduling
1. Click the Create New button, and Select Discovery Schedule
1. Enter the following details:
    * Name: AD Discover
    * Schedule: Run Every Period
    * Every (n) Minutes: 60
    * Description: Scheduled AD Discovery
    * Site Target: Server | SIS Instance
    * Protocol: DCOM
    * Discovery Mode: Active Directory
    * Container: horbnbill.edu
    * Admin Credentials: Network Admin
1. Ensure Next Scheduled Date and Time is set to a couple of minutes in the future
1. Click Enable Schedule
1. Navigate to (Home > ITOM > Job Scheduling)
    ![Scheduled Job List](/_books/itom-config/images/scheduled-job-list.png)
1. Wait for the Job schedule Time, and Click on the AD Discovery Job Name
1. Click the Job History
1. Click on the Scheduled AD Discover Name

### IT Automation
IT Automation jobs can be scheduled to execute any package operation and are most commonly used for tasks executed regularly to a specific schedule, such as maintenance type operations.

#### Windows Disk Cleanup
The Windows Disk Cleanup package is commonly used on a regular basis to clear down temporary files, unused system files and various other files from a Windows computer. The following will guide you through the process of setting up a schedule to execute the package operation on a weekly basis.

1. Navigate to Home > ITOM > Job Scheduling
1. Click the Create New button, and Select IT Automation Schedule
1. Enter the following Schedule details:
    * Name: Windows Disk Cleanup
    * Schedule: Run daily
1. Enter the following IT Automation Job Settings:
    * Package: private:hornbill > Disk Cleanup > Windows Disk Cleanup
    * Site Target: SIS Server
    * Target Device: List| Test Servers
    * Admin Credentials: Network Admin
    * Reference: Demo Job
1. Set the following Operation Parameters to True:
    * InternetCacheFiles, Recycle Bin, and Temporary Files
1. Ensure Next Scheduled Date and Time is set to a couple of minutes in the future
1. Click Enable Schedule
1. Navigate to (Home > ITOM > Job Scheduling)
1. Wait for the Job schedule Time, and Click on the Job Name: Windows Disk Cleanup
1. Click Job History

    ![Schedule Job History](/_books/itom-config/images/scheduled-job-history.png)
1. Click on the Job Name: Windows Disk Cleanup (with the highest Job Id)

    ![Scheduld Job Properteis](/_books/itom-config/images/scheduled-job-properties.png)
1. Review the list of jobs, and confirm that all are successful
1. Click on the Name of any Job entry in the list and review the Details
1. To return to the parent Click the link shown in the Summary section

<!-- https://wiki.hornbill.com/index.php?title=ITOM_Quick_Start_Guide -->