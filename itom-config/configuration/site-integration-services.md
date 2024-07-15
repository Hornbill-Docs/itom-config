# Site Integration Service
Hornbill's Site Integration Service (SIS) is a Windows NT Server service installed on a server behind an organization's firewall. It runs as a standard Windows NT Service and, once paired with a Hornbill instance, monitors the ITOM Job Queue. The SIS connects to the instance order to retrieve jobs that have been placed on the job queue for execution.

Should jobs be detected during a polling interval, the SIS pulls all information from the paired instance regarding the jobs required to be undertaken on specified devices on the network. There are currently two types of jobs available; Computer Discovery and IT Automation. Automation jobs are powered via pre-built packages provided by Hornbill or can be custom-built using the ITOM Package Creator.

A web interface is provided, which allows for the initial pairing and display of the service's current status. The service is self-updating and will detect if a new version has been made available within the cloud and, if so, download and install it automatically. All communications with the cloud will always be initiated locally from the SIS to the cloud and never from the cloud.

## Technical Considerations
### System Requirements
* OS: Windows Server 2012, 2012 R2, 2016, 2019 or 2022
* RAM: 4GB
* Free Disk Space: 10GB
* CPU load is minimal
* Can be run on virtual as well as physical machines.

### Connectivity
* The SIS communicates with a Hornbill instance using the secure HTTPS protocol.
* Hornbill instances will only respond to a successfully paired SIS.

:::note
Support for communications via a proxy service is not currently available.
:::

### Discovery and Package Deployment
The SIS is capable of discovering the following devices:

* Windows 32/64bit computers that are currently supported by Microsoft
* Unix/Linux/Mac Computers running ssh

Depending on the content of the deployment package, there may be additional OS requirements.

### Firewall Configuration
A Windows firewall rule for Inbound traffic (Local subnet) that allows all TCP traffic into the SIS service executable is created on installation and named:
* Hornbill SIS Server - Context Callback (TCP - In).

The following outbound ports between the SIS server and the cloud instance are required:
* HTTPS TCP 443

The following ports between the SIS server and Managed Devices are required, dependant on which method is adopted to retrieve Inventory details:
* WinRM - TCP 5985
* DCOM - TCP 135
* DCOM - Range of dynamic ports:
    * TCP 49152-65535 (RPC dynamic ports – Windows Vista, 2008 and above)
    * TCP 1024-65535 (RPC dynamic ports – Windows NT4, Windows 2000, Windows 2003)
Site Integration Service Discovery (Dependant on the discovery mode used)
* Active Directory / LDAP
    * TCP Port 389 (Between the SIS and the AD Domain Controller / LDAP Server)
* Secure Shell (ssh)
    * TCP Port 22 (Between the SIS and target devices)

The following is required to support the TCP Ping test used during discovery (only required if the feature is in use):
* ICMP Echo Reply

## Toolbar
* **Refresh**<br>A refresh of the list may be required to display any new devices discovered while you are viewing the list.
* **Show**<br>Displays connectors from the selected group.
* **+ Create Group**<br>(Selectable via the drop-down) allows for the Creation of SIS Groups.
* **Paired / Not Paired**<br>Toggle button that allows the display of Paired or Not Paired SIS.
* **Download Site Integration Server**<br>Downloads the on-premise SIS Installer.
* **Move Selected To...**<br>Moves the selected SIS entries to the selected Group.
* **+**<br>Add a new SIS Connector.
* **Delete**<br>Deletes the selected SIS entries.

List
* **Name**<br>The name of the Connector.
* **Group**<br>The group that the Connector belongs to.
* **Description**<br>The user-provided description for the connector.
* **Service Type**<br>Operating system architecture of the Server hosting the SIS installation.
* **Service State**<br>Toggle to enable or disable the SIS.
* **Online Status**<br>The current status of the link to the SIS service.
* **Service Build**<br>SIS Server build version. Any Service showing an older build may highlight that there is an issue with automatic updates for that service.
* **Last Seen On**<br>This will display the last time there was communication between the Hornbill SIS Service and the SIS Server.

## Creating an SIS Service Profile on the Hornbill Instance
1. Navigate to Administration > Hornbill ITOM > Site Integration Services.
1. Click the NewPackageButton.png button to create a new SIS Connector.
1. Enter the following details:
    * Name - name used to identify the SIS server to the Hornbill Instance.
    * Group - Should be a least one default group, others can be selected via drop down if created previously.
1. Click the Create Site Integration Service button.
1. The Authorisation Code is displayed, and should be recorded for later use.
1. See the next section for details of how to pair your SIS with your Hornbill instance.
1. Should you choose not to complete the pairing at this time, the connector can be found by selecting the Not Paired filter in your list of SIS Connectors.

:::tip
The Authorisation Code is valid for 1 hour. Should the SIS connector and SIS installation not be paired during this time, the key will expire. To generate a new Key, remove the SIS connector and recreate it.
:::

## Downloading and Installing the SIS Service
The Site Integration Service is installed as a Windows NT Service called "ESPSisService"
The Hornbill SIS is installed as a Windows NT Service and will require local administration rights for installation on the target computer.

1. Navigate to: Administration > Hornbill ITOM > Site Integration Services
1. Click the Download Site Integration Server button on the toolbar
1. Locate the downloaded executable (.exe), and double click to begin
1. Click Install
1. Click Ok to Confirm the Installation
1. Close the Install dialog
1. Open the Services mmc console
1. Start the EspSisService if it isn't already running

## Pairing an SIS Server with a Hornbill Instance
Once the EspSisService is running, the process of pairing the service with a Hornbill instance can begin, which will require an Authorization Code. This code will have been provided while creating an SIS connector. See the section Creating an SIS Connector on the Hornbill Instance.
1. Open the Browser and navigate to http://localhost:11117. After a short pause, a prompt for the Instance Id and an Authorization Code appears.
1. Enter the Instance ID and Authorization Code.
1. Click the Pair with Instance button.

The Instance ID and valid Authorization Code are required to pair an SIS with your Hornbill instance. Once paired, the SIS status is reviewable via http://localhost:11117.

## Grouping Site Integration Servers
The creation of groups enables SIS connectors and SIS installations to be logically grouped; each SIS entry must belong to a single group. A "Default" group is provided with the option to create additional groups as required. Generally, the network infrastructure, load balancing and failover requirements will determine the number and grouping of SIS installations.

* **Load Balancing**<br>When more than one server is placed within a group, Jobs sent to the group for processing will be processed by the next available SIS server spreading the load.
* **Failover protection**<br>SIS servers poll the Job queue for available jobs, and thus if a server fails, any other server within the same group will pick the next available job. Any job currently being processed by the SIS server will fail, and the status set accordingly. If the job has already been pushed to a client and executed, then it will potentially be orphaned, and the status set to Timed-Out.

::: tip
If a standalone SIS server fails then all jobs aimed at that server will be left in the Job queue and will not be processed until the server is up and running again.
:::

## Creating a Group
1. From the ITOM page select Site Integration Services
1. Click the Show dropdown
1. Click +Create Group option
1. Enter the New Group Name
1. Click Apply

## Removing an SIS Server Installation
If you want to remove the SIS server from your instance. simply navigate to the SIS servers tab in the admin tool and delete the SIS server you want to remove.

The removal of the SIS server software from your server(s) requires manual steps in order to remove the service and all related files.

1. Open Windows Powershell console as an administrator
1. Enter the following:
    * Stop-Service EspSisService
    * sc.exe delete EspSisService
    * Remove-Item "$env:ProgramFiles\Hornbill\Site Integration Server" -Recurse
    * Remove-Item "$env:ProgramData\Hornbill\Site Integration Server" -Recurse
:::tip
Care should be taken with these steps as they will perform a recursive delete on the two folders specified. Once the service is removed don't forget to remove the SIS entry on the instance.
:::
<!-- https://wiki.hornbill.com/index.php?title=Site_Integration_Services -->